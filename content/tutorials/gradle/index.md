---
title: "Getting started with Gradle"
description: "Learning how to setup FrontLine Cloud using Gradle"
lead: ""
date: 2021-03-19T17:03:31+01:00
lastmod: 2021-03-19T17:03:31+01:00
draft: false
images: []
weight: 50
contributors: []
---

{{< include introduction.md >}}

{{< include download_samples.md >}}

## Create an artifact

There are multiple ways to create a Gatling artifact, which will be used by FrontLine to launch a run. We can use either a build tool (Maven, Sbt, Gradle) or the Gatling bundle.

In this tutorial, we'll use the Gradle build tool, so make sure you have Gradle configured. We'll use Gradle from the terminal, but you can also do it easily with an IDE like IntelliJ.

Extract the archive, then navigate to the gradle folder and open a terminal there. This folder contains a basic Gatling simulation which can be used with FrontLine.

Execute the following command to create an artifact:

```shell
gradle frontLineJar
```

{{< img src="gradle_command.png" alt="Maven command" >}}

Gradle will download the necessary dependencies and package our simulation. That's it, we created our first Gatling artifact!

{{< include upload.md "frontline-samples/gradle/build/libs/gradle.jar" >}}

Upload it to FrontLine, either by drag-and-dropping it to the modal, or by clicking on the modal to open the file manager.

{{< img src="choose_artifact.png" alt="Choose the generated artifact" >}}

We now have to click on the Upload button to upload it to FrontLine. After a few seconds, the upload will be complete, and our artifact will be successfully ready to use!

{{< img src="upload.png" alt="Start the upload" >}}

{{< include simulation_1.md "frontline.sample.BasicSimulation" >}}

{{< img src="create_simulation_step_1.png" alt="General configuration" >}}

{{< include simulation_2.md "frontline.sample.BasicSimulation" >}}