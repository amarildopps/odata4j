## Example: Simple In-Memory Producer ##

### Add odata4j Library ###
Add the `odata4j-<version>-nojpabundle.jar` file to `WEB-INF/lib`. When using CXF, `odata4j-<version>-cxfbundle.jar` has to be used.

### Create Producer Factory ###
This sample factory creates an in-memory producer that exposes thread information of the running JVM. Put the compiled class into `WEB-INF/classes`.
```
package org.odata4j.tomcat;

import java.util.Properties;

import org.core4j.Enumerable;
import org.core4j.Func;
import org.core4j.Funcs;
import org.odata4j.producer.ODataProducer;
import org.odata4j.producer.ODataProducerFactory;
import org.odata4j.producer.inmemory.InMemoryProducer;

public class ExampleProducerFactory implements ODataProducerFactory {

  @Override
  public ODataProducer create(Properties properties) {
    InMemoryProducer producer = new InMemoryProducer("example");

    // expose this jvm's thread information (Thread instances) as an entity-set called "Threads"
    producer.register(Thread.class, Long.class, "Threads", new Func<Iterable<Thread>>() {
      public Iterable<Thread> apply() {
        ThreadGroup tg = Thread.currentThread().getThreadGroup();
        while (tg.getParent() != null)
          tg = tg.getParent();
        Thread[] threads = new Thread[1000];
        int count = tg.enumerate(threads, true);
        return Enumerable.create(threads).take(count);
      }
    }, Funcs.method(Thread.class, Long.class, "getId"));

    return producer;
  }
}
```

### Edit Deployment Descriptor ###
Edit the `WEB-INF/web.xml` file.

#### Using Jersey ####
```
<?xml version="1.0" encoding="utf-8"?>
<web-app xmlns="http://java.sun.com/xml/ns/javaee"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://java.sun.com/xml/ns/javaee
    http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd"
  version="3.0">

  <!-- Servlet 1: Expose the OData service endpoint -->
  <servlet>
    <servlet-name>OData</servlet-name>
    <servlet-class>com.sun.jersey.spi.container.servlet.ServletContainer</servlet-class>
    <init-param>
      <param-name>javax.ws.rs.Application</param-name>
      <param-value>org.odata4j.jersey.producer.resources.ODataApplication</param-value>
    </init-param>
    <load-on-startup>1</load-on-startup>
  </servlet>
  <servlet-mapping>
    <servlet-name>OData</servlet-name>
    <url-pattern>/example.svc/*</url-pattern>
  </servlet-mapping>

  <!-- Servlet 2: Enable crossdomain access for browser clients -->
  <servlet>
    <servlet-name>CrossDomain</servlet-name>
    <servlet-class>com.sun.jersey.spi.container.servlet.ServletContainer</servlet-class>
    <init-param>
      <param-name>javax.ws.rs.Application</param-name>
      <param-value>org.odata4j.producer.resources.RootApplication</param-value>
    </init-param>
    <load-on-startup>1</load-on-startup>
  </servlet>
  <servlet-mapping>
    <servlet-name>CrossDomain</servlet-name>
    <url-pattern>/*</url-pattern>
  </servlet-mapping>

</web-app>
```
  * Instead of specifying parameter **javax.ws.rs.Application** it is still possible to use **com.sun.jersey.config.property.resourceConfigClass**.
  * When not using Jersey-specifc configuration (see below), the generic **org.odata4j.producer.resources.DefaultODataApplication** can be set.

#### Using CXF ####
```
<?xml version="1.0" encoding="utf-8"?>
<web-app xmlns="http://java.sun.com/xml/ns/javaee"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://java.sun.com/xml/ns/javaee
    http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd"
  version="3.0">

  <!-- Servlet 1: Expose the OData service endpoint -->
  <servlet>
    <servlet-name>OData</servlet-name>
    <servlet-class>org.apache.cxf.jaxrs.servlet.CXFNonSpringJaxrsServlet</servlet-class>
    <init-param>
      <param-name>javax.ws.rs.Application</param-name>
      <param-value>org.odata4j.producer.resources.DefaultODataApplication</param-value>
    </init-param>
    <load-on-startup>1</load-on-startup>
  </servlet>
  <servlet-mapping>
    <servlet-name>OData</servlet-name>
    <url-pattern>/example.svc/*</url-pattern>
  </servlet-mapping>

  <!-- Servlet 2: ... same as above ... -->

</web-app>
```

### Specify Producer Factory ###
There are two possibilities to specify the OData producer factory. It can be either set using a system property or added as an additional initialization parameter to the deployment descriptor.

The system property has to be set before starting Tomcat (e.g. using the CATALINA\_OPTS environment variable):
> `-Dodata4j.producerfactory=org.odata4j.tomcat.ExampleProducerFactory`
The initialization parameter requires the Jersey-specifc application **org.odata4j.jersey.producer.resources.ODataApplication** and is added to the corresponding `<servlet>` tag:
```
<init-param>
  <param-name>odata4j.producerfactory</param-name>
  <param-value>org.odata4j.tomcat.ExampleProducerFactory</param-value>
</init-param>
```

### Run the Example ###
Start Tomcat and point your client (browser) to the following URL:
> `http://<tomcat-server>:<port>/<context-name>/example.svc/`