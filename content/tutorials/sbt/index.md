---
title: "Getting started with sbt"
description: "Learning how to setup FrontLine using sbt"
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

There are multiple ways to create a Gatling artifact, which will be used by FrontLine to launch a run. We can use either a build tool (Maven, sbt, Gradle) or the Gatling bundle.

In this tutorial, we'll use the sbt build tool, so make sure you have sbt configured and Java installed. We'll use sbt from the terminal, but you can also do it easily with an IDE like IntelliJ.

Extract the archive, then navigate to the sbt folder and open a terminal there. This folder contains a basic Gatling simulation which can be used with FrontLine.

Execute the following command to create an artifact:

```shell
sbt test:assembly
```

{{< img src="sbt_command.png" alt="sbt command" >}}

sbt will download the necessary dependencies and package our simulation. That's it, we created our first Gatling artifact!

{{< include upload.md "frontline-samples/sbt/target/sbt-frontline-1.0.0.jar" >}}

Upload it to FrontLine, either by drag-and-dropping it to the modal, or by clicking on the modal to open the file manager.

{{< img src="choose_artifact.png" alt="Choose the generated artifact" >}}

We now have to click on the Upload button to upload it to FrontLine. After a few seconds, the upload will be complete, and our artifact will be successfully ready to use!

{{< img src="upload.png" alt="Start the upload" >}}

{{< include simulation_1.md "frontline.sample.BasicSimulation" >}}

{{< img src="create_simulation_step_1.png" alt="General configuration" >}}

{{< include simulation_2.md "frontline.sample.BasicSimulation" >}}