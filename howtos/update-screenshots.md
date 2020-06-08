# How to update screenshots using automation

This guide shows you how to update webapp screenshots.

## Setup
Requirements:
- NodeJS >= 8 
- Google Chrome 

Before you start, clone or pull the latest versions of the [platform ee](https://github.com/camunda/camunda-bpm-platform-ee) and [docs manual](https://github.com/camunda/camunda-docs-manual/). Both repositories should be checked out in the same folder.

## Generation

1. Navigate to `camunda-bpm-platform-ee/webapps/camunda-webapp/plugins`
2. Install webapp dependencies `npm i`
3. Start the development server `mvn clean jetty:run -Pdevelop` and build the webapps `grunt build`
4. Run `grunt screenshots`. Check the console for errors.
5. The screenshots are placed in the docs manual folder. Check that they are in the correct place and that the images are correct.
6. Commit and push to docs-manual.