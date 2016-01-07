# Release Checklist #

### Javadocs ###
  * Review javadocs.  At a minimum, document all public apis in:
    * org.odata4j.consumer
    * org.odata4j.core
    * org.odata4j.edm
    * org.odata4j.producer
  * Regenerate and commit javadocs with final version

### Release candidate ###
  * Create release candidate with final version: x.x-SNAPSHOT -> x.x
  * Run tests

### Samples ###
  * Migrate android sample to new client bundle
    * Fix any regressions
    * Run sample project, verify
  * Migrate appengine sample to new nojpa bundle
    * Fix any regressions
    * Update appengine sdk to latest
    * Deploy to oodata4j-sample.appspot.com, verify
  * Migrate heroku sample
    * TODO
  * Migrate cloudfoundry sample
    * TODO
  * Tag samples repo with x.x

### Finalize ###
  * Tag main repo with x.x
  * Push to oss.sonatype: Do a maven release build from command line (javadoc build, artifact signing, upload artifacts to sonatype)

```
mvn clean deploy -P release.build
```

### Sonatype Nexus Repository ###

Before you can upload artifacts to Sonatype you have to request a user account and modify your local settings.xml file. Then you have to follow the Sonatype publishing process. All details are written here: [Sonatype Repository Usage Guide](https://docs.sonatype.org/display/Repository/Sonatype+OSS+Maven+Repository+Usage+Guide)

You are done if all new versions of artifacts are synchronized to Maven Central and visible here: [Search odata4j in Maven Central](http://search.maven.org/#search|ga|1|odata4j)

### odata4j.org ###
  * Create project download (archive zip)
    * Use similar description as previous version
    * Label as Featured, Type-Archive, Stable
    * Unlabel previous Featured download
  * Update links on the front page to new version
  * Update Roadmap wiki page with release notes
  * Update other wiki pages as needed