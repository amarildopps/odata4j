### Contribution Workflow ###
Sign into Gerrit (http://review.odata4j.org)

Obtain your http username/password (Settings -> HTTP Password)

Clone the repo and install the Change-Id commit hook
```
$ git clone http://<your.username>@review.odata4j.org/p/odata4j.git
$ cd odata4j
$ curl -o .git/hooks/commit-msg http://review.odata4j.org/tools/hooks/commit-msg
$ chmod +x .git/hooks/commit-msg
```
Start a local development branch
```
$ git checkout -b <yourbranchname>
```

Make changes, test, commit

Commit message should follow these rules:
  * Start with a short one line summary (no longer than 50 chars)
  * Followed by a blank line
  * Followed by one or more explanatory paragraphs (lines no longer than 72 chars)
  * Use the present tense (fix instead of fixed)
  * Include a Bug: Issue <#> line if fixing a project issue
  * Include a Change-Id line (automatically added by commit hook)

Example commit message:
```
Add sample commit message to guidelines doc

The original patch set for the contributing guidelines doc did not
include a sample commit message, this new patchset does.  Hopefully this
makes things a bit clearer since examples can sometimes help when
explanations don't.

Note that the body of this commit message can be several paragraphs, and
that I word wrap it at 72 characters.  Also note that I keep the summary
line under 50 characters since it is often truncated by tools which
display just the git summary.

Bug: Issue 98765605
Change-Id: Ic4a7c07eeb98cdeaf44e9d231a65a51f3fceae52
```
Update the change with `git commit --amend`  (do not create new commits)

Upload to Gerrit for review
```
$ git push origin HEAD:refs/for/master
# you will be prompted for your http password
```

This will create a new Gerrit change: `http://review.odata4j.org/<changenum>`

Review on Gerrit
  * Look over once yourself, then request review (add a reviewer)
  * If reviewer adds comments, resolve them, amend and reupload the commit if necessary
  * If reviewer sets review+2 and verified+1, the Gerrit change can be submitted

### Code Conventions ###
  * Use 2-spaces for standard indentation (no tabs, including xml files)
  * Use 4-spaces for statement continuation
  * No max line width (yet) - but be reasonable
  * Space after commas, around operators/assignment and after loop keywords
  * One empty line between methods
  * Remove trailing whitespace, even on empty lines
  * Open blocks on the same line
```
for (int i = 0; i < n; i++) {
  ...
```
  * Ok to omit braces for one-line blocks
```
if (true)
  throw new UnsupportedOperationException();
```
  * Prefer immutability for domain objects, with Builder types if necessary
  * Prefer Builder types over complex factory method overloading
  * Builder types declared as static inner types instead of top-level types, .newBuilder() to init, .build() to build
```
public class Foo {
  public static class Builder {
    private Builder() {...
    public Foo build();
  }
  public Builder newBuilder() {...
  public Builder newBuilder(Foo foo) {... // if necessary
}
// not: public class FooBuilder
```
  * One top-level type per .java file
  * Use `x == null` instead of `null == x`
  * Omit `this.` inside methods unless ambiguous
  * Imports should be completely specified.  Do not: `import my.package.*;`

### Eclipse ###
  * Formatter settings: http://code.google.com/p/odata4j/source/browse/odata4j-eclipse-prefs-java-codestyle-formatter.xml

### Before Each Commit ###
  * Ensure that unit and integration tests pass (see JunitTestSuite)
  * Run Maven build