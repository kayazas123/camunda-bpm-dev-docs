Process:

<img src="img/releases/alphaRelease.png" align="left" >


Steps:
- [ ] [Define release version](#define-release-version) (*)
- [ ] [Make calendar appointments](#make-calendar-appointments) (*)
- [ ] [Prepare the blogpost](#prepare-the-blogpost) (*)
- [ ] [Submit the blogpost to marketing](#submit-the-blogpost-to-marketing) (CM,CR)
- [ ] [Check Preconditions](#check-preconditions) (*)
- [ ] [Trigger the release build](#trigger-the-release-build) (*)
- [ ] [Update the Enterprise Download Page](#update-the-enterprise-download-page) (MS,*)
- [ ] [Test Release](#test-release) (MS, *)
- [ ] [Release Maven Central](#release-maven-central) (*)
- [ ] [Release the documentation (manual)](#release-the-documentation-manual) (*)
- [ ] [Release Javadocs](#release-javadocs) (*)
- [ ] [Release Jira](#release-jira) (*)
- [ ] [Forward Security Reports](#forward-security-reports) (*)
- [ ] [Check Docker Images](#check-docker-images) (*)
- [ ] [Publish the Enterprise Page](#publish-the-enterprise-page) (*)
- [ ] [Send Release Notice](#send-release-notice) (*)
- [ ] [Forum Announcement](#forum) (*)
- [ ] [Improve this guide](#improve-this-guide) (*)
- [ ] [Celebrate the release](#present-and-celebrate-the-release) (*)
- [ ] [Pick Next Release Manager](#pick-next-release-manager) (*)

Steps marked with (*) are to be performed by the "Release Manager".
MS = Michael Schöttes \
TL = Thorben Lindhauer \
RS = Roman Smirnov \
CM = Charley Mann \
CR = Christopher Rogers

# Define release version 
There are five alpha releases before every minor release named alpha1 to alpha5. Major, minor, and patch versions are the same as the next minor release. The format of the version is `<MAJOR>.<MINOR>.<PATCH>-<ALPHA>` (e.g. 7.9.0-alpha3, 7.12.0-alpha5, ...). More information can be found in the [release policy](https://docs.camunda.org/enterprise/release-policy/) in the docs.

This guide contains links and filters using placeholders. Please replace the placeholder with the corresponding substitute.

# Make calendar appointments

**Heads-up:** The release presentation is organized company-wide by the `@product-release-presentation-dri`. The DRI will create the Zoom meeting & the calendar appointment. Read all details about the company-wide release presentation organization on [confluence](https://confluence.camunda.com/display/camBPM/Release+Presentation+Organization).

## **Hold Release presentation**
> 🕔 Make an appointment 15 minutes before the release presentation until its end. Audience: camundabpm@camunda.com

## **Blog post contribution**
> 🕔  set a reminder in the evening of the day before the code freeze, so everyone has their blogpost contribution committed.
> Audience: camundabpm@camunda.com

## **Code Freeze**
> 🕔  in the evening of the day before the build will take place until its end. If the release presentation is on Tuesday, the code freeze can be scheduled for Friday evening.
> Audience: camundabpm@camunda.com

## **Build release**
> 🕗  as the duration of the build lasts at least two hours, make sure to trigger it early in the morning after the code freeze and keep in mind that it might fail. Make sure you have no other appointments on the day of the release.
> Audience: camundabpm@camunda.com, infra@camunda.com

## **Celebrate release**
> Please see [Celebrate The Release](#present-and-celebrate-the-release).

# Prepare the Blogpost

Goal: Prepare a blogpost skeleton as a draft on WordPress, such that your colleagues can contribute their sections.


## Create a draft on WordPress

1. Open https://camunda.com/wp-admin/ & log in with your WordPress credentials (ask David Paradis if you don't have credentials yet)
2. Click on "Posts" in the left sidebar
3. Click on "Add New"
4. Add "Camunda BPM Runtime 7.Y.0-alphaZ Released" as title
5. Add the blog post skeleton
6. In the right sidebar, open the dropdown "Status & visibility" and choose "Camunda BPM Team" as author
7. Open the "Categories" dropdown and select the following checkboxes `Camunda BPM`, `Product News` & `Release Notes`
8. Click on "Preview" to see a preview of the blog post

## Encourage Content

Ask your colleagues to contribute content for any noteworthy features they have implemented. They can log in with their WordPress credentials and edit the draft blog post.

# Submit the blogpost to Marketing

Some days before the release, submit the blogpost to marketing so that they have the chance to proofread it:

1. Open the blog post in WordPress
2. Click on the "Submit for Review" button in the upper right corner
3. Send an email to `marketing@camunda.com` to communicate at which date the blog post should be published (they will also take care of publishing the release on our social media channels)

# Check Preconditions

- There are no Snapshot dependencies on secondary projects like Spin or Connect ([Release procedure](https://github.com/camunda/camunda-bpm-dev-docs/blob/master/releases/Release-secondary-projects.md)). For that, check [Root pom.xml](https://github.com/camunda/camunda-bpm-platform/blob/master/pom.xml) for snapshot dependencies (All snapshot dependencies contain the word 'SNAPSHOT' in their name).
- There are no code problems ([check ci](https://broken.cambpm.camunda.cloud/) section "Master" and "Release")
- [Release Test Job](https://release.cambpm.camunda.cloud/view/Release-Test/) passed successfully recently
- Post a message to `#cambpm-announcements` and `@cambpm-dri` (INFRA) on Slack:
```
Hey,

Please stop pushing to master since we are going to build the alpha release.
```

# Trigger the Release Build

Open the following URL in Jenkins:

https://release.cambpm.camunda.cloud/view/Release-Master/

Click on the "Run" icon. Make sure to set the configuration to something like:

```
DEVELOPMENT_VERSION: 7.9.0-SNAPSHOT
RELEASE_VERSION:     7.9.0-alpha3
PUSH_REMOTE:         true
RELEASE_TYPE:        ALPHA
OVERRIDE_TAG:        false
SKIP_TESTS:          true
SKIP_DEPLOY:         false
BRANCH:              master
```

Click Build.
Wait for the following jobs to turn green before continuing with the next step (check, e.g. [here](https://release.cambpm.camunda.cloud/view/Release-Master/builds)):


* 7.9-RELEASE-build-camunda-bpm-CE-tags
* 7.9-RELEASE-build-camunda-bpm-EE-tags

Please also check that there are no issues at the Jenkins 'Release' build (see [here](https://broken.cambpm.camunda.cloud/)).

# Update the Enterprise Download Page

Add the latest release to the following page [`camunda-docs-static/enterprise/content/download.md`](https://github.com/camunda/camunda-docs-static/blob/master/enterprise/content/download.md).

All releases are handled as HUGO page variables in the document header. Add the release in the following format:

```
  branches:
  - branch: "<minor-version>"
    releases:
    - number: "<release-version>"
      note: "<link-to-blog-post>"
      date: "<release-date>"
```

Make sure that you have set the correct minor version (e.g., 7.9) and release version (e.g., 7.9.0-alpha3).

# Release the Enterprise Page

Commit the changes and build the enterprise page:

```
git commit -m "chore(download): add 7.9.0-alpha3 to download page"

git push origin master
```

> Review your commit on http://stage.docs.camunda.org/enterprise/download/

> Make sure that the link to the release notes works as expected. To generate it, go to [this page](https://app.camunda.com/jira/secure/ReleaseNote.jspa).


# Test Release

For each container (including Spring Boot & Run), one developer should perform a test. Download the release artifacts from http://stage.docs.camunda.org/enterprise/download/

## Preparations

* Make sure the Docker QA images are ready to use
  1. Check if the Jenkins Job was executed successfully (if not retrigger it)
     * You can find the job here: `https://ci.cambpm.camunda.cloud/view/all/job/<minor-version>/job/<minor-version>-platform-docker-qa/`
       * **Heads-up:** The Docker QA job in the release pipeline only triggers the actual job and does not indicate the fail/success of the actual job
     * If the job continues to fail, you could try to build & push the images locally from your machine. Please see the documentation about [Manually Build & Push the Docker QA Images](https://github.com/camunda/camunda-bpm-dev-docs/blob/master/howtos/manually-build-docker-qa-images.md)
  2. Promote the versions of the Docker images in the [Portainer Templates Repository](https://github.com/camunda-ci/portainer-templates).
     * You can find an example commit here: https://github.com/camunda-ci/portainer-templates/commit/d94033
  3. To confirm that everything works as expected, go to Portainer, run a container for each image, and check the "Stack Details". You should validate the right version is picked up, and the container is "runnable". Please see the following screenshot:\
   ![Portainer Stack Details](https://raw.githubusercontent.com/camunda/camunda-bpm-dev-docs/master/howtos/img/manually-build-docker-qa-images-portainer-stack-details.png)
     * The step requires a VPN connection. Find all the details here: https://confluence.camunda.com/display/ADMIN/VPN
     * The how-to for running the containers can be found [here](https://github.com/camunda/camunda-bpm-dev-docs/blob/master/howtos/was-wls-autosetup.md)
* Provide a test plan sheet as a copy from [the template](https://docs.google.com/spreadsheets/d/1K9xRFix6NFjnFJDVailOYPTkzJyQCA9yIrFcyFtC3KE/edit#gid=1656336280),
make sure to prepare the server/database combinations to be tested before the next step (Conduct Testing). Assign combinations of application servers and databases to devs. Make sure the major servers and databases are tested once. It is not necessary to test multiple combinations of those.
For an alpha, the coverage of JDKs, browsers, and operating systems is not so important so that this choice can be left to the devs conducting the test.

**Only do this for the first alpha of a minor release (i.e. the first alpha after a minor release)**

There won't be any Docker stack files for the new minor version in the Portainer Templates when the first alpha is built.

To make the new WAS & WLS Docker QA images available on Portainer, perform the following steps:

1. Add the new Docker stack files for WAS & WLS. You can use [this commit](https://github.com/camunda-ci/portainer-templates/commit/b0cd9b048e25820b0e4215b7522bb8fa3f563151) as an example.
2. Include the new stacks in the lists of stack templates, like in [this commit](https://github.com/camunda-ci/portainer-templates/commit/7efd7a5f1e5b454c8a33dd7f8ecbb4e30a8eab66).

_Note: The changes can be made in a single commit._

## Conduct Testing

1. Send a message in Slack (#cambpm-internal) asking the team to participate in manual testing:
```
Hey team,

Please join me in testing the Camunda BPM Runtime 7.14.0-alpha2 release:

1. Test the server/database combination assigned to your name and document your JDK version, browser, OS and progress in the [Release Test Spreadsheet](<link>)
2. You can download the distros on the [Enterprise Download Page](https://stage.docs.camunda.org/enterprise/download/)
3. Perform the [Standard Regression Test](https://github.com/camunda/camunda-bpm-dev-docs/blob/master/releases/Performing-an-Alpha-Release.md#standard-regression-test)
4. If you have found a bug/regression, please create a ticket and link it in the Notes column in the [Release Test Spreadsheet](<link>)
```
2. Keep track of environments and test results in the test plan sheet.

### Standard Regression Test
1. Download the release artifact from the enterprise download page
2. Combine the platform with a database of choice.
3. Start the platform
    1. Check the server log that no exceptions occur
4. Open Admin
    1. Check if the dashboard and main menu contain the sections 'Users', 'Groups', 'Tenants', 'Authorizations', 'System'
    2. Add a license key to the platform
5. Open Tasklist
    1. Start a new invoice process
    2. Walkthrough the invoice process step by step
6. Open Cockpit
    1. Use the dashboard search plugin to find all finished invoice instances (There should be at least one)
    2. Go to the runtime instance view of the invoice showcase
    3. Switch to the history view
    4. Migrate all instances from the invoice process version 1 to version 2
    5. Use the dashboard search plugin to find all instances started after a certain date
    6. Cancel these instances by using the batch operations
    7. Select a process instance and go to the process instance view
    8. Perform a process instance modification
    9. Add a variable to the process instance

#### Testing with Spring Boot
Use the spring-boot-example [example webapp ee](https://github.com/camunda/camunda-bpm-examples/tree/master/spring-boot-starter/example-webapp-ee) as a test scenario.

Check out compatible versions for the release --> [Spring Boot Version Compatibility](https://docs.camunda.org/manual/develop/user-guide/spring-boot-integration/version-compatibility/).

Change the pom.xml file:
```
<properties>
  <camunda.version>7.X.0-alphaY-ee</camunda.version>
  <spring.boot.version>X.Y.Z.RELEASE</spring.boot.version> <!-- make sure to test the latest supported Spring Boot version -->
</properties>
```


Enable Groovy Script Engine if necessary

```
<!-- https://mvnrepository.com/artifact/org.codehaus.groovy/groovy-jsr223 -->
<dependency>
    <groupId>org.codehaus.groovy</groupId>
    <artifactId>groovy-jsr223</artifactId>
    <version>2.4.13</version>
</dependency>
```

##### Using the Invoice example in Spring Boot

The Spring Boot Starter - Invoice example integrates the Invoice application with the Camunda Spring Boot Starter. It
can be found at [https://github.com/camunda/camunda-bpm-examples/tree/master/spring-boot-starter/example-invoice](https://github.com/camunda/camunda-bpm-examples/tree/master/spring-boot-starter/example-invoice).

The example adds the necessary JDBC dependencies, as well as the `application.yaml` configuration to run on one of
the Camunda-supported databases. By default, H2 will be used, while the rest can be un-commented. 


### Release Specific Test

According to the implemented feature topics, choose some of the new features to test them manually.
If you don't know which fixes are worth the effort use the following rule of thumb:
* Prefer UI features as the test coverage is usually lower compared to backend features. Use a different browser for the test.
* Prefer features related to customer support issues as our customers expect that these features are working correctly.

If you are satisfied with the release, post a message to `#cambpm-announcements` on Slack:

```
Hey,

the release test passed, you can commit to master again :)

```

# Release Maven Central

> Precondition release tests are passed.

We have several CI jobs that upload artifacts to Maven Central into their staging repository section. To make them publicly available, we need to close the staging repositories and release the artifacts manually.

1. Go to [Maven Central](https://oss.sonatype.org/) and log in using the credentials found in our [Confluence](https://app.camunda.com/confluence/display/camBPM/Maven+Central+Release)
2. On the left side, click on the 'Staging Repositories' link.
3. After it has been loaded, scroll down to the bottom of the list. You should find the relevant Camunda staging repositories there.
4. Mark each repository you want to release.

    *For Example:*  

    >org.camunda.bpm:camunda-root:7.13.0-alpha5
	  >org.camunda.bpm.webapp:camunda-webapp:7.13.0-alpha5

5. Click on 'Release' at the menu on top of the list. A window will pop up where you can enter a description, but it is not necessary. Activate 'Drop repository after release automatically'. Then proceed.
6. Done.

Hint: It takes about two hours until the artifacts can be found on Maven Central.

# Release the Documentation (Manual)

As part of an alpha release, the current `master` content needs to be released as `latest` at the [docs repository](https://github.com/camunda/camunda-docs-manual).

#### 1. Tag the docs

```
git checkout master
git pull
git tag -a 7.9.0-alpha3 # the comment should be '7.9.0-alpha3'
git push origin 7.9.0-alpha3
```

#### 2. Release the tag as latest

Set the branch `latest` to the tag

```
git checkout latest
git reset --hard 7.9.0-alpha3
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
     versionAlias: "7.5"
     versions:
       - "latest"
```

#### 3. Run `hugo` and check result

#### 4. Commit changes
```
git commit -a -m "chore(release): release master as latest"
```

#### 5. Force push to latest
```
git push --force-with-lease origin latest --tags
```

#### 6. Release Stage

* The content will be published to http://stage.docs.camunda.org/manual/latest/

* Make sure, that [this](https://ci.cambpm.camunda.cloud/view/Docs/job/docs/job/camunda-docs-manual-stage%20(latest,%20latest)/) build has already been triggered and successfully passed through.

* Once the release is staged there, release it by triggering the following build:
[camunda docs release](https://ci.cambpm.camunda.cloud/view/Docs/job/docs/job/camunda-docs-release%20(manual-latest)/)

* Now the new content is available in http://docs.camunda.org/manual/latest/

#### 7. Update redirects

**Only do this for alpha1 (i.e., the first alpha after a minor release)**

In `camunda-docs-static`, edit the redirects

```diff
--- a/config/live/.htaccess
+++ b/config/live/.htaccess
@@ -12,7 +12,7 @@ RewriteRule ^latest/api-references/java/ /manual/7.3/reference/javadoc/

 # Javadoc
 RewriteRule ^manual/develop/reference/javadoc/$ /javadoc/camunda-bpm-platform/7.12-SNAPSHOT [R=307,L]
-RewriteRule ^manual/latest/reference/javadoc/$ /javadoc/camunda-bpm-platform/7.11 [R=307,L]
+RewriteRule ^manual/latest/reference/javadoc/$ /javadoc/camunda-bpm-platform/7.12 [R=307,L]
 RewriteRule ^manual/([^/]+)/reference/javadoc/$ /javadoc/camunda-bpm-platform/$1 [R=307,L]

 # Manual without version
diff --git a/config/stage/.htaccess b/config/stage/.htaccess
index 972f0b6..e35ffde 100644
--- a/config/stage/.htaccess
+++ b/config/stage/.htaccess
@@ -7,7 +7,7 @@ RewriteRule ^latest/api-references/java/ /manual/7.3/reference/javadoc/

 # Javadoc
 RewriteRule ^manual/develop/reference/javadoc/$ /javadoc/camunda-bpm-platform/7.12-SNAPSHOT [R=307,L]
-RewriteRule ^manual/latest/reference/javadoc/$ /javadoc/camunda-bpm-platform/7.11 [R=307,L]
+RewriteRule ^manual/latest/reference/javadoc/$ /javadoc/camunda-bpm-platform/7.12 [R=307,L]
 RewriteRule ^manual/([^/]+)/reference/javadoc/$ /javadoc/camunda-bpm-platform/$1 [R=307,L]

 # Manual without version
```

Commit and push

```bash
git add -A
git commit -m "release 7.12.0-alpha1"
git push origin master
```

# Release Javadocs

The javadocs are staged automatically by the release build at ``http://stage.docs.camunda.org/javadoc/camunda-bpm-platform/<minor-version>``

Verify that the correct javadocs are present.

After that, release the javadocs by triggering the following build:

``https://ci.cambpm.camunda.cloud/view/Docs/job/docs/job/camunda-docs-release%20(javadoc-camunda-bpm-platform-<minor-version>)/``

Now the new javadocs are available here: ``https://docs.camunda.org/javadoc/camunda-bpm-platform/<minor-version>``

Make sure that you have set the right minor version within all the links. (e.g. 7.9, 7.12)

adjust the version in the URL, e.g., 7.9 or 7.12

# Release JIRA
The goal for releasing the JIRA is to add the version you would like to release to issues that have already been closed. To achieve this, please make sure that the alpha version you would like to release [already exists](https://app.camunda.com/jira/browse/CAM/?selectedTab=com.atlassian.jira.jira-projects-plugin:versions-panel). If not, ask Michael, Roman or Thorben to create this alpha version in JIRA.

To add the release version to the respective "fixVersion" field of all issues which have already been closed, go to the [issues page](https://app.camunda.com/jira/issues) and make sure that the advanced search is enabled.

Put the following query in the search box:

```
project = CAM AND fixVersion = <minor-version> AND fixVersion != <release-version> AND status = Closed AND fixVersion NOT IN(<minor-version>-alpha1, <minor-version>-alpha2)
```
Make sure that you have set the right minor version within the query. (e.g. 7.9.0, 7.12.0)

1. Click on "Tools -> all .. issue(s)".
1. Select all issues and click on "Next".
1. Select "Edit Issues" and click on "Next".
1. Select "Change Fix Version/s" and make sure that "Add to existing" is selected in the dropdown.
1. Select the version you would like to release.
1. Make sure that "Send mail for this update" is unchecked (bottom of the page).
1. Click on "Next".
1. Click on "Confirm".
1. After the completion of the bulk operation, click on "Ok, got it".

Put the following query in the search box of the [issues page](https://app.camunda.com/jira/issues):
```
project = CAM AND fixVersion = <release-version>
```
Perform a sanity check of the released issues by making sure that the `Fix Version/s` only includes the 
* current alpha version (e.g., 7.13.0-alpha2)
* the current minor version (e.g., 7.13.0)
* backport versions (optional, e.g., 7.12.3, 7.11.9)

If you have any questions, feel free to approach Thorben. If Thorben is not available, you can ask Michael and/or Roman.

# Forward Security Reports

Determine all security reports for which fixes have are released and forward them to 1) Thorben 2) Roman. They will then take care of publishing security notices. Find all such issues with the following JIRA query:

```
project = CAM AND fixVersion = <release-version> AND type = "Security Report"
```

# Check Docker Images

Verify that the docker images [CE](https://hub.docker.com/r/camunda/camunda-bpm-platform/) and [EE](https://docs.camunda.org/manual/latest/installation/docker/#enterprise-edition) are built.

* CE job successfully run - ``https://ci.cambpm.camunda.cloud/job/<minor-version>/job/<minor-version>-platform-docker-ce/``
* EE job successfully run - ``https://ci.cambpm.camunda.cloud/job/<minor-version>/job/<minor-version>-platform-docker-ee/``

Make sure that you have set the right minor version within the link. (e.g. 7.9, 7.12)

# Publish the Enterprise Page

Check for unpublished commits (of other teams, e.g., Optimize) in [docs-static](https://github.com/camunda/camunda-docs-static/tree/master/enterprise), if there are some, sync with them if it is ok to publish the page.
Release the Enterprise documentation by triggering the following build:
https://ci.cambpm.camunda.cloud/view/Docs/job/docs/job/camunda-docs-release%20(enterprise)/

# Send Release Notice

Let everyone know that a new release is available by sending the following email to `alle@camunda.com` with CC to `team-support@camunda.com`:

```
Hi everyone,
 
We have released Camunda BPM Runtime <release-version> Today. Find all the details below: 

Blog Post: <link-to-blog-post>
Release Presentation: <presentation-date, e.g. Feb 28, 2020 02:00 PM (Berlin time)>, Join the presentation on Zoom: <link-to-zoom-call>
Release Notes: <link-to-release-notes> 
Download: https://docs.camunda.org/enterprise/download/

@Team-Support Please include this release in the next customer notification

Best,
The Runtime Team
```

Concerning the Support Team:

You will receive a test email that needs to be approved.
⚠️ If you receive an Out of Office notification, you can ask Michael from QA to send the email.

# Forum

Announce the release in https://forum.camunda.org by posting in the Announcements category linking to the blogpost (adapt elements in bold):

```
Title: Camunda BPM Runtime <release-version> released

Category: Announcements

Text:

Hi all,

Camunda BPM Runtime <release-version> has been released today.
Read all about it on our blog: <link-to-blog-post>.
```

# Improve this Guide

If you noticed any errors in the guides, any steps that were left out, please edit this document and keep it up to date. For changes to the release procedure, create a ticket in the [CAMTEAM](https://app.camunda.com/jira/browse/CAMTEAM) project, and follow the PR base workflow.

Note: For the version numbers used here, it is ok to leave in older versions.

# Present and Celebrate the Release

Present the release remotely via Zoom. If you like, you can moderate the presentation. If you prefer someone else do it (which is perfectly fine), please ask Thorben. All presenters should be present and be able to participate in a Zoom conference actively.

## Requirements

* talk **before** with the people who should present the new features
* ask those people to set up demos on their machines that they can share on screen

# Pick Next Release Manager

The Release Manager Rotation is Round-Robin based on the [Community Worker Rotation](https://app.camunda.com/confluence/display/camBPM/Community+Worker+Process#CommunityWorkerProcess-CommunityWorkerRotation:).

After the Release Manager is picked, update the `CamBPM Release Manager` user group in Slack. To find it, go to:
1. `People` (in the left side-panel)
2. `User groups` tab
3. Locate and click on `CamBPM Release Manager`
4. Click on `Edit Members`
5. Remove the current Release Manager and add the new one.
