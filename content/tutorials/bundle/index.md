---
title: "Getting started with the Gatling bundle"
menutitle: "Bundle"
slug: "getting-started-with-gatling-bundle"
description: "Learning how to setup FrontLine Cloud using the Gatling bundle"
lead: ""
date: 2021-03-19T17:03:31+01:00
lastmod: 2021-03-19T17:03:31+01:00
draft: false
images: []
menu:
  tutorials:
    parent: "Getting started"
weight: 50
contributors: []
---

{{< include introduction.md >}}

There are multiple ways to create a Gatling artifact, which will be used by FrontLine to launch a run. We can use either a build tool (Maven, Sbt, Gradle) or the Gatling bundle.

In this tutorial, we'll use the Gatling bundle. Make sure that you have a JDK 8 or 11 installed on your computer.

## Download the zip bundle
[`Click here to download the latest version of the Gatling bundle`](https://gatling.io/open-source/start-testing) and extract the archive.

## Download and copy the extra script

* On Windows: [`click here to download this artifact.bat`](https://raw.githubusercontent.com/gatling/gatling/master/gatling-bundle/src/universal/bin/artifact.bat).
* On Linux or MacOS: [`click here to download this artifact.sh`](https://raw.githubusercontent.com/gatling/gatling/master/gatling-bundle/src/universal/bin/artifact.sh).

Copy this file in the `bin` directory of the decompressed archive.

{{< img src="bundle.png" alt="Launch artifact.bat" >}}

## Generate the artifact

Run the `artifact.bat` or `artifact.sh` file, Gatling will start creating our first artifact!

{{< include upload.md "target/artifact.jar" >}}

## Upload the artifact

Upload it to FrontLine, either by drag-and-dropping it to the modal, or by clicking on the modal to open the file manager.

{{< img src="choose_artifact.png" alt="Choose the generated artifact" >}}

We now have to click on the Upload button to upload it to FrontLine. After a few seconds, the upload will be complete, and our artifact will be successfully ready to use!

{{< img src="upload.png" alt="Start the upload" >}}

{{< include simulation_1.md "computerdatabase.BasicSimulation" >}}

{{< img src="create_simulation_step_1.png" alt="General configuration" >}}

{{< include simulation_2.md "frontline.sample.BasicSimulation" >}}

