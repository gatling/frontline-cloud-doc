---
title: "Maven"
description: "Learning how to setup FrontLine Cloud using Maven"
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

There are multiple ways to create a Gatling artifact, which will be used by FrontLine to launch a run.
We can use either a build tool (Maven, Sbt, Gradle) or the Gatling bundle.

In this tutorial, we'll use the Maven build tool, so make sure you have Maven configured.
We'll use Maven from the terminal, but you can also do it easily with an IDE like IntelliJ.

{{< include download_samples.md >}}

Extract the archive, then navigate to the maven folder.
This folder contains a basic Gatling simulation which can be used with FrontLine.

## Check the Maven build configuration

You can check in the `pom.xml` file that this project uses a special `frontline-maven-plugin` plugin to generate the artifact in the proper format.

If you have some existing projects you want to use, you'll have to configure this plugin there too.
If so, please have a look at the [Reference Documentation](/docs/user/artifact_gen/#maven-project) for more information.

## Generate the artifact

Open a terminal in the maven folder and execute the following command to create an artifact:

```shell
mvn clean package -DskipTests
```

{{< img src="maven_command.png" alt="Maven command" >}}

Maven will download the necessary dependencies and package our simulation.
That's it, we created our first Gatling artifact!

{{< include upload.md "frontline-samples/maven/target/maven-sample-1.0.0-shaded.jar" >}}

Upload it to FrontLine, either by drag-and-dropping it to the modal, or by clicking on the modal to open the file manager.

{{< img src="choose_artifact.png" alt="Choose the generated artifact" >}}

We now have to click on the Upload button to upload it to FrontLine.
After a few seconds, the upload will be complete, and our artifact will be successfully ready to use!

{{< img src="upload.png" alt="Start the upload" >}}

{{< include simulation_1.md "frontline.sample.BasicSimulation" >}}

{{< img src="create_simulation_step_1.png" alt="General configuration" >}}

{{< include simulation_2.md "frontline.sample.BasicSimulation" >}}
