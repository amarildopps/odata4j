# Introduction #

odata4j comes with a bunch of JUnit test classes running several hundreds of test methods. Some of them are unit tests whereas others are integration tests. The so called full integration tests (FIT) launch and fill a local database, start a local producer and connect via a consumer connection to run a test scenario.

Because of odata4j supports two JAX-RS implementations which are Sun Jersey and Apache CXF, it is aimed to re-use and run test cases on both implementation stacks.

# Details #

The maven module **odata4j-core** contains **unit tests** that solely depend on classes that are part of the odata4j core.

Module **odata4j-fit** contains **integration tests** and that require a concrete JAX-RS implementation which can be Jersey or CXF. The concrete JAX-RS implementation is encapsulated behind a facade class and can be exchanged. The class diagram shows this principle.

<img src='http://wiki.odata4j.googlecode.com/hg/odata4j%20-%20fit%20test%20classes.png' />

# Maven Test Build #

Building and running all unit tests and integration tests using Jersey (default) as JAX-RS runtime:

```
mvn clean install
```
or
```
mvn clean install -Dorg.odata4j.jaxrs.runtime=jersey
```

Building and running all unit tests and integration tests using CXF as JAX-RS runtime:

```
mvn clean install -Dorg.odata4j.jaxrs.runtime=cxf
```

Building and running all unit and integration tests (i.e. using CXF and Jersey as JAX-RS runtime):

```
mvn clean install -Dorg.odata4j.jaxrs.runtime=cxf,jersey
```

This will run each test case twice. First run is using CXF and the second run uses Jersey.

Obsolete since version 0.7.0-SNAPSHOT:

Please note that the case first Jersey, second CXF is not supported due to limitations of CXF factory mechanism. A run in this order will result in erroneous unit execution.