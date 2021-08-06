---
title: "Getting started with Gradle"
menutitle: "Gradle"
description: "Learn how to setup Gatling Enterprise using Gradle"
date: 2021-03-19T17:03:31+01:00
lastmod: 2021-03-19T17:03:31+01:00
weight: 50
---

{{< include introduction.md >}}

## Download the sample Gatling simulation

There are multiple ways to create a Gatling artifact, which will be used by Gatling Enterprise to launch a run.
You can use either a build tool (Maven, SBT, Gradle) or the Gatling bundle.

In this tutorial, we'll use the **Gradle** build tool, so make sure you have Gradle configured.
We will use Gradle from the terminal, but you can also easily run Gradle commands from an IDE such as IntelliJ IDEA.

{{< include download_samples.md >}}

Extract the archive, then navigate to the **gradle** folder.
This folder contains a basic Gatling simulation configured to be used with Gatling Enterprise.

## Check the Gradle build configuration

You can check in the `build.gradle` files that this project uses a special `io.gatling.frontline.gradle` plugin to generate the artifact in the proper format.

If you have some existing projects you want to use, you'll have to configure this plugin there too.
If so, please have a look at the [Reference Documentation]({{< ref "../../reference/user/artifact_gen#gradle-project" >}}) for more information.

## Generate the artifact

Open a terminal in the gradle folder and execute the following command to create an artifact:

```console
gradle frontLineJar
```

{{< img src="gradle_command.png" alt="Maven command" >}}

Gradle will download the necessary dependencies and package your simulation.
That's it, you've created your first Gatling artifact!

## Upload the artifact 

{{< include upload_manual.md "frontline-samples/gradle/build/libs/gradle.jar" >}}

{{< img src="choose_artifact.png" alt="Choose the generated artifact" >}}

Click on the Upload button. After a few seconds, the upload will complete, and your artifact will be ready to use!

{{< img src="upload.png" alt="Start the upload" >}}

## Create a simulation

{{< include simulation_1.md "frontline.sample.BasicSimulation" >}}

{{< img src="create_simulation_step_1.png" alt="General configuration" >}}

{{< include simulation_2.md "frontline.sample.BasicSimulation" >}}

## Start load testing!

{{< include run.md >}}

## Additional resources

{{< include resources.md >}}
