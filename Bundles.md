# Bundles (Draft) #

Prior to version 0.6 four different bundles have been created (bundle, nojpabundle, clientbundle, core). With version 0.6 odata4j supports two different JAX-RS implementations (Jersey and CXF), thus a changed set of bundles has to be created.

The following table outlines a DRAFT list of bundles and their dependencies.

| Dependencies \ Bundles | jersey-full | cxf-full | jersey-container | cxf-container | jersey-nojpa-container | cxf-nojpa-container | jersey-client | cxf-client | core |
|:-----------------------|:------------|:---------|:-----------------|:--------------|:-----------------------|:--------------------|:--------------|:-----------|:-----|
| core4j                 | x           | x        | x                | x             | x                      | x                   | x             | x          | x    |
| odata4j-core           | x           | x        | x                | x             | x                      | x                   | x             | x          | x    |
| odata4j-jersey         | x           |          | x                |               | x                      |                     | x             |            | x    |
| odata4j-cxf            |             | x        |                  | x             |                        | x                   |               | x          | x    |
| jersey-core            | x           |          | x                |               | x                      |                     | x             |            |      |
| jersey-client          | x           |          |                  |               |                        |                     | x             |            |      |
| jersey-server          | x           |          |                  |               |                        |                     |               |            |      |
| jersey-servlet         |             |          | x                |               | x                      |                     |               |            |      |
| cxf-bundle-jaxrs       |             | x        |                  | x             |                        | x                   |               | x          |      |
| jetty-client           |             | x        |                  |               |                        |                     |               | x          |      |
| jetty-server           |             | x        |                  |               |                        |                     |               |            |      |
| jsr311-api             | x           | x        | x                | x             | x                      | x                   | x             | x          |      |
| joda-time              | x           | x        | x                | x             | x                      | x                   | x             | x          |      |
| javax.persistence      | x           | x        | x                | x             |                        |                     |               |            |      |
| eclipselink            | x           | x        | x                | x             |                        |                     |               |            |      |

Five kinds of bundles are assembled:
  1. **full**: Bundles that contain client and server parts of Jersey or CXF and Jetty. With these bundles standalone applications can be build.
  1. **container**: These bundles can be deployed into servlet containers (e.g. Tomcat).
  1. **nojpa** (container): These bundles are intended for restricted containers (e.g. Google App Engine) that do not support JPA.
  1. **client**: In case an OData consumer is created, one of these client bundles can be used. They only contain Jersey/Jetty client components.
  1. **core**: The core bundle contains the full set of odata4j components, but no further software.