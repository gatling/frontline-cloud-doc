---
title: "Getting started with SBT"
menutitle: "SBT"
description: "Learn how to setup Gatling Enterprise using SBT"
date: 2021-03-19T17:03:31+01:00
lastmod: 2021-03-19T17:03:31+01:00
weight: 50
---

{{< include introduction.md >}}

## Download the sample Gatling simulation

There are multiple ways to create a Gatling artifact, which will be used by Gatling Enterprise to launch a run.
You can use either a build tool (Maven, SBT, Gradle) or the Gatling bundle.

In this tutorial, we'll use the **SBT** build tool, so make sure you have SBT configured.
We will use SBT from the terminal, but you can also easily run SBT commands from an IDE such as IntelliJ IDEA.

{{< include download_samples.md >}}

Extract the archive, then navigate to the **SBT** folder.
This folder contains a basic Gatling simulation which can be used with Gatling Enterprise.

## Check the SBT build configuration

You can check in the `build.sbt` and `project/plugins.sbt` files that this project uses a special `sbt-frontline` plugin to generate the artifact in the proper format.

If you have some existing projects you want to use, you'll have to configure this plugin there too.
If so, please have a look at the [Reference Documentation]({{< ref "../../reference/user/artifact_gen#maven-project" >}}) for more information.

## Generate the artifact

Open a terminal in the SBT folder and execute the following command to create an artifact:

```console
sbt test:assembly
```

{{< img src="sbt_command.png" alt="SBT command" >}}

SBT will download the necessary dependencies and package your simulation.
That's it, you've created your first Gatling artifact!

## Upload the artifact 

### Option 1: Manual Upload

{{< include upload_manual.md "frontline-samples/sbt/target/sbt-frontline-1.0.0.jar" >}}

Upload it to Gatling Enterprise, either by drag-and-dropping it to the modal, or by clicking on the modal to open the file manager.

{{< img src="choose_artifact.png" alt="Choose the generated artifact" >}}

Click on the Upload button. After a few seconds, the upload will complete, and your artifact will be ready to use!

{{< img src="upload.png" alt="Start the upload" >}}

### Option 2: API Upload

{{< include upload_api.md "frontline-samples/sbt/target/sbt-frontline-1.0.0.jar" >}}

## Create a simulation

{{< include simulation_1.md "frontline.sample.BasicSimulation" >}}

{{< img src="create_simulation_step_1.png" alt="General configuration" >}}

{{< include simulation_2.md "frontline.sample.BasicSimulation" >}}

## Start load testing!

{{< include run.md >}}

## Additional resources

{{< include resources.md >}}
