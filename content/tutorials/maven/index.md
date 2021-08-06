---
title: "Getting started with Maven"
menutitle: "Maven"
description: "Learn how to setup Gatling Enterprise using Maven"
date: 2021-03-19T17:03:31+01:00
lastmod: 2021-03-19T17:03:31+01:00
weight: 50
---

{{< include introduction.md >}}

## Download the sample Gatling simulation

There are multiple ways to create a Gatling artifact, which will be used by Gatling Enterprise to launch a run.
You can use either a build tool (Maven, SBT, Gradle) or the Gatling bundle.

In this tutorial, we'll use the **Maven** build tool, so make sure you have Maven configured.
We will use Maven from the terminal, but you can also easily run Maven commands from an IDE such as IntelliJ IDEA.

{{< include download_samples.md >}}

Extract the archive, then navigate to the **maven** folder.
This folder contains a basic Gatling simulation configured to be used with Gatling Enterprise.

## Check the Maven build configuration

You can check in the `pom.xml` file that this project uses a special `frontline-maven-plugin` plugin to generate the artifact in the proper format.

If you have some existing projects you want to use, you'll have to configure this plugin there too.
If so, please have a look at the [Reference Documentation]({{< ref "../../reference/user/artifact_gen#maven-project" >}}) for more information.

## Generate the artifact

Open a terminal in the maven folder and execute the following command to create an artifact:

```console
mvn clean package -DskipTests
```

{{< img src="maven-command.png" alt="Maven command" >}}

Maven will download the necessary dependencies and package your simulation.
That's it, you've created your first Gatling artifact!

## Upload the artifact

{{< include upload_manual.md "frontline-samples/maven/target/maven-sample-1.0.0-shaded.jar" >}}

Upload it to Gatling Enterprise, either by drag-and-dropping it to the modal, or by clicking on the modal to open the file manager.

{{< img src="choose-artifact.png" alt="Choose the generated artifact" >}}

Click on the Upload button. After a few seconds, the upload will complete, and your artifact will be ready to use!

{{< img src="upload.png" alt="Start the upload" >}}

## Create a simulation

{{< include simulation_1.md "frontline.sample.BasicSimulation" >}}

{{< img src="create-simulation-step-1.png" alt="General configuration" >}}

{{< include simulation_2.md "frontline.sample.BasicSimulation" >}}

## Start load testing!

{{< include run.md >}}

## Additional resources

{{< include resources.md >}}
