# Creating Maven Modules

This page lists some things that need to be considered when adding Maven modules to an existing project, so that releases and CI work correctly. This can be the case when you create a new product module or when you merge repositories.

## Ensure Maven Central Release

Goal: The artifact is published to Maven central during a release build.

Risk: If this does not work, then we may have to redo the release build.

Context: Applies only to community edition modules that are supposed to be accessible by our users. 

Solution: Make sure that the Maven modules are covered by the release builds, so that artifacts will become available on Maven central. E.g. for camunda-bpm-platform, make sure the new modules are covered by the sonatype-oss-release profile.

## Ensure Snapshot Version Update During Release

Goal: Jenkins updates the release and snapshot versions in the parent POM reference of all modules during a release build. In other words: The module is included in commits like https://github.com/camunda/camunda-bpm-platform/commit/544a30f0cfa67fcdd664f58cff5ca6dd6a20f4d6 and https://github.com/camunda/camunda-bpm-platform/commit/1d19ab6b79868a1e9b98b7046dd1cd9f7abd7a26. 

Risk: The POMs keep referencing old snapshots that are eventually removed from Nexus.

Context: Jenkins only updates the versions for modules that are included in the release build.

Solution: Make sure that the new module is included in the release build. Check the release build's profiles to verify that.

## Ensure That the Module is Covered by the Right Assembly Build

Goal: The new module is included in the right assembly build.

Risk: If a module is located in the wrong build, it may use outdated snapshot dependencies (when it depends on a snapshot that is created by a later build in the pipeline) and after a release the build may fail entirely (as no such snapshot exists yet for the next version).

Context: Currently we have four assembly builds for each branch:

* 7.14-platform-ASSEMBLY: Builds the open-source modules that have no webapp dependencies (e.g. camunda-engine)
* 7.14-webapp-DISTRO: Builds the webapp
* 7.14-platform-DISTRO: Builds the open-source modules that have a webapp dependency (e.g. Tomcat distro, Spring Boot Starter Web)
* 7.14-EE-platform-DISTRO: Builds the EE modules

Solution: Include the modules in the correct builds. If a module is open-source and has a webapp dependency, it must go into 7.14-platform-DISTRO, etc. Check the Maven profiles that the builds use to control this.