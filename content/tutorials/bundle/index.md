---
title: "Getting started with the Gatling bundle"
menutitle: "Bundle"
slug: "getting-started-with-gatling-bundle"
description: "Learn how to setup Gatling Enterprise using the Gatling bundle"
date: 2021-03-19T17:03:31+01:00
lastmod: 2021-03-19T17:03:31+01:00
weight: 50
---

{{< include introduction.md >}}

## Download the zip bundle

There are multiple ways to create a Gatling artifact, which will be used by Gatling Enterprise to launch a run. 
We can use either a build tool (Maven, SBT, Gradle) or the Gatling bundle.

In this tutorial, we'll use the **Gatling bundle**. Make sure that you have a JDK 8 or 11 installed on your computer.

[`Click here to download the latest version of the Gatling bundle`](https://gatling.io/open-source/#downloadgatling) and extract the archive.

## Download and copy the extra script

* On Windows: [`click here to download this artifact.bat`](https://raw.githubusercontent.com/gatling/gatling/master/gatling-bundle/src/universal/bin/artifact.bat).
* On Linux or MacOS: [`click here to download this artifact.sh`](https://raw.githubusercontent.com/gatling/gatling/master/gatling-bundle/src/universal/bin/artifact.sh).

Copy this file in the `bin` directory of the decompressed archive.

{{< img src="bundle.png" alt="Launch artifact.bat" >}}

## Generate the artifact

Run the `artifact.bat` or `artifact.sh` file, Gatling will start creating your first artifact!

## Upload the artifact 

{{< include upload_manual.md "target/artifact.jar" >}}

Upload it to Gatling Enterprise, either by drag-and-dropping it to the modal, or by clicking on the modal to open the file manager.

{{< img src="choose_artifact.png" alt="Choose the generated artifact" >}}

Click on the Upload button. After a few seconds, the upload will complete, and your artifact will be ready to use!

{{< img src="upload.png" alt="Start the upload" >}}

## Create a simulation

{{< include simulation_1.md "computerdatabase.BasicSimulation" >}}

{{< img src="create_simulation_step_1.png" alt="General configuration" >}}

{{< include simulation_2.md "frontline.sample.BasicSimulation" >}}

## Start load testing!

{{< include run.md >}}

## Additional resources

{{< include resources.md >}}

