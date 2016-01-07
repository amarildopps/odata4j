
---

## Roadmap ##

---

### Post Version 1.0 ###

  * OData V3<br />

### Version 1.0 ###

General
  * Concurrency control and ETags ([Issue 90](https://code.google.com/p/odata4j/issues/detail?id=90))
  * Media Link Entries (MLEs) ([Issue 95](https://code.google.com/p/odata4j/issues/detail?id=95))

Consumers
  * Bring expression model to 100%, query by expression via consumer
  * $expand, $select
  * Flesh out pojo mapping

Producers
  * DataServiceVersion negotiation
  * gzip compression
  * Access control
  * Authentication
  * $select

Misc
  * JPA example: jetty + maven, northwind db
  * JPA example: glassfish + eclipselink + mysql, sakila db<br />

### Version 0.8 ###
_In Development_

  * Create JDBC producer ([Issue 28](https://code.google.com/p/odata4j/issues/detail?id=28))
  * Review and cleanup Annotation support ([Issue 200](https://code.google.com/p/odata4j/issues/detail?id=200))
<br />

---

## Changelog ##

---


### Version 0.7 ###
_Latest Stable Version_

General
  * Support for OData error responses ([Issue 41](https://code.google.com/p/odata4j/issues/detail?id=41))

Consumers
  * Support for $count ([Issue 22](https://code.google.com/p/odata4j/issues/detail?id=22))
  * Concurrency control support ([Issue 194](https://code.google.com/p/odata4j/issues/detail?id=194))

Producers
  * Improve Function Call Support ([Issue 182](https://code.google.com/p/odata4j/issues/detail?id=182))

Development
  * New maven module 'odata4j-examples' containing sample code

[View all issues addressed in 0.7](http://code.google.com/p/odata4j/issues/list?can=1&q=Milestone%3DRelease0.7)
<br />
### Version 0.6 ###
  * Depend strictly on JAX-RS to allow both Jersey and Apache CXF implementations ([Issue 54](https://code.google.com/p/odata4j/issues/detail?id=54))

[View all issues addressed in 0.6](http://code.google.com/p/odata4j/issues/list?can=1&q=Milestone%3DRelease0.6)
<br />
### Version 0.5 ###

General
  * $links ([Issue 24](https://code.google.com/p/odata4j/issues/detail?id=24))
  * OSGi support ([Issue 62](https://code.google.com/p/odata4j/issues/detail?id=62))
  * Edm Entity Type hierarchy/subtypes ([Issue 42](https://code.google.com/p/odata4j/issues/detail?id=42))
  * Edm Collection Types
  * [Queryable Metadata](QueryableMetadata.md) ([Issue 74](https://code.google.com/p/odata4j/issues/detail?id=74))
  * EDM Documentation elements and Annotation elements/attributes ([Issue 75](https://code.google.com/p/odata4j/issues/detail?id=75))
  * Mutable builder model for EdmDataServices to ease metadata construction
  * Support Guid primary keys, and UUID support in JPA Producer ([Issue 56](https://code.google.com/p/odata4j/issues/detail?id=56))
  * Parsing any() and all() aggregate functions

Consumers
  * Support multiple app:workspaces for SAP Netweaver Gateway 2.0 ([Issue 57](https://code.google.com/p/odata4j/issues/detail?id=57))
  * ClientFactory for alternative jersey Clients ([Issue 65](https://code.google.com/p/odata4j/issues/detail?id=65))
  * JSON consumers

Producers
  * Service operations ([Issue 17](https://code.google.com/p/odata4j/issues/detail?id=17))
  * $expand ([Issue 12](https://code.google.com/p/odata4j/issues/detail?id=12))

Development
  * Migrated to Mercurial for source control
  * Maven - no more libs
  * Jenkins build triggered on every push thanks to Cloudbees

[View all issues addressed in 0.5](http://code.google.com/p/odata4j/issues/list?can=1&q=Milestone%3DRelease0.5)
<br />
### Version 0.4 ###
Producers
  * Navigation properties ([Issue 7](https://code.google.com/p/odata4j/issues/detail?id=7))
  * Custom query options ([Issue 10](https://code.google.com/p/odata4j/issues/detail?id=10))
  * Batch operations
  * Producer provider config in web.xml (no system properties needed)
  * JPA Producer: Hibernate compatibility ([Issue 29](https://code.google.com/p/odata4j/issues/detail?id=29))

[View all issues addressed in 0.4](http://code.google.com/p/odata4j/issues/list?can=1&q=Milestone%3DRelease0.4)
<br />
### Version 0.3 ###
  * Example android client project [odata4j-android](http://code.google.com/p/odata4j/source/browse?repo=android&name=0.3#hg%2Fodata4j-android), a simple odata service navigator app ([Install odata4j-android-0.3.apk](http://odata4j.googlecode.com/svn/tags/0.3/odata4j-android/dist/odata4j-android-0.3.apk))
  * Various bug fixes, including contributions from users on the [issue list](http://code.google.com/p/odata4j/issues/list)<br />
### Version 0.2 ###
  * JSON support $format=json and $callback) <font color='gray'>Producers</font>
  * Pojo api for ODataConsumer#getEntities/getEntity <font color='gray'>Consumers</font>
  * Support for [Dallas](https://www.sqlazureservices.com/Catalog.aspx) CTP3
    * Basic Authentication <font color='gray'>Consumers</font>
    * Simplified metadata <font color='gray'>Consumers</font>
  * Simple skiptoken support <font color='gray'>Producers</font>
  * Model links as part of entity model
  * Javadoc and source jars as part of each distribution