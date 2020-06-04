# Release Procedure

Steps marked with (*) are be performed by the "Release Manager" or a member of the Runtime Team.

PM = Product Manager (Andre Bappert)

EM = Engineering Manager (Thorben Lindhauer)

DE = Director of Engineering (Roman Smirnov) 

QA = Michael Schöttes

MKT = marketing@camunda.com

SUP = team-support@camunda.com

INFRA = `@cambpm-dri` in #infra channel on Slack

DEVREL = Developer Relations (Niall Deehan)

Release Manager:

Round-Robin from the following list:

```
Tobias Metzke
Miklas Boskamp
Tassilo Weidner
Yana Vasileva
Martin Stamm
Nikola Koevski
```

The release procedure itself has 4 phases:

## [Phase 1: Setup Tasks](#phase-1)

These tasks can be done within the two weeks before the release:

*  [Request the new license book from Ulrike](#request-the-new-license-book) (EM)
*  [Prepare testing plan](#prepare-test-plan) (* + QA)
*  [Build a Release Candidate](#build-a-release-candidate) (*)
*  [Test standalone webapps](#test-standalone-webapps) (*)
*  [Adjust and migrate Getting Started guides](#adjust-and-migrate-getting-started-guides) (*)
*  [Adjust and migrate Examples](#adjust-and-migrate-examples) (*)
*  [Adjust and migrate unit testing template](#adjust-and-migrate-unit-testing-template) (*)
*  [Write Update Guide](#write-update-guide) (*)
*  [Stage a blog post](#stage-the-blog-post) (*)
*  [Update translations](#update-german-webapp-translations) (*)
*  [Update Screenshots](#update-screenshots) (*)
*  [Release all secondary Camunda projects](#release-all-secondary-camunda-projects) (*)
*  [Update the Entity Relationship Diagrams](#update-the-entity-relationship-diagrams) (*)
*  [Schedule the releases of side projects](#schedule-the-releases-of-side-projects) (*)

## [Phase 2: Prepare the Release](#phase-2)

These steps can be done a couple of days before the release:

*  [Check Preconditions](#check-preconditions) (*)
*  [Trigger the release build](#trigger-the-release-build) (*)
*  [Test Release](#test-release) (* + QA)
*  [Create new branches](#create-new-branches) (*)
*  [Stage new version of the docs](#stage-new-version-of-the-docs) (*)
*  [Adjust docs with new version](#adjust-docs-with-new-version) (*)
*  [Stage Enterprise Download Page](#stage-enterprise-download-page) (*, QA)
*  [Stage Community Download Page](#stage-community-download-page) (*)

## [Phase 3: Publish the Release](#phase-3)

This is done on the release day:

*  [Release Jira](#release-jira) (EM, DE)
*  [Forward Security Reports](#forward-security-reports) (EM, DE)
*  [Release Maven Central](#release-maven-central) (*)
*  [Release Javadocs](#release-javadocs) (*)
*  [Release the Staged Docs](#release-the-staged-docs) (*)
*  [Push Redirects / URL rewrites adjustments](#push-redirects--url-rewrites-adjustments) (*)
*  [Adjust Google Custom Search Engine](#adjust-google-custom-search-engine) (PM)
*  [Release the Getting Started](#release-the-getting-started) (*)
*  [Release the Enterprise Download Page](#release-the-enterprise-download-page) (*)
*  [Release the Community Download Page](#release-the-community-download-page) (*)
*  [Release the Blog Post](#release-the-blog-post) (*)
*  [Publish the release](#publish-the-release) (MKT + SUP + DEVREL)
*  [Send Release Notice](#send-release-notice) (*)
*  [Improve this guide](#improve-this-guide) (*)

## [Phase 4: After the Release](#phase-4)

*  [Camunda BPM Platform code changes](#camunda-bpm-platform-code-changes) (*)
*  [Pick Alpha Release Manager](#pick-alpha-release-manager)

***

# Phase 1

## Request the new license book

At latest two weeks before the release, create a CAM ticket to update the license book and assign it to the Tech Lead. The Tech Lead should then perform the following steps:

1. Announce to the team that any subsequent dependency updates until code freeze must be synced with Tech Lead
1. Determine all depencies with missing licensing information. Check the build artifact `new-dependencies.txt` of https://ci.cambpm.camunda.cloud/job/7.14/job/7.14-EE-platform-DISTRO/ (replace with appropriate version).
1. Contribute the project information for every dependency where it is missing. Check the readme of https://github.com/camunda/camunda-bpm-platform-ee/tree/master/distro/license-book-generator for how to do that.
1. Send a list of all libraries with missing licensing information and their project homes to Legal
1. Once Legal provides the missing licensing information, update it in the license-book-generator sources
1. Verify that the generated license book is now complete
1. Update the license book in the sources at https://github.com/camunda/camunda-bpm-platform/tree/master/distro/license-book
1. Update the Nexus link to the license book in the [docs](https://docs.camunda.org/manual/develop/introduction/licenses/#third-party-libraries)

## Prepare test plan

Set a meeting with QA to prepare a test plan for the manual testing of the release:
1. Pick which features to test by looking at the [Confluence Roadmap list](https://app.camunda.com/confluence/display/camBPM/Roadmap+Camunda+BPM)
2. Create a (Google/Airtable) spreadsheet to be used for keeping track of the testing process (you can use [this spreadsheet](https://docs.google.com/spreadsheets/d/1_xXKKN-JmnmnFWRl9pnXdNXzl3sCkOf_tfu-zJnMdHU/edit?usp=sharing) as a template)
  * The spreadsheet should include all the features that need to be tested, feature documentation and implementer name
  * The spreadsheet should include an estimated effort (small, medium, large) and all test cases should be divided equally to all team members, e.g. by using round-robin on the different effort levels respecting that the implementer and reviewer should not test again
  * The spreadsheet should include all combinations of environments that should be testetd. Those environments should also be assigned to topics as recommended environments (consider those in the assignment of the topics to testers as well, so that testers have a reasonable amount of environments to set up for their topics).
3. Check if there are any new supported environments (ex. new Database Versions) and if they are available for testing through [Portainer](http://portainer.camunda.loc:9000/).
  * If there is an environment missing, ask INFRA to provide it.
4. Validate the assumptions for the test plan with the development team (are the effort estimations correct, are testers fine with their assigned topics, are all necessary features included). This can be done in a separate meeting or by individual feedback.

## Build a Release Candidate

Use the [Alpha Release Guide](https://github.com/camunda/camunda-bpm-dev-docs/blob/master/releases/Performing-an-Alpha-Release.md) and perform the following steps to create a release candidate:
1. [Check Preconditions](https://github.com/camunda/camunda-bpm-dev-docs/blob/master/releases/Performing-an-Alpha-Release.md#check-preconditions)
1. [Trigger the release build](https://github.com/camunda/camunda-bpm-dev-docs/blob/master/releases/Performing-an-Alpha-Release.md#trigger-the-release-build)
1. [Update the Enterprise Download Page](https://github.com/camunda/camunda-bpm-dev-docs/blob/master/releases/Performing-an-Alpha-Release.md#update-the-enterprise-download-page)
1. Test the release candidate by using the [test plan](#prepare-test-plan) and [Test standalone webapps](#test-standalone-webapps)
1. [Release Maven Central](https://github.com/camunda/camunda-bpm-dev-docs/blob/master/releases/Performing-an-Alpha-Release.md#release-maven-central)
1. [Release JIRA](https://github.com/camunda/camunda-bpm-dev-docs/blob/master/releases/Performing-an-Alpha-Release.md#release-jira)

## Test standalone webapps

You have to download the CE standalone webapps
([`camunda-webapp-SERVER-standalone-VERSION.war`](https://downloads.camunda.cloud/release/camunda-bpm/))
and the EE standalone webapps
([`camunda-webapp-ee-SERVER-standalone-VERSION-ee.war`](https://downloads.camunda.cloud/enterprise-release/camunda-bpm/)).

**Note:** There is no separate standalone webapp for wildfly, use the jboss one
for testing.

To test the standalone webapp you need a vanilla version of the application
servers (without camunda installed). You can download some of the vanilla servers
directly from the vendors:

- [Tomcat](http://tomcat.apache.org/)
- [JBoss](https://jbossas.jboss.org/downloads)
- [Widlfly](https://wildfly.org/downloads/)

The versions to use can be found in the current [parent
pom.xml](https://github.com/camunda/camunda-bpm-platform/blob/master/parent/pom.xml#L41-L43).

For WebSphere and Weblogic, images are available in our [portainer](http://portainer.camunda.loc:9000/#/templates). Enable "Show container templates" and type "WebSphere" or "Weblogic" in the search bar. Do not use one of the autosetup templates as these already ship with Camunda installed.

To deploy the standalone webapp follow the [installation
guide](http://stage.docs.camunda.org/manual/develop/installation/standalone-webapplication/#deploy). Normally
you have to copy the war file to the corresponding webapps folder of the
application server or use the web console for websphere and weblogic.

You can configure the process engine and database by editing the
`WEB-INF/applicationContext.xml` in the war file, if you want to test something
special.

After starting the standalone webapp test the webapps by starting some
processes, completing task, adding users and so one. Also focus on new
features introduced in the upcoming release.

**Note:** The job executor is disabled in the standalone webapps, which means
no jobs will be executed. So for example batches will not be executed either.

After finishing the test compare the process engine configuration in
`WEB-INF/applicationContext.xml` of the war file with the configuration
of the process engine shipped with the distribution. In general the engines
should be configure identical.

## Adjust and migrate Getting Started guides

Follow the procedure here:
https://github.com/camunda/camunda-bpm-dev-docs/blob/master/releases/Getting-started-guides.md

Keep in mind that the platform version in the `pom.xml` files should be adjusted only for testing. Only commit changes that are needed for the Guides to be functional. The platform version will be permanently adjusted through a Jenkins job at a later phase of the Release process.

## Adjust and migrate Examples

Test and adjust the existing Camunda BPM Examples for the new minor release. 
For each of the following repositories
* https://github.com/camunda/camunda-bpm-examples
* https://github.com/camunda/camunda-external-task-client-js/tree/master/examples
* https://github.com/camunda/camunda-external-task-client-java/tree/master/examples
* https://github.com/camunda/camunda-bpm-assert/tree/master/examples

perform the following steps:

* Clone the Git Repository
* If the branch does not already exist, create a new branch for the minor release (ex. 7.14) to push all the changes to the branch and merge it to master when the new minor release exists. This allows to have the examples runnable until the release is finished.
* Adjust the `camunda-version` in the Maven `pom.xml` of the current example
* Follow the README of the current example to test it
* Adjust the links of the README to point to the new version of the docs (ex.: `https://docs.camunda.org/manual/7.14`)
* Push the changes to the master/minor release branch
* Do the steps before for all examples
* Squash all the commits and merge the release branch to master
* Only for examples repository: Add a new (7.X) tag to the overview [README](https://github.com/camunda/camunda-bpm-examples/blob/master/README.md)
* Create the 7.X tag for the minor release version on the squashed commit
* Create a release on GitHub for the created tag
* Push the changes to master

### Changes to the `Java` source files in the examples
Make sure the files contain a valid license header.

### Changes to the External Task Client examples
Make sure the examples use the newest Client version.

## Adjust and migrate unit testing template

Bump the Camunda version in https://github.com/camunda/camunda-engine-unittest

## Write Update Guide

Document the steps which are necessary to update from Camunda BPM 7.13.x to 7.14.0 for the different application server.

* Go into the cloned Git repository `camunda-docs-manual`
* Create a new folder `713-to-714` under `content/minor`
* Describe the different steps for the current application server in an own file.

Usually it is okay to copy + paste the guides from the previous version and update the list of artifacts in the guide. (i.e. make sure all artifacts that changed between the previous release and the current release are included in the guide)

## Stage the blog post

#### 1. Update the 'master' and create a branch 7.14.0 on the [blog repository](https://github.com/camunda/blog.camunda.org).

```
git checkout master
git pull origin master
git checkout -b 7.14.0
```

#### 2. Create a file 'camunda-bpm-7140-released.md' at /content/post/<year>/<month>/ and push it to the repo.

```
git add /content/post/<year>/<month>/camunda-bpm-7140-released.md
git commit
git push origin 7.14.0
```

#### 3. Ask EM to write the blog post. 

It should be ready two days before the release presentation and submitted to marketing for proof reading. Marketing can also share information on the publishing date for the post.


## Update german Webapp translations

Ask a native-german colleague to update the [german translations](https://github.com/camunda/camunda-webapp-translations).

## Update Screenshots

Generate up-to-date screenshots according to the [HowTo](../howtos/update-screenshots.md).

## Release all secondary Camunda projects

1. Check the [bom](https://github.com/camunda/camunda-bpm-platform/blob/master/bom/pom.xml#L34-L43), if there are secondary projects with an alpha/snapshot version.
2. If that's the case we need to release them. Follow this [procedure](https://github.com/camunda/camunda-bpm-dev-docs/blob/master/releases/Release-secondary-projects.md)

**Hint:**
When releasing a secondary project, other secondary projects might have cross-references to it and you should update them accordingly. E.g., for camunda-commons you need to update the `pom.xml` files of the following projects as well:
* spin
* connect
* engine

## Update the Entity Relationship Diagrams

1. Go to the [database upgrade scripts](https://github.com/camunda/camunda-bpm-platform/tree/master/engine/src/main/resources/org/camunda/bpm/engine/db/create) and check what changed in the database structure.
2. Create ERDs for the new version by copying the last version ERDs (ex. erd-bpmn_7.14.mwb from a copy of erd-bpmn_7.13.mwb). Then update the new ERDs accordingly.
  * The ERD files can be found in the camunda-docs-manual repo [here](https://github.com/camunda/camunda-docs-manual/tree/master/content/user-guide/process-engine/erd-project).
  * They can be edited through the [MySQL Workbench](https://www.mysql.com/products/workbench/) tool.
  * After they are edited, generate the appropriate `.SVG` images for each ERD and put them in the `img` directory [here](https://github.com/camunda/camunda-docs-manual/tree/master/content/user-guide/process-engine/img). Ex. for `erd-bpmn_7.14.mwb` there should be a `erd_714_bpmn.svg` image.
  * Finally, update the image links in the [database.md page|https://github.com/camunda/camunda-docs-manual/blob/master/content/user-guide/process-engine/database.md]
3. Check if everything looks good after you make the adjustments in the [documentation](https://stage.docs.camunda.org/manual/latest/user-guide/process-engine/database/#entity-relationship-diagrams).

## Schedule the releases of side projects

Check if a new version of each of the side projects needs to be released and schedule the release date. Check for dependencies to artifacts that might get released later (e.g. engine) and be sure to release those dependencies first.

The side projects are:

* [JS External Task Client](https://github.com/camunda/camunda-external-task-client-js/wiki/Release-procedure)
* [Java External Task Client](https://github.com/camunda/camunda-external-task-client-java)
* [Camunda BPM Assert](https://github.com/camunda/camunda-bpm-assert)

For the release follow this [procedure](https://github.com/camunda/camunda-bpm-dev-docs/blob/master/releases/Release-secondary-projects.md).

-----

# Phase 2

## Check Preconditions

- There are no Snapshot dependencies to secondary projects like Spin Connect (check [bom](https://github.com/camunda/camunda-bpm-platform/blob/master/bom/pom.xml) for snapshot dependencies)
- There are no code problems ([check ci](https://broken.cambpm.camunda.cloud/) section "master")
- [Release Test Job](https://release.cambpm.camunda.cloud/view/Release-Test/) passed successfully recently
- There are no code tickets in review or test
- Post a message to `#cambpm-announcements` and `@cambpm-dri` (INFRA) on Slack:
```
Hey Team,

please stop pushing to master since we are going to build the 7.x release.
```
*Keep in mind that staged artifacts are kept up to 6 days, arrange the build no more than 6 days before the official release. This information is based on previous observations and not an official number. When in doubt release the artifacts rather sooner than later.

## Trigger the Release Build

Open the following URL in Jenkins:
https://release.cambpm.camunda.cloud/view/Release-Master/

Set correct versions, select Release Type = "Final" and click on the "Run" icon.

Wait for the following jobs to turn green before continuing with the next step:
* 7.X-RELEASE-build-camunda-bpm-CE-tags
* 7.X-RELEASE-build-camunda-bpm-EE-tags

## Test Release

Let the team and QA test the [released artifacts](https://downloads.camunda.cloud/enterprise-release/camunda-bpm/) with the [test plan](https://github.com/camunda/camunda-bpm-dev-docs/blob/master/releases/Performing-a-Minor-Release.md#prepare-test-plan) including the [standard regression test](https://github.com/camunda/camunda-bpm-dev-docs/blob/master/releases/Performing-an-Alpha-Release.md#standard-regression-test) and paying attention to all changes since the release candidate.

If everything is satisfying with the release, send a message to `#cambpm-announcements` on Slack:

```
Hey Team,

the release test passed, you can commit to master again in case of approved bug fixes :)

```

## Create new branches

In order to have all the branches ready once the release is built we need to trigger the respective jobs before triggering the release job.

If anything goes wrong, ask the `@cambpm-dri` in the #infra channel on Slack for help.

### Create maintenance branches

Trigger the creation of the maintenance branches with [this job](https://release.cambpm.camunda.cloud/view/all/job/maintenance/job/RELEASE-create-camunda-bpm-maintenance-branch/).
Example for Parameters:
```
TAG_VERSION = 7.14.0
DEVELOPMENT_VERSION = 7.14.1-SNAPSHOT
BRANCH_VERSION = 7.14
PUSH_REMOTE = true
```

Check that new branches were created in the following repositories:
* https://github.com/camunda/camunda-bpm-platform-maintenance
* https://github.com/camunda/camunda-bpm-platform-ee-maintenance
* https://github.com/camunda/docker-camunda-bpm-platform
* https://github.com/camunda/camunda-bpm-webapp
* https://github.com/camunda/camunda-webapp-translations

### Create new minor version jobs

Perform these steps:

* Check out [the repository](https://github.com/camunda-ci/jenkins-job-dsl-seed-jobs)
* Execute the [add-new-minor-version.sh](https://github.com/camunda-ci/jenkins-job-dsl-seed-jobs/blob/master/add-new-minor-version.sh) script
* Open a PR in the GitHub repository with the changes made by the script and assign `@cambpm-dri` as reviewer
* Merge the branch after the review is done
* After drinking 1 espresso, the new jobs should now appear on https://ci.cambpm.camunda.cloud/view/Docs/
* Trigger `camunda-docs-manual-stage (7.14, 7.14)` and check the result at http://stage.docs.camunda.org/manual/7.14/

### Add new version to the broken board

The [broken board](https://broken.cambpm.camunda.cloud/) should also display the new version jobs. To do that, perform these steps:
* Add the new version to [camunda-ci-dashboard.hcl](https://github.com/camunda/infra-core/blob/master/camunda-ci/kustomize/cambpm/broken.cambpm.camunda.cloud/config/camunda-ci-dashboard.hcl)
* Open a PR to the `stage` branche in the GitHub repository with the added version and assign `@cambpm-dri` as reviewer
* Merge the branch after the review is done
* Check that the new version is displayed at the [broken board](https://broken.cambpm.camunda.cloud/)

## Stage new version of the docs

### Create new Manual Branch

First a new branch needs to be created in the camunda-docs-manual repository

```sh
git checkout -b 7.14
```

### Update standalone webapp download links

On the new branch, bump the version of the standalone webapps as follows:

```diff
--- a/content/installation/standalone-webapplication.md
+++ b/content/installation/standalone-webapplication.md
@@ -42,24 +42,24 @@ As a **Community Edition** user you can download the Camunda standalone webapp m
     <tr>
       <td>Apache Tomcat</td>
       <td>
-        <a href="//downloads.camunda.cloud/release/camunda-bpm/tomcat/7.13/camunda-webapp-tomcat-standalone-7.13.0.war">
-          camunda-webapp-tomcat-standalone-7.13.0.war
+        <a href="//downloads.camunda.cloud/release/camunda-bpm/tomcat/7.14/camunda-webapp-tomcat-standalone-7.14.0.war">
+          camunda-webapp-tomcat-standalone-7.14.0.war
         </a>
       </td>
     </tr>
     <tr>
       <td>WildFly</td>
       <td>
-        <a href="//downloads.camunda.cloud/release/camunda-bpm/jboss/7.13/camunda-webapp-jboss-standalone-7.13.0.war">
-          camunda-webapp-jboss-standalone-7.13.0.war
+        <a href="//downloads.camunda.cloud/release/camunda-bpm/jboss/7.14/camunda-webapp-jboss-standalone-7.14.0.war">
+          camunda-webapp-jboss-standalone-7.14.0.war
         </a>
       </td>
     </tr>
...
```

### Adjust config on branch

On the new branch, adjust the configuration. Open the file `config.yaml` with an editor and edit it as follows:

```diff
--- a/config.yaml
+++ b/config.yaml
@@ -1,5 +1,5 @@
 ---
-baseurl: "/manual/develop/"
+baseurl: "/manual/7.14/"
 languageCode: "en-us"
 title: "Camunda BPM documentation"
 theme: "camunda"
@@ -20,10 +20,10 @@ params:
       url: "/enterprise"
   section:
     id: "manual"
-    version: "develop"
+    version: "7.14"
-    versionAlias: "7.14"
     versions:
       - "latest"
+      - "7.14"
       - "7.13"
       - "7.12"
	   - "7.11"
       - "7.10"
       - "7.9"
       - "7.8"
       - "7.7"
       - "7.6"
       - "7.5"
       - "7.4"
       - "7.3"
       - "7.2"
       - "7.1"
@@ -31,5 +31,5 @@ params:
       - "develop"
   github:
     repositoryName: "camunda/camunda-docs-manual"
-    branch: "master"
+    branch: "7.14"
 ...
```

Test the changes by running

```sh
hugo server --baseUrl=http://localhost --port:1313
```

and opening http://localhost:1313/manual/7.14/ in the browser. Also check, that the new version is included in the version selection dropdown.

Commit changes and push the branch:

```sh
git commit -a -m "chore(release): create 7.14 branch"
git push origin 7.14
```

## Adjust docs with new version

### Release new docs version as latest

You also need to release the new version as "latest".

Checkout the latest branch of the camunda-docs-manual repository and set to new version

```sh
git checkout latest
git reset 7.14 --hard
```

Next, adjust the configuration for the latest version

```diff
--- a/config.yaml
+++ b/config.yaml
@@ -1,5 +1,5 @@
 ---
-baseurl: "/manual/7.14/"
+baseurl: "/manual/latest/"
 languageCode: "en-us"
 title: "Camunda BPM documentation"
 theme: "camunda"
@@ -20,7 +20,8 @@ params:
       url: "/enterprise"
   section:
     id: "manual"
-    version: "7.14"
+    version: "latest"
+    versionAlias: "7.14"
     versions:
       - "latest"
       - "7.14"
```

Commit and force-push

```sh
git commit -a -m "chore(release): release 7.14 as latest"
git push origin latest --force
```

### Add new version of the manual to all other versions

Next we need to make sure the user can select the new version in the version select boxes on all other branches.
For docpad based branches (7.0 - 7.3), this information will not be updated.
For hugo based branches (master, 7.4+) this means:

```diff
--- a/config.yaml
+++ b/config.yaml
@@ -21,9 +21,10 @@ params:
   section:
     id: "manual"
     version: "7.4"
     versions:
       - "latest"
+      - "7.14"
       - "7.13"
```

```sh
git commit -a -m "chore(release): add 7.14"
git push origin 7.4
```

On master, we also bump the version alias to the next minor version:

```diff
--- a/config.yaml
+++ b/config.yaml
@@ -21,9 +21,10 @@ params:
   section:
     id: "manual"
     version: "develop"
-    versionAlias: "7.14"
+    versionAlias: "7.15"
     versions:
       - "latest"
+      - "7.14"
       - "7.13"
```

```sh
git commit -a -m "chore(release): add 7.14, bump to 7.15 "
git push origin master
```

### Adjust Redirects / URL rewrites

Next we need to adjust the URL rewrites. First for stage. Go into the repository `camunda-docs-static`.
Perform the following changes to release 7.14 (essentially bump `develop` to `7.15-SNAPSHOT`) :

```diff
--- a/config/stage/.htaccess
+++ b/config/stage/.htaccess
@@ -6,7 +6,7 @@ RewriteRule ^$ /manual/
 RewriteRule ^latest/api-references/java/ /manual/7.3/reference/javadoc/

 # Javadoc
-RewriteRule ^manual/develop/reference/javadoc/$ /javadoc/camunda-bpm-platform/7.14-SNAPSHOT [R=307,L]
+RewriteRule ^manual/develop/reference/javadoc/$ /javadoc/camunda-bpm-platform/7.15-SNAPSHOT [R=307,L]
 RewriteRule ^manual/latest/reference/javadoc/$ /javadoc/camunda-bpm-platform/7.14 [R=307,L]
 RewriteRule ^manual/([^/]+)/reference/javadoc/$ /javadoc/camunda-bpm-platform/$1 [R=307,L]
```

Commit & push to master

```sh
git commit -a -m "chore(release): set next javadocs redirect"
git push origin master
```

This is all that needs to be done for stage. For live we need to set the new version as the default "stable version". However, we should not commit and push this directly to master since it will become visible right away. Instead we prepare a pull request:

```sh
git checkout -b 7.14
```

Perform the following edits to release 7.14 (essentially you need to replace all the occurrences of the string `7.13` with `7.14` and bump `develop` to `7.15`) :

```diff
--- a/config/live/.htaccess
+++ b/config/live/.htaccess
@@ -8,31 +8,31 @@ RewriteRule ^/?(.*) https://%{SERVER_NAME}/$1 [R=301,L]
 RewriteRule ^$ /manual/


 # Javadoc
-RewriteRule ^manual/develop/reference/javadoc/$ /javadoc/camunda-bpm-platform/7.14-SNAPSHOT [R=307,L]
+RewriteRule ^manual/develop/reference/javadoc/$ /javadoc/camunda-bpm-platform/7.15-SNAPSHOT [R=307,L]
 RewriteRule ^manual/latest/reference/javadoc/$ /javadoc/camunda-bpm-platform/7.14 [R=307,L]
 RewriteRule ^manual/([^/]+)/reference/javadoc/$ /javadoc/camunda-bpm-platform/$1 [R=307,L]

 # manual without version
-RewriteRule ^manual/?$ /manual/7.13/ [R=307,L]
+RewriteRule ^manual/?$ /manual/7.14/ [R=307,L]

-RewriteCond %{REQUEST_URI} ^/manual((?!7.13/)[^/]+)
+RewriteCond %{REQUEST_URI} ^/manual((?!7.14/)[^/]+)
 RewriteCond %{DOCUMENT_ROOT}/manual/%1 !-d
-RewriteRule ^manual/(.*) /manual/7.13/ [R=307,L]
+RewriteRule ^manual/(.*) /manual/7.14/ [R=307,L]

- # Redirect /manual/X to /manual/7.13/X if folder X does not exist but 7.13/X does
-RewriteCond %{REQUEST_URI} ^/manual/((?!7.13/)[^/]+)
+ # Redirect /manual/X to /manual/7.14/X if folder X does not exist but 7.14/X does
+RewriteCond %{REQUEST_URI} ^/manual/((?!7.14/)[^/]+)
 RewriteCond %{DOCUMENT_ROOT}/manual/%1 !-d
-RewriteCond %{DOCUMENT_ROOT}/manual/7.13/%1 -d
-RewriteRule ^manual/(.*) /manual/7.13/$1 [R=307,L]
+RewriteCond %{DOCUMENT_ROOT}/manual/7.14/%1 -d
+RewriteRule ^manual/(.*) /manual/7.14/$1 [R=307,L]

-# Redirect /manual/X/Y to /manual/7.13/Y if folder X does not exist but 7.13/Y does
-RewriteCond %{REQUEST_URI} ^/manual/((?!7.13/?)[^/]+)(/[^/]+)?
+# Redirect /manual/X/Y to /manual/7.14/Y if folder X does not exist but 7.14/Y does
+RewriteCond %{REQUEST_URI} ^/manual/((?!7.14/?)[^/]+)(/[^/]+)?
 RewriteCond %{DOCUMENT_ROOT}/manual/%1 !-d
-RewriteCond %{DOCUMENT_ROOT}/manual/7.13%2 -d
-RewriteRule ^manual/[^/]+(/[^/]+(?:/.*))? /manual/7.13$1 [R=307,L]
+RewriteCond %{DOCUMENT_ROOT}/manual/7.14%2 -d
+RewriteRule ^manual/[^/]+(/[^/]+(?:/.*))? /manual/7.14$1 [R=307,L]
```

commit and push to branch:

```sh
git commit -am "chore(release): set 7.14 as default version"
git push origin 7.14
```

Merge this branch into master on the release day.

#### Update the documentation smoke tests

We also need to [adjust the smoke tests](https://github.com/camunda/camunda-docs-static/blob/master/test/HttpRedirectionTest.yml) for the URL rewrites that we adjusted before. As with the rewrite rules before, you basically need to replace all the occurrences of `7.13` with `7.14`.

```diff
--- a/test/HttpRedirectionTest.yml
+++ b/test/HttpRedirectionTest.yml
https://docs.camunda.org:
  /:
-    target: /manual/7.13/
+    target: /manual/7.14/
    status: 307
  /manual/:
-    target: /manual/7.13/
+    target: /manual/7.14/
    status: 307
  /manual/reference/bpmn20:
-    target: /manual/7.13/
+    target: /manual/7.14/
    status: 307
```

Commit the changes to the release branch and merge this branch into master on the release day:

```sh
git commit -am "chore(release): update smoke tests to version 7.14"
git push origin 7.14
```

## Stage Enterprise Download Page

The new minor version needs to be added to the enterprise download page. First, generate the link to the release notes via generate notes via [JIRA](https://jira.camunda.com/plugins/servlet/project-config/CAM/administer-versions?status=unreleased). Select the version and click on "Release Notes".
Perform the following edits (adjust release notes link):

```diff
--- a/enterprise/content/download.md
+++ b/enterprise/content/download.md
@@ -35,13 +35,17 @@ downloads:
     - war

   selected:
-    branch: "7.13"
-    version: "7.13.0"
+    branch: "7.14"
+    version: "7.14.0"
     server: "tomcat"

   branches:
   - branch: "7.14"
     releases:
+    - number: "7.14.0"
+      note: "https://jira.camunda.com/secure/ReleaseNote.jspa?projectId=10230&version=15532"
+      date: "2019.11.29"
+
-    - number: "7.14.0-alpha5"
-      note: "https://jira.camunda.com/secure/ReleaseNote.jspa?projectId=10230&version=15532"
-      date: "2019.05.01"
```

Note: You will need to remove all the alpha versions of the version you are releasing.

Commit and push to master:

```sh
git commit -a -m "chore(release): add 7.14.0 to download page"
git push origin master
```

## Stage Community Download Page

Repository: [camunda.com-new](https://github.com/camunda/camunda.com-new)
You need to adjust [releases.json](https://github.com/camunda/camunda.com-new/blob/master/data/releases.json) and check your change on [stage](https://app.camunda.com/jenkins/view/All/job/stage.camunda.com-new%20(master)/).

Note: "showAlpha" must be set to false when releasing minor

-----

# Phase 3

## Release Jira

Talk to EM or DE.

## Forward Security Reports

Determine all security reports for which fixes have are released and forward them to 1) EM 2) DE. They will then take care of publishing security notices. Find all such issues with the following JIRA query:

```
project = CAM AND fixVersion = <released version> AND type = "Security Report"
```

## Release Maven Central

We have several CI jobs which upload artifacts to Maven Central into their staging repository section. In order to make them publicly available, we need to manually close the staging repositories and release the artifacts.

1. Go to [Maven Central](https://oss.sonatype.org/) and login using the credentials found in our [Confluence](https://app.camunda.com/confluence/display/camBPM/Maven+Central+Release)
2. On the left side, click on the 'Staging Repositories' link.
3. After it has been loaded, scroll down to the bottom of the list. You should find the relevant Camunda staging repositories there.
4. Mark each repository you want to release.

    *For Example:*

    >org.camunda.bpm.webapp:camunda-webapp:7.14.0
    >org.camunda.bpm:camunda-root:7.14.0
    >org.camunda.bpm.assert.project:camunda-bpm-assert-root:7.14.0
    >org.camunda.bpm:camunda-external-task-client-root:1.4.0

5. Click on 'Release' at the menu on top of the list. A window will pop up were you can enter a description but it is not necessary. Activate 'Drop repository after release automatically'. Then proceed.
6. Done.

## Release Javadocs

1. The javadocs are staged automatically by the release build. Checkout if the javadocs are available at http://stage.docs.camunda.org/javadoc/camunda-bpm-platform/7.14 (you need to adapt the version in the link to the release version).
2. Verify that the correct javadocs are present.
3. Go to [Jenkins Camunda Docs Jobs](https://ci.cambpm.camunda.cloud/view/Docs/). After that, release the javadocs by triggering the the job `camunda-docs-release (javadoc-camunda-bpm-platform-7.14)` and `camunda-docs-release (javadoc-camunda-bpm-platform-7.15-SNAPSHOT)` (adapt versions).


## Release the Staged Docs

#### 1. Go to [Jenkins Camunda Docs Jobs](https://ci.cambpm.camunda.cloud/view/Docs/)
#### 2. Start job `camunda-docs-release (manual-7.14)`
#### 3. Start all other `camunda-docs-release (manual-7.X)` jobs
#### 4. As part of an minor release, the current `master` content needs to be released as `latest` at the [docs repository](https://github.com/camunda/camunda-docs-manual).
##### 4.1 Tag the docs
```
git checkout master
git pull
git tag -a 7.14.0 # the comment should be '7.14.0'
git push origin 7.14.0
```

##### 4.2 Release the tag as latest

Set the branch `latest` to the tag

```
git checkout latest
git reset --hard 7.14.0
```

Edit the config file on the branch

```diff
--- a/config.yaml
+++ b/config.yaml
@@ -1,5 +1,5 @@
 ---
-baseurl: "/manual/develop/"
+baseurl: "/manual/latest/"
 languageCode: "en-us"
 title: "Camunda BPM documentation"
 theme: "camunda"
@@ -20,7 +20,7 @@ params:
       url: "/enterprise"
   section:
     id: "manual"
-    version: "develop"
+    version: "latest"
     versionAlias: "7.14"
     versions:
       - "latest"
```

##### 4.3 Run `hugo` and check result

##### 4.4 Commit changes
```
git commit -a -m "chore(release): release master as latest"
```

##### 4.5 Force push to latest
```
git push -f origin latest
```

##### 4.6 Release Stage

* The content will be published to http://stage.docs.camunda.org/manual/latest/

* Make sure, that [this](https://ci.cambpm.camunda.cloud/view/Docs/job/docs/job/camunda-docs-manual-stage%20(latest,%20latest)/) build has already been triggered and successfully passed through.

* Once the release is staged there, release it by triggering the following build:
[camunda docs release](https://ci.cambpm.camunda.cloud/view/Docs/job/docs/job/camunda-docs-release%20(manual-latest)/)

* Now the new content is available in http://docs.camunda.org/manual/latest/

#### 5. Send a message to `#cambpm-announcements`

```
Hi all,

I have now released the current version of the docs as 7.14 and latest. Master has been bumped to 7.15.
If you are still documenting things that are relevant for 7.14, you need to

- commit to master
- cherry-pick to 7.14 branch
- cherry-pick to latest branch

Thanks
```

## Push Redirects / URL rewrites adjustments

1. Go the [camunda-docs-static](https://github.com/camunda/camunda-docs-static) repository, check that the branch `7.14` is available and [these steps](#adjust-redirects--url-rewrites) were performed.
2. Merge the `7.14` branch into master:
```
git checkout master
git merge 7.14
git push origin master
```

## Adjust Google Custom Search Engine

Talk to PM (or Daniel)
https://cse.google.com/cse/setup/basic?cx=007121298374582869478:yaec0vxmc7e

## Release the Getting Started

1. Trigger the [update-get-started-poms](https://release.cambpm.camunda.cloud/job/maintenance/job/update-get-started-poms/), do not forget to enter the correct versions.
2. Check the repos if everything is correct (including tags).
3. Go to [Jenkins Camunda Docs Jobs](https://ci.cambpm.camunda.cloud/view/Docs/)
4. Start job `camunda-docs-release (get-started)`

## Release the Enterprise Download Page

1. Go to [Jenkins Camunda Docs Jobs](https://ci.cambpm.camunda.cloud/view/Docs/)
2. Start job `camunda-docs-release (enterprise)`

## Release the Community Download Page

> Verify the standalone download links are updated: https://docs.camunda.org/manual/latest/installation/standalone-webapplication/#download

Sources are here: https://github.com/camunda/camunda.com-new/live

#### 1. Update ee and ce sections in [releases.json](https://github.com/camunda/camunda.com-new/blob/live/data/releases.json)

#### 2. Push it on `live`
```
git commit -am ":beer:(download) 7.14.0 released"
git push origin live
```

#### 3. Check the jenkins build at https://app.camunda.com/jenkins/view/All/job/camunda.com-new%20(live)/

#### 4. Check the result at http://camunda.com/

## Release the Blog Post

1. Before you publish it ensure that the `camunda.com` [download page](https://camunda.com/download/) is up to date. Note: The title of the blog post must be in the following format: Camunda 7.14.0 released.
2. Publish the blog post by merging the branch with the master:

```
git checkout master
git merge 7.14.0
```

## Publish the release

Let MKT, SUP and Niall (DEVREL) know that a new release is available by sending them the following email (Please **CC Daniel, Roman**):

```
Hi everyone,

we have published a new minor release:

* The version is 7.14.0.
* The link to the blog post is https://blog.camunda.org/post/<year>/<month>/camunda-bpm-7140-released
* The blog post is scheduled to go live on "BLOG_POST_RELEASE_DATE"
* The most noteworthy thing about this release is "COOLEST_FEATURE_IN_714"
* We propose to include the following image (XX Link) in the social media posts.

@marketing, Please share this on social media.
@Niall, Please create a post in the forum.
@Team-Support, Please inform our enterprise customers.

Best,
XX
```
(Also See: https://confluence.camunda.com/display/MAR/Social+Media)

## Send Release Notice
Once the blog post is online, send out an e-mail to alle@camunda.com similar to the [alpha release notice](https://github.com/camunda/camunda-bpm-dev-docs/blob/master/releases/Performing-an-Alpha-Release.md#send-release-notice).
Include links to the blog post, the release presentation Zoom room or recording of the presentation, the release notes and download link (https://docs.camunda.org/enterprise/download/).

## Improve this Guide

If you noticed any errors in the guides, any steps that were left out, please edit this document and keep it up to date.

Note: The version numbers in this guide should not be adjusted as it is easy to miss a place and create an inconsistent guide.

-----

# Phase 4

* time: when the artifacts are built

## Camunda BPM Platform code changes

### Update "old version" in maven

Update the `camunda.version.old` property in:
* camunda-bpm-platform:
  * [database pom][database-pom]
  * [qa/create new engine (here: `version.previous`)](https://github.com/camunda/camunda-bpm-platform/blob/master/qa/test-db-rolling-update/create-new-engine/pom.xml)
  * [qa/test-db-upgrade](https://github.com/camunda/camunda-bpm-platform/blob/master/qa/test-db-upgrade/pom.xml)
* [camunda-bpmn-model][bpmn-model-pom]
* [camunda-cmmn-model][cmmn-model-pom]
* [camunda-dmn-model][dmn-model-pom]
* [camunda-xml-model][xml-model-pom]

to the previous minor version.

If the minor release includes a new spin release, update the `spin.version.old` properties in [camunda/camunda-spin][spin-pom].
If the minor release includes a new connect release, update the `connect.version.old` properties in [camunda/camunda-connect][connect-pom].

Update the `camunda.version` property in:

* camunda-bpm-platform:
  * [qa/test-db-rolling-update/create-old-engine](https://github.com/camunda/camunda-bpm-platform/blob/master/qa/test-db-rolling-update/create-old-engine/pom.xml)
  * [qa/test-db-rolling-update/rolling-update-util](https://github.com/camunda/camunda-bpm-platform/blob/master/qa/test-db-rolling-update/rolling-update-util/pom.xml)
  * [qa/test-db-rolling-update/test-old-engine](https://github.com/camunda/camunda-bpm-platform/blob/master/qa/test-db-rolling-update/test-old-engine/pom.xml)
  * [ qa/test-old-engine/](https://github.com/camunda/camunda-bpm-platform/blob/master/qa/test-old-engine/pom.xml)

### Add instance migration project

When the new version is 7.x, a new instance migration project that performs upgrade logic from 7.(x-1) to 7.(x) has to be added. Perform the following steps:

* In the [instance-migration project][instance-migration] and its most recent `test-fixture` project, set the property `camunda.version.current` to `7.(x-1).0`
* Add a new module called `test-fixture-7(x)`
* Configure the new module such that it executes patch scripts and upgrade scripts from `7.(x-1).0` to `7.(x).0-SNAPSHOT` (take a look at other `test-fixture` projects)
* Add the module to the [instance migration pom][instance-migration-pom]

### Create upgrade scripts

In the [sql-scripts][sql-scripts] folder of the [camunda/camunda-bpm-platform][camunda-bpm-platform]:

1. Create an empty upgrade script for each supported database following the naming pattern ```${database}_engine_${previousVersion}_to_${currentVersion}```.
2. Follow this [SQL Development Guide](https://github.com/camunda/camunda-bpm-dev-docs/blob/master/development/SQL-and-DDL-Management.md#development-1) to set the initial content of the new scripts.

[camunda-bpm-platform]: https://github.com/camunda/camunda-bpm-platform/tree/master
[sql-scripts]: https://github.com/camunda/camunda-bpm-platform/tree/master/engine/src/main/resources/org/camunda/bpm/engine/db
[database-pom]: https://github.com/camunda/camunda-bpm-platform/blob/master/database/pom.xml
[instance-migration]: https://github.com/camunda/camunda-bpm-platform/tree/master/qa/test-db-instance-migration
[instance-migration-pom]: https://github.com/camunda/camunda-bpm-platform/tree/master/qa/test-db-instance-migration/pom.xml
[bpmn-model-pom]: https://github.com/camunda/camunda-bpmn-model/blob/master/pom.xml
[cmmn-model-pom]: https://github.com/camunda/camunda-cmmn-model/blob/master/pom.xml
[dmn-model-pom]: https://github.com/camunda/camunda-dmn-model/blob/master/pom.xml
[xml-model-pom]: https://github.com/camunda/camunda-xml-model/blob/master/pom.xml
[spin-pom]: https://github.com/camunda/camunda-spin/blob/master/pom.xml
[connect-pom]: https://github.com/camunda/camunda-connect/blob/master/pom.xml

### Example commit for above code changes

See this [commit](https://github.com/camunda/camunda-bpm-platform/commit/df0df1688e7582eb89ade6a1fc004505501822b8) for [camunda-bpm-platform][camunda-bpm-platform].

## Pick Alpha Release Manager

The Release Manager Rotation is Round-Robin based on the [Community Worker Rotation](https://app.camunda.com/confluence/display/camBPM/Community+Worker+Process#CommunityWorkerProcess-CommunityWorkerRotation:).

After the Release Manager is picked, update the `CamBPM Release Manager` user group in Slack. To find it, go to:
1. `People` (in the left side-panel)
2. `User groups` tab
3. Locate and click on `CamBPM Release Manager`
4. Click on `Edit Members`
5. Remove the current Release Manager and add the new one.
