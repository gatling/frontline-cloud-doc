---
title: "Getting started with Gatling Enterprise"
menutitle: "Getting started with Gatling Enterprise"
lead: "Learn how to run your first test from scratch"
slug: "getting-started"
aliases:
 - getting-started-with-gatling-bundle
 - gradle
 - maven
 - sbt
description: "Learn how to setup Gatling Enterprise for the first time"
date: 2021-08-30T16:00:00+02:00
lastmod: 2021-08-30T16:00:00+02:00
---

In this tutorial, we'll describe every step to help you run your first test with Gatling Enterprise.

{{< alert tip >}}
**Requirements**

* A valid [GitHub](https://github.com/) account
* A Java Development Kit (JDK) installed in version 8 or 11
* The [Gatling bundle](https://gatling.io/open-source/#downloadgatling) as a starter or an existing Maven/Gradle/SBT Gatling project
{{< /alert >}}

Gatling is a code based load testing tool. It is based on the Scala programming language and is an Open-Source project that is free to use! Gatling is used to power Gatling Enterprise in order to make it a full-fledged load testing tool with even more features, such as live reports, advanced metrics and large scale clustered tests. The tests you code with Gatling are compatible with Gatling Enterprise without any changes.

Gatling requires a Java installation so make sure that you have a JDK 8 or 11 installed on your computer. [Adoptium](https://adoptium.net) provides good packages that contain everything you need.

While there are a lot of ways to start working on your first test, **if you are new to this, we recommend using the Gatling bundle**. It is self sufficient and doesn't require knowledge in any of the usual Java/Scala build tools, i.e. Gradle, Maven or SBT.

If you already have experience with these tools, they are the long-term better option as they will allow integrating Gatling into an already existing tool chain and continuous integration tools such as Jenkins. For now, we'll go with the Gatling bundle for simplicity!

## Step 1 - Sign up and log in {#sign-up-and-log-in}

Before we start, make sure to go to [cloud.gatling.io](https://cloud.gatling.io) where you will need a [GitHub](https://github.com) account in order to sign up.

Gatling Enterprise handles users by putting them in organizations. This will allow you to work with multiple people on the same test projects, so they can either run new tests or view the results of previous ones.

A user can be part of either a team within an organization or have access to the organization as a whole. In all cases, a user must be part of an organization. Hence, once signed up and connected, you'll need to create one:

**If you are the first person in your organization to join Gatling, sign up and create an account**. In this case, you will need to create an organization:

{{< loom "a60f7e69b5044fc5bbfe5d5c2219ddb3" >}}

{{< alert warning >}}
Be careful with this step as your account can not be connected to multiple organizations. If you want to join an existing organization, please reach out to a system administrator of your team for you to be added.
{{< /alert >}}

**If you have been added to an organization by a System Administrator already.**

Press "Continue with GitHub", where you will be asked to authorize the application. The GitHub authorization page opens in a new tab. If no tab opens, make sure it is not being blocked by your browser.

{{< img src="github.png" alt="Continue with GitHub" caption="Continue with GitHub button at the bottom of the Login form" >}}

One last thing, new [organizations]({{< ref "../../reference/user/organization" >}}) are given **free credits** to **launch their first test**.

## Step 2 - Download the Gatling bundle {#download-gatling-bundle}

{{< alert tip >}}
If you already know about Gatling and/or you are using a Gradle/Maven/SBT project, you can directly skip to the [generate a package]({{< ref "#step-3-generate-package" >}}) section.
{{< /alert >}}

Click [here](https://gatling.io/open-source/#downloadgatling) to download the latest version of the Gatling bundle and extract the archive.

{{< loom "c5d9c2663b6c440f94c51eeb04291337" >}}

Taken from the [bundle structure documentation](https://gatling.io/docs/gatling/reference/current/general/bundle_structure/), we learn that the important parts of the bundle are as such:

* `bin` contains launch scripts for Gatling and more, this is where you execute Gatling and package the tests for Gatling Enterprise, which we'll see later on
* `user-files/simulations` contains your scala test files, this is where you describe what you want your virtual users to do, and what you want to do with them
* `results` contains log files of each tests and reports generated in sub directories if launched with Gatling

{{< alert info >}}
Inside Gatling, a *test* is called a *simulation*.
{{< /alert >}}

### Running the test locally

You can test your installation by running your test locally. If Java is properly installed, you can enter the following command in your terminal, or open the `gatling.sh` (if Linux/MacOS) or `gatling.bat` (if Windows) in any file explorer:

```console
$ ./bin/gatling.sh
```

If working properly, you should see something like this:

```console
GATLING_HOME is set to xxx/gatling-charts-highcharts-bundle-3.6.1
Choose a simulation number:
     [0] computerdatabase.BasicSimulation
     [1] computerdatabase.advanced.AdvancedSimulationStep01
     [2] computerdatabase.advanced.AdvancedSimulationStep02
     [3] computerdatabase.advanced.AdvancedSimulationStep03
     [4] computerdatabase.advanced.AdvancedSimulationStep04
     [5] computerdatabase.advanced.AdvancedSimulationStep05
```

So far we didn't create any test of our own but the Gatling bundle provides some example for you to start with. Here, the bundle is prompting you to choose one of the sample tests so it can run it. You can, for example, type `0`, after which the Gatling bundle will prompt you for an optional description of the test being run:

```console
Select run description (optional)
```

Finally, if everything is properly setup, you'll see that the test has started:

```console
Simulation computerdatabase.BasicSimulation started...
```

By default, Gatling works in console mode, outputting where the test is that every 5 seconds. After Gatling is done, you should see something like this:

```console
Simulation computerdatabase.BasicSimulation completed in 23 seconds
Parsing log file(s)...
Parsing log file(s) done
Generating reports...

================================================================================
---- Global Information --------------------------------------------------------
> request count                                         13 (OK=13     KO=0     )
> min response time                                     84 (OK=84     KO=-     )
> max response time                                    168 (OK=168    KO=-     )
> mean response time                                    93 (OK=93     KO=-     )
> std deviation                                         22 (OK=22     KO=-     )
> response time 50th percentile                         87 (OK=87     KO=-     )
> response time 75th percentile                         87 (OK=87     KO=-     )
> response time 95th percentile                        121 (OK=121    KO=-     )
> response time 99th percentile                        159 (OK=159    KO=-     )
> mean requests/sec                                  0.542 (OK=0.542  KO=-     )
---- Response Time Distribution ------------------------------------------------
> t < 800 ms                                            13 (100%)
> 800 ms < t < 1200 ms                                   0 (  0%)
> t > 1200 ms                                            0 (  0%)
> failed                                                 0 (  0%)
================================================================================

Reports generated in 0s.
Please open the following file: .../gatling-charts-highcharts-bundle-3.6.1/results/basicsimulation-20210826135452662/index.html
```

You can open the url at the end of the output, which will show a report of the test that was just run:

{{< img src="gatling.png" alt="Gatling Open-Source reports" caption="Samples from a Gatling Open-Source report" >}}

{{< alert tip >}}
Running a test locally is also a great way to do debugging before sending it to Gatling Enterprise, as running anything inside Gatling Enterprise consumes credits while running a test with Gatling is free.
{{< /alert >}}

## Step 3 - Generate a package {#generate-package}

Gatling Enterprise requires a package built from a Gatling project in order to run a test. This package is called an `artifact`.

To create an artifact, run the `artifact.bat` (if Windows) or `artifact.sh` (if Linux/MacOS) file, Gatling will start creating your first artifact:

```console
$ ./bin/artifact.sh
GATLING_HOME is set to .../gatling-charts-highcharts-bundle-3.6.1
GATLING_VERSION is set to '3.6.1'
Creating jar...done
```

This script can also be clicked or double clicked from any file explorer. You'll see the result in the folder `target`, which should now have a file named `artifact.jar`.

{{< loom "ca115b7607224376a2f6cefdb0cfc479" >}}

When using other tools, the command to type and the name of the outputted file will differ:

{{< code-toggle console >}}
bundle: ./bin/artifact.sh
gradle: gradle frontLineJar
maven: mvn clean package -DskipTests
sbt: sbt test:assembly
{{</ code-toggle >}}

The name of the file being:

{{< code-toggle console >}}
bundle: target/artifact.jar
gradle: build/libs/gradle.jar
maven: target/${name_of_the_project}-1.0.0-shaded.jar
sbt: target/${name_of_the_project}-1.0.0.jar
{{</ code-toggle >}}

## Step 4 - Upload the package {#upload-package}

On Gatling Enterprise, click on the artifacts icon and on create. Give a name to your package, you can choose to make it available to all teams or to limit access to a specific one, then hit save.

Next, upload the package you generated earlier by clicking on the cloud icon. The artifact you generated earlier is located here: `target/artifact.jar`

Upload it to Gatling Enterprise, either by drag-and-dropping it to the modal, or by clicking on the modal to open the file manager.

{{< loom "7511a5a75a8f4f61baf4987bdb6ec306" >}}

## Step 5 - Create a simulation {#create-simulation}

To get a grasp of how to run a simulation with Gatling, you will find a ready to use script in the bundle. So let's get started with your first simulation!

In the Simulation section, click on "Add" if there are no other simulations or on "Create" if other simulations have already been created before.

Choose a name for your simulation and define the team that will have access to this simulation (if none has been created yet, just click on "Default").

Under Class name, it is important you enter the name of the script that was uploaded in the package, otherwise the run of your simulation will fail in the next step. In this case, the ready-made script called `BasicSimulation.scala` is located in the bundle: user-files > simulations > computer database. Therefore, you need to enter `computerdatabase.BasicSimulation` as the class name. Once you've entered the script name click on the "Next" button.

Next, you'll need to select your package name. It is important to select the package that contains the simulation you'd like to run.  When you've selected your package, click on the "Next" button.

In the final step, you can choose where you want the injection to take place. Select a location, eg: "US East - N. Virginia (AWS)" and click on the "Save" button.  The traffic in your simulation will be coming from the region you've selected. The three next steps are optional and you can learn more about them [here]({{< ref "../../reference/user/simulations/" >}}), but for now, let's just simply click "save".

{{< loom "8f52e27d235c4a9c9b9c820055f7b3ac" >}}

{{< alert tip >}}
If you would like to write your own script as a first test, you can modify the BasicSimulation file in the space as shown in the video. If you would like to learn more about creating different types of tests and writing your own scripts check out the [Gatling Academy](https://gatling.io/academy/). Please bear in mind you get 5 credits for free but if your test needs more, it will stop before it should finish.
{{< /alert >}}

{{< loom "055f4f01d22e4d3fa65905819a719417" >}}

That's it, you have created your first simulation on Gatling Enterprise!

## Step 6 - Start load testing! {#start-load-testing}

You are now ready to start load testing. To start the simulation you have just created, click on the Start icon. As soon as the simulation is running ("injecting" but not during the building and deploying statuses), you will be able to access the report, even while it is building.

{{< loom "e470b748187f4371995005282541ddd5" >}}

Now that you know how to create and run a simulation on Gatling, we suggest you jump on to the tutorials Interface tour and get the most out of the [reports]({{< ref "../../reference/user/reports" >}}).

## Going further

### Learn about load testing concepts

Load testing is hard but the test you write doesn't need to be. In [this article](https://www.infoq.com/articles/load-testing-apis-gatling/) you'll learn what matters the most when creating your first load testing campaign.

### Make your own tests

If you've read the article cited in the previous section, you've learned you don't need much to make a load testing that can provide meaningful results. At some point though, you might need to add complexity into the test, such as real user behavior, specific injection profiles, etc.. This is where learning how to craft tests will come handy. Good resources are:

- The [Quickstart tutorial](https://gatling.io/docs/gatling/tutorials/quickstart/) to learn about the Recorder and how to quickly bootstrap a complex scenario and go from there
- The [Advanced tutorial](https://gatling.io/docs/gatling/tutorials/advanced/) for more advanced crafting
- For a exhaustive of all you can do with Gatling, the [Cheat-Sheet](https://gatling.io/docs/gatling/reference/current/cheat-sheet/) is your best friend
- The [Gatling Academy](https://gatling.io/academy/), where you can follow our own training program!

## Additional resources

You have successfully started load testing, you can read our documentation if you need more information about our product:

- [Gatling documentation](https://gatling.io/docs/gatling/)
- [Gatling Enterprise documentation]({{< ref "../../" >}})
- [Documentation about the creation of an artifact]({{< ref "../../reference/user/artifact_gen" >}})
- [Documentation about how to use the reports]({{< ref "../../reference/user/reports" >}})
