# Introduction #

Since version 0.5.0 odata4j supports OSGi runtime environments. All transitive dependent artifacts have to be deployed as bundles to OSGi runtime before odata4j can be used.

This is a recommendation how dependencies can be resolved.

# Details #

Maven is usefull to find out all transitive dependencies of odata4j. Just call the maven goal below to get a full list of transitive dependencies. Only dependencies marked as 'compile' or 'provided' are required. Dependencies marked as 'test' can be ignored except you want to run odata4j JUnit test suite within OSGi runtime.

All of this required artifacts have to be deployed at least in compatible versions to OSGi runtime. Some of them are not available as OSGi bundles (this is changing because more and more projects will support OSGi by default). For this components the Springsource Enterprise Bundle Repository is a good source to get wrappers bundles for artifacts not supporting OSGi by default.

[Springsource Enterprise Bundle Repository](http://ebr.springsource.com)



---


```
mvn dependency:tree
```
```
[INFO] Scanning for projects...
[INFO]                                                                         
[INFO] ------------------------------------------------------------------------
[INFO] Building odata4j 0.5-SNAPSHOT
[INFO] ------------------------------------------------------------------------
[INFO] 
[INFO] --- maven-dependency-plugin:2.1:tree (default-cli) @ odata4j ---
[INFO] org.odata4j:odata4j:bundle:0.5-SNAPSHOT
[INFO] +- org.core4j:core4j:jar:0.5-SNAPSHOT:compile
[INFO] +- org.eclipse.persistence:eclipselink:jar:2.1.2:compile
[INFO] +- javax.persistence:javax.persistence:jar:2.0-SNAPSHOT:compile
[INFO] +- com.sun.jersey:jersey-core:jar:1.1.5:compile
[INFO] |  \- javax.ws.rs:jsr311-api:jar:1.1.1:compile
[INFO] +- com.sun.jersey:jersey-client:jar:1.1.5:compile
[INFO] +- com.sun.jersey:jersey-server:jar:1.1.5:compile
[INFO] |  \- asm:asm:jar:3.1:compile
[INFO] +- junit:junit:jar:4.8.2:test
[INFO] +- joda-time:joda-time:jar:1.6:compile
[INFO] +- xmlpull:xmlpull:jar:1.1.3.4a:provided
[INFO] +- hsqldb:hsqldb:jar:1.8.0.10:test
[INFO] +- org.hibernate:hibernate-entitymanager:jar:3.5.5-Final:test
[INFO] |  +- org.hibernate:hibernate-core:jar:3.5.5-Final:test
[INFO] |  |  +- antlr:antlr:jar:2.7.6:test
[INFO] |  |  +- commons-collections:commons-collections:jar:3.1:test
[INFO] |  |  +- dom4j:dom4j:jar:1.6.1:test
[INFO] |  |  |  \- xml-apis:xml-apis:jar:1.0.b2:test
[INFO] |  |  \- javax.transaction:jta:jar:1.1:test
[INFO] |  +- org.hibernate:hibernate-annotations:jar:3.5.5-Final:test
[INFO] |  |  \- org.hibernate:hibernate-commons-annotations:jar:3.2.0.Final:test
[INFO] |  +- cglib:cglib:jar:2.2:test
[INFO] |  +- javassist:javassist:jar:3.9.0.GA:test
[INFO] |  +- org.hibernate.javax.persistence:hibernate-jpa-2.0-api:jar:1.0.0.Final:test
[INFO] |  \- org.slf4j:slf4j-api:jar:1.5.8:test
[INFO] +- org.slf4j:slf4j-jdk14:jar:1.5.8:test
[INFO] \- xmlunit:xmlunit:jar:1.3:test
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 2.042s
[INFO] Finished at: Tue Sep 27 13:32:28 CEST 2011
[INFO] Final Memory: 9M/265M
[INFO] ------------------------------------------------------------------------

```