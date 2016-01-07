### Overview ###
There is a vast amount of data available today and data is now being collected and stored at a rate never seen before. Much of this data is managed by Java server applications and difficult to access or integrate into new uses.

The [Open Data Protocol (OData)](http://www.odata.org/) is a Web protocol for querying and updating data that provides a way to unlock this data and free it from silos that exist in applications today. OData does this by applying and building upon Web technologies such as [HTTP](http://www.w3.org/Protocols/), [Atom Publishing Protocol (AtomPub)](http://www.ietf.org/rfc/rfc4287.txt) and [JSON](http://json.org/) to provide access to information from a variety of applications, services, and stores.


### Project Info ###
[odata4j](http://code.google.com/p/odata4j/) is a new open-source toolkit for building first-class OData producers and first-class OData consumers in Java.


### Goals and Guidelines ###
  * Build on top of existing Java standards (e.g. [JAX-RS](https://jsr311.dev.java.net/), [JPA](http://jcp.org/en/jsr/detail?id=317), [StAX](http://jcp.org/en/jsr/detail?id=173)) and use top shelf Java open-source components ([jersey](https://jersey.dev.java.net/), [Joda Time](http://joda-time.sourceforge.net/))
  * OData consumers should run equally well under constrained Java client environments, specifically [Android](http://developer.android.com/index.html)
  * OData producers should run equally well under constrained Java server environments, specifically [Google AppEngine](http://code.google.com/appengine/)


### Consumers - Getting Started ###
  * Download the latest archive zip
  * Add odata4j-bundle-x.x.jar to your build path (or odata4j-clientbundle-x.x.jar for a smaller client-only bundle)
  * Create an new consumer using `ODataConsumer.create("http://pathto/service.svc/")` and use the consumer for client scenarios


### Consumers - Examples ###
_(All of these examples work on Android as well)_
  * Basic consumer query & modification example using the OData Read/Write Test service:  [ODataTestServiceReadWriteExample.java](http://code.google.com/p/odata4j/source/browse/odata4j-fit/src/test/java/org/odata4j/examples/consumers/ODataTestServiceReadWriteExample.java?name=0.6)
  * Simple Netflix example: [NetflixConsumerExample.java](http://code.google.com/p/odata4j/source/browse/odata4j-fit/src/test/java/org/odata4j/examples/consumers/NetflixConsumerExample.java?name=0.6)
  * List all entities for a given service: [ServiceListingConsumerExample.java](http://code.google.com/p/odata4j/source/browse/odata4j-fit/src/test/java/org/odata4j/examples/consumers/ServiceListingConsumerExample.java?name=0.6)
  * Use the Dallas data service to query AP news stories (requires Dallas credentials):  [DallasConsumerExampleAP.java](http://code.google.com/p/odata4j/source/browse/odata4j-fit/src/test/java/org/odata4j/examples/consumers/DallasConsumerExampleAP.java?name=0.6)
  * Use the Dallas data service to query UNESCO data (requires Dallas credentials):  [DallasConsumerExampleUnescoUIS.java](http://code.google.com/p/odata4j/source/browse/odata4j-fit/src/test/java/org/odata4j/examples/consumers/DallasConsumerExampleUnescoUIS.java?name=0.6)
  * Basic Azure table-service table/entity manipulation example (requires Azure storage credentials):  [AzureTableStorageConsumerExample.java](http://code.google.com/p/odata4j/source/browse/odata4j-fit/src/test/java/org/odata4j/examples/consumers/AzureTableStorageConsumerExample.java?name=0.6)
  * Test against the sample odata4j service hosted on Google AppEngine:  [AppEngineConsumerExample.java](http://code.google.com/p/odata4j/source/browse/odata4j-fit/src/test/java/org/odata4j/examples/consumers/AppEngineConsumerExample.java?name=0.6)


### Producers - Getting Started ###
  * Download the latest archive zip
  * Add odata4j-bundle-x.x.jar to your build path (or odata4j-nojpabundle-x.x.jar for a smaller bundle with no JPA support)
  * Choose or implement an ODataProducer
    * Use InMemoryProducer to expose POJOs as OData entities using bean properties.  Example:  [InMemoryProducerExample.java](http://code.google.com/p/odata4j/source/browse/odata4j-fit/src/test/java/org/odata4j/examples/producer/InMemoryProducerExample.java?name=0.6)
    * Use JPAProducer to expose an existing JPA 2.0 entity model:  [JPAProducerExample.java](http://code.google.com/p/odata4j/source/browse/odata4j-fit/src/test/java/org/odata4j/examples/producer/JPAProducerExample.java?name=0.6)
    * Implement ODataProducer directly
    * Take a look at [odata4j-appengine](http://code.google.com/p/odata4j/source/browse?repo=samples&name=0.4#hg%2Fodata4j-appengine) for an example of how to expose AppEngine's [Datastore](http://code.google.com/appengine/docs/java/javadoc/com/google/appengine/api/datastore/package-summary.html) as an OData endpoint
    * Hosting in Tomcat: http://code.google.com/p/odata4j/wiki/Tomcat


### Status ###
odata4j is still early days, a quick summary of what's implemented
  * Fairly complete expression parser (pretty much everything except complex navigation property literals)
  * URI parser for producers
  * Complete EDM metadata parser (and model)
  * Dynamic entity/property model (OEntity/OProperty)
  * Consumer api:  ODataConsumer
  * Producer api:  ODataProducer
  * ATOM and JSON transports
  * Non standard behavior (e.g. Azure authentication, Dallas paging) via client extension api
  * Transparent server-driven paging
  * Cross domain policy files for silverlight clients
  * Free WADL for your OData producer thanks to jersey.  e.g. [odata4j-sample application.wadl](http://odata4j-sample.appspot.com/datastore.svc/application.wadl)
  * Tested with current OData client set (.NET ,Silverlight, [LinqPad](http://www.linqpad.net/), [PowerPivot](http://www.powerpivot.com/))

### Roadmap and Changelog ###
[Roadmap](Roadmap.md)

