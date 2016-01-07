# Introduction #

Since odata4j version 0.6 the project was refactored to support multiple JAX-RS implementations. As result the maven project structure is now multi module having a implementation independent core module and optional modules for Jersey (Sun) and CXF (Apache) dependent implementations.

# Details #

Maven Module Structure:

<img src='http://wiki.odata4j.googlecode.com/hg/odata4j%20-%20modules.png' />

| **Module Name** | **Description** |
|:----------------|:----------------|
| odata4j         | Parent module including all sub modules |
| odata4j-core    | Producer and consumer core classes |
| odata4j-jersey  | Jersey dependent producer and consumer implementation |
| odata4j-cxf     | CXF dependent producer and consumer implementation |
| odata4j-examples | OData producer and consumer examples |
| odata4j-fit     | Full integration tests using Jersey and CXF implementations |
| odata4j-dist    | Module for packing distribution packages |

# Maven Build #

Building the project:

`> mvn clean install`

Building and deploying a release (with code signing):

`> mvn clean deploy -P release.build`

Building Eclipse project files (.project &.classpath):

`> mvn eclipse:clean eclipse:eclipse`

Load generated projects into Eclipse using import wizard.

# Maven POM Dependencies #

Using odata4j with Jersey:

```
<dependency>
  <groupId>org.odata4j</groupId>
  <artifactId>odata4j-core</artifactId>
  <version>0.6.0</version>
  <type>pom</type>
</dependency>
<dependency>
  <groupId>org.odata4j</groupId>
  <artifactId>odata4j-jersey</artifactId>
  <version>0.6.0</version>
</dependency>
```

Using odata4j with CXF:

```
<dependency>
  <groupId>org.odata4j</groupId>
  <artifactId>odata4j-core</artifactId>
  <version>0.6.0</version>
  <type>pom</type>
</dependency>
<dependency>
  <groupId>org.odata4j</groupId>
  <artifactId>odata4j-cxf</artifactId>
  <version>0.6.0</version>
</dependency>
```