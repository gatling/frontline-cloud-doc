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
weight: 10010
---

In this tutorial, we'll describe every step to help you run your first test with Gatling Enterprise.

{{< alert info >}}
**Requirements**
* A Java Development Kit (JDK) installed in version 8, 11 or 17
{{< /alert >}}

{{< alert tip >}}
**Recommendations**

* The [Gatling bundle](https://gatling.io/open-source/#downloadgatling) is beginner-friendly and is the best choice if you're just getting started.
* For long term projects, consider using a **Maven**, **Gradle** or **SBT** Gatling project. Please make sure to check the sample projects you can reach from the "Samples" button.
{{< /alert >}}

Gatling is a code based load testing tool. It is based on the Scala programming language and is an Open-Source project that is free to use! Gatling is used to power Gatling Enterprise in order to make it a full-fledged load testing tool with even more features, such as live reports, advanced metrics and large scale clustered tests. The tests you code with Gatling are compatible with Gatling Enterprise without any changes.

Gatling requires a Java installation so make sure that you have a JDK 8, 11 or 17 installed on your computer. [Azul System's Zulu](https://www.azul.com/downloads/?package=jdk) provides good packages that contain everything you need.

While there are a lot of ways to create your first test, **if you are new to load testing or not a developer, we recommend using the Gatling bundle as it is beginner-friendly**. It is self-sufficient and doesn't require knowledge of the usual Java/Kotlin/Scala build tools, i.e. Gradle, Maven or SBT.

If you already have experience with these tools, they are the better options long-term as they will allow integrating Gatling into an already existing tool chain:

* Code editors, such as IntelliJ and Visual Studio Code
* Continuous integration tools, such as Jenkins

## Step 1 - Sign up and log in {#sign-up-and-log-in}

Before we start, make sure to go to [cloud.gatling.io](https://cloud.gatling.io) where you will need to create an account either by registering with your email address or a [GitHub](https://github.com) account in order to sign up. Once you've created your account you will need to verify your email address.  You should receive a verification email from us, be sure to check your spam folder if you don't see it.

Gatling Enterprise handles users by putting them in organizations. This will allow you to work with multiple people on the same test projects, so they can either run new tests or view the results of previous ones.

A user can either be part of a team within an organization, or have access to that organization as a whole. 
In all cases, a user must be part of an organization. Hence, once signed up and connected, you'll need to create one:

{{< img src="login.png" alt="Login / Register" caption="Login with your account" >}}

**If you are the first person in your organization to join Gatling, login and create an organization:**
{{< img src="organization-creation.png" alt="Organization creation" caption="Create an organization" >}}

**If you have been invited to an organization by a System Administrator already.**

{{< img src="invitation.png" alt="Email invitation" caption="Click on accept invitation" >}}
{{< img src="invitation-prompt.png" alt="Invitation cancellation" caption="Click on continue to access the organization" >}}


One last thing, new [organizations]({{< ref "../../reference/user/organization" >}}) are given **free credits** to **launch their first test**.

## Step 2 - Prepare the Project {#prepare-project}

#### Using the Bundle {#prepare-project-bundle}

Click [here](https://gatling.io/open-source/#downloadgatling) to download the latest version of the Gatling bundle and extract the archive.

[comment]: <> (FIXME: Video need to be updated)
[comment]: <> ({{< youtube qdhLMwHlvhU >}})

Taken from the [bundle structure documentation](https://gatling.io/docs/gatling/reference/current/general/bundle_structure/), we learn that the important parts of the bundle are as such:

* `bin` contains launch scripts for Gatling and more, this is where you execute Gatling and package the tests for Gatling Enterprise, which we'll see later on
* `user-files/simulations` contains your scala test files, this is where you describe what you want your virtual users to do, and what you want to do with them
* `results` contains log files of each tests and reports generated in sub directories if launched with Gatling

{{< alert info >}}
Inside Gatling, a *test* is called a *simulation*.
{{< /alert >}}

You can test your installation by running your test locally. If Java is properly installed, you can enter the following command in your terminal, or open the `gatling.sh` (if Linux/MacOS) or `gatling.bat` (if Windows) in any file explorer:

```
$ ./bin/gatling.sh
```

If working properly, you should see something like this:

```
GATLING_HOME is set to xxx/gatling-charts-highcharts-bundle-{{< var gatlingVersion >}}
Choose a simulation number:
     [0] computerdatabase.BasicSimulation
     [1] computerdatabase.advanced.AdvancedSimulationStep01
     [2] computerdatabase.advanced.AdvancedSimulationStep02
     [3] computerdatabase.advanced.AdvancedSimulationStep03
     [4] computerdatabase.advanced.AdvancedSimulationStep04
     [5] computerdatabase.advanced.AdvancedSimulationStep05
```

So far we didn't create any test of our own but the Gatling bundle provides some example for you to start with. Here, the bundle is prompting you to choose one of the sample tests so it can run it. You can, for example, type `0`, after which the Gatling bundle will prompt you for an optional description of the test being run:

```
Select run description (optional)
```

Finally, if everything is properly setup, you'll see that the test has started:

```
Simulation computerdatabase.BasicSimulation started...
```

By default, Gatling works in console mode, outputting where the test is that every 5 seconds. After Gatling is done, you should see something like this:

```
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
Please open the following file: .../gatling-charts-highcharts-bundle-{{< var gatlingVersion >}}/results/basicsimulation-20210826135452662/index.html
```

You can open the url at the end of the output, which will show a report of the test that was just run:

{{< img src="gatling.png" alt="Gatling Open-Source reports" caption="Samples from a Gatling Open-Source report" >}}

{{< alert tip >}}
Running a test locally is also a great way to do debugging before sending it to Gatling Enterprise, as running anything inside Gatling Enterprise consumes credits while running a test with Gatling is free.
{{< /alert >}}

#### Using maven, gradle or sbt {#prepare-project-buildtools}

{{< alert tip >}}
If you already know about Gatling and/or you are using a Gradle, Maven or SBT project, you'll need to update the configuration to make it compatible with Gatling Enterprise first:

* [Configuring a Gradle project]({{< ref "../../reference/user/package_gen/#gradle-project" >}})
* [Configuring a Maven project]({{< ref "../../reference/user/package_gen/#maven-project" >}})
* [Configuring a SBT project]({{< ref "../../reference/user/package_gen/#sbt-project" >}})

Or, you can download preconfigured sample projects from Gatling Enterprise by clicking on the "Sample simulations" button on the top right in the Simulations page.

Now, you can directly skip to the [generate a package]({{< ref "#generate-package" >}}) section.
{{< /alert >}}

## Step 3 - Create a simulation

### Using the bundle {#create-simulation-bundle}

#### Step 3.1 - Generate a Package {#generate-package}

Gatling Enterprise requires a package built from a Gatling project in order to run a test.

To create a package, run the `artifact.bat` (if Windows) or `artifact.sh` (if Linux/MacOS) file, Gatling will start creating your first package:

```
$ ./bin/artifact.sh
GATLING_HOME is set to .../gatling-charts-highcharts-bundle-{{< var gatlingVersion >}}
GATLING_VERSION is set to '{{< var gatlingVersion >}}'
Creating jar...done
```

This script can also be clicked or double clicked from any file explorer. You'll see the result in the folder `target`, which should now have a file named `artifact.jar`.

{{< youtube _fq3giAIibw >}}

#### Step 3.2 - Upload the Package {#upload-package}

On Gatling Enterprise, click on the [Packages section]({{< ref "../../reference/user/package_conf/" >}}) and on create. Give a name to your package, you can choose to make it available to all teams or to limit access to a specific one, then hit save.

Next, upload the package you generated earlier by clicking on the cloud icon. The package you generated earlier is located here: `target/artifact.jar`

Upload it to Gatling Enterprise, either by drag-and-dropping it to the modal, or by clicking on the modal to open the file manager.

#### Step 3.3 - Create a Simulation {#create-simulation}

To get a grasp of how to run a simulation with Gatling, you will find a ready to use script in the bundle. So let's get started with your first simulation!

In the [Simulations section]({{< ref "../../reference/user/simulations/" >}}), click on **Create**.

Choose a name for your simulation and define the team that will have access to this simulation (if none has been created yet, a default one named **Default team** exist).

Under Class name, it is important you enter the name of the script that was uploaded in the package, otherwise the run of your simulation will fail in the next step.

In this case, the ready-made script called `BasicSimulation.scala` is located in the bundle: `user-files > simulations > computerdatabase`

Therefore, you need to enter `computerdatabase.BasicSimulation` as the class name.

Then, click on the **Next** button.

Next, you'll need to select your package name. It is important to select the package that contains the simulation you'd like to run. Then, click on the **Next** button.

In the final step, you can choose where you want the injection to take place, and click on the **Save** button.

Your simulation will send the traffic from the region you've selected.

The three next steps are optional and you can learn more about them [in the dedicated section]({{< ref "../../reference/user/simulations/" >}}), but for now, let's just simply click **Save**.

{{< alert tip >}}
If you would like to write your own script as a first test, you can modify the BasicSimulation file in the space as shown in the video. If you would like to learn more about creating different types of tests and writing your own scripts check out the [Gatling Academy](https://gatling.io/academy/). Please bear in mind you get 5 credits for free but if your test needs more, it will stop before it should finish.
{{< /alert >}}

That's it, you have created your first simulation on Gatling Enterprise!

### Using maven, gradle or sbt {#create-simulation-buildtools}

#### Step 3.1 - Create an API token

On Gatling Enterprise, click on the [API Tokens section]({{< ref "../../reference/admin/api_tokens/" >}}) and on create. Give a name to your API token, and give him `Configure` as Organization role, then hit save.

Next, copy the content of the API token, you won't be able to access it once you close the modal.

#### Step 3.2 - Create and start a simulation

Maven, gradle or sbt can create interactively a simulation, upload its package and start it.

Type the command corresponding to your build plugin, don't forget to replace the API token:

{{< code-toggle console >}}
bundle: Not available yet
gradle: gradle gatlingEnterpriseStart -Dgatling.enterprise.apiToken=<CREATED_API_TOKEN>
maven: mvn gatling:enterpriseStart -Dgatling.enterprise.apiToken=<CREATED_API_TOKEN>
sbt: sbt "Gatling / enterpriseStart" -Dgatling.enterprise.apiToken=<CREATED_API_TOKEN>
{{</ code-toggle >}}


The command will ask you first about the classname of your simulation. Type the number corresponding to your choice and press enter.

In the same fashion, choose the team, simulation name, package name and the load injectors region. Choose a number of load injectors appropriate to your use case, as it'll increase your billing.

The command will create and start the simulation, then give you the ID of the created simulation. You will be able to start again the same simulation if you specify this ID:

{{< code-toggle console >}}
bundle: Not available yet
gradle: gradle gatlingEnterpriseStart -Dgatling.enterprise.apiToken=<CREATED_API_TOKEN> -Dgatling.enterprise.simulationId=<CREATED_SIMULATION_ID>
maven: mvn gatling:enterpriseStart -Dgatling.enterprise.apiToken=<CREATED_API_TOKEN> -Dgatling.enterprise.simulationId=<CREATED_SIMULATION_ID>
sbt: sbt "Gatling / enterpriseStart" -Dgatling.enterprise.apiToken=<CREATED_API_TOKEN> -Dgatling.enterprise.simulationId=<CREATED_SIMULATION_ID>
{{</ code-toggle >}}


## Step 4 - Start Load Testing! {#start-load-testing}

You are now ready to start load testing.

To start the newly created simulation, click on the **Start** icon (it should already be started if you're using maven, sbt or gradle).

The simulation status will change to **building**, then to **deploying** then to **injecting**.
You can now access the report (even during injection phase).

Now that you know how to create and run a simulation on Gatling, we suggest you jump on to the [overview]({{< ref "../../reference/user/overview" >}}) and get the most out of the [reports]({{< ref "../../reference/user/reports" >}}).

## Going Further

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
- [Documentation about the creation of a package]({{< ref "../../reference/user/package_gen" >}})
- [Documentation about how to use the reports]({{< ref "../../reference/user/reports" >}})
