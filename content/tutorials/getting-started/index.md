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
* A Java Development Kit (JDK) installed in version 8, 11 or 17. [Azul System's Zulu](https://www.azul.com/downloads/?package=jdk) provides good packages that contain everything you need.
{{< /alert >}}

{{< alert tip >}}
**Recommendations**

* The [Gatling bundle](https://gatling.io/open-source/#downloadgatling) is beginner-friendly and is the best choice if you're just getting started.
* For long term projects, consider using a **Maven**, **Gradle** or **SBT** Gatling project. Please make sure to check the sample projects you can reach from the "Samples" button.
{{< /alert >}}

Gatling is a code based load testing tool. It is based on the Scala programming language and is an Open-Source project that is free to use! Gatling is used to power Gatling Enterprise in order to make it a full-fledged load testing tool with even more features, such as live reports, advanced metrics and large scale clustered tests. The tests you code with Gatling are compatible with Gatling Enterprise without any changes.

While there are a lot of ways to create your first test, **if you are new to load testing or not a developer, we recommend using the Gatling bundle as it is beginner-friendly**. It is self-sufficient and doesn't require knowledge of the usual Java/Kotlin/Scala build tools, i.e. Gradle, Maven or SBT.

If you already have experience with these tools, they are the better options long-term as they will allow integrating Gatling into an already existing tool chain:

* Code editors, such as IntelliJ and Visual Studio Code
* Continuous integration tools, such as Jenkins

## Step 1 - Sign up and log in {#sign-up-and-log-in}

Before we start, make sure to go to [cloud.gatling.io](https://cloud.gatling.io) where you will need to create an account either by registering with your email address or a [GitHub](https://github.com) / [Gmail](https://www.google.com/gmail) account in order to sign up. Once you've created your account you will need to verify your email address.  You should receive a verification email from us, be sure to check your spam folder if you don't see it.

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
* `user-files/simulations` contains your java test files, this is where you describe what you want your virtual users to do, and what you want to do with them
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
Do you want to run the simulation locally, on Gatling Enterprise, or just package it?
Type the number corresponding to your choice and press enter
[0] <Quit>
[1] Run the Simulation locally
[2] Package and upload the Simulation to Gatling Enterprise Cloud, and run it there
[3] Package the Simulation for Gatling Enterprise
[4] Show help and exit
```
Here we want to run the simulation locally, so type `1`

So far we didn't create any test of our own but the Gatling bundle provides an example for you to start with.
In case you have multiple simulations, you will have to select one, here we have only one for now so this choice is obviously skipped. 

Then the Gatling bundle will prompt you for an optional description of the test being run:

```
Select run description (optional)
```

Finally, if everything is properly setup, you'll see that the test has started:

```
Simulation computerdatabase.BasicSimulation started...
```

By default, Gatling works in console mode, outputting where the test is that every 5 seconds. After Gatling is done, you should see something like this:

```
Simulation computerdatabase.ComputerDatabaseSimulation completed in 17 seconds
Parsing log file(s)...
Parsing log file(s) done
Generating reports...

================================================================================
---- Global Information --------------------------------------------------------
> request count                                        105 (OK=104    KO=1     )
> min response time                                     96 (OK=96     KO=101   )
> max response time                                    657 (OK=657    KO=101   )
> mean response time                                   145 (OK=146    KO=101   )
> std deviation                                        108 (OK=109    KO=0     )
> response time 50th percentile                        106 (OK=106    KO=101   )
> response time 75th percentile                        112 (OK=112    KO=101   )
> response time 95th percentile                        420 (OK=420    KO=101   )
> response time 99th percentile                        443 (OK=443    KO=101   )
> mean requests/sec                                  5.833 (OK=5.778  KO=0.056 )
---- Response Time Distribution ------------------------------------------------
> t < 800 ms                                           104 ( 99%)
> 800 ms <= t < 1200 ms                                  0 (  0%)
> t ≥ 1200 ms                                            0 (  0%)
> failed                                                 1 (  1%)
---- Errors --------------------------------------------------------------------
> status.find.is(201), but actually found 200                         1 (100,0%)
================================================================================

Reports generated in 0s.
Please open the following file: .../gatling-charts-highcharts-bundle-{{< var gatlingVersion >}}/results/computerdatabasesimulation-20221020093736074/index.html

```
{{< alert tip >}}
Some errors are generated randomly to demonstrate the retry mechanism, nothing to worry about here.
{{< /alert >}}

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

#### Step 3.1 - Create an API token

On Gatling Enterprise, click on the [API Tokens section]({{< ref "../../reference/admin/api_tokens/" >}}) and on create. Give a name to your API token, and give him `Configure` as Organization role, then hit save.

Next, copy the content of the API token, you won't be able to access it once you close the modal.

#### Step 3.2 - Create and run your first simulation on the cloud

The bundle, maven, gradle or sbt can create interactively a simulation, upload its package and start it.

Type the command corresponding to your environment, don't forget to replace <CREATED_API_TOKEN> by the value of your API token:

{{< alert tip >}}
Windows users must replace `gatling.sh` by `gatling.bat`
{{< /alert >}}

{{< code-toggle console >}}
bundle: ./bin/gatling.sh --api-token <CREATED_API_TOKEN>
gradle: gradle gatlingEnterpriseStart -Dgatling.enterprise.apiToken=<CREATED_API_TOKEN>
maven: mvn gatling:enterpriseStart -Dgatling.enterprise.apiToken=<CREATED_API_TOKEN>
sbt: sbt "Gatling / enterpriseStart" -Dgatling.enterprise.apiToken=<CREATED_API_TOKEN>
{{</ code-toggle >}}

```
Do you want to create a new simulation or start an existing one?
Type the number corresponding to your choice and press enter
[0] <Quit>
[1] Create a new Simulation on Gatling Enterprise, then start it
[2] Start an existing Simulation on Gatling Enterprise
```

Let's create a new simulation, type `1`, then press **Enter**.

```
Proceeding to the create simulation step
Choose one simulation class in your package:
Type the number corresponding to your choice and press enter
[0] <Quit>
[1] computerdatabase.ComputerDatabaseSimulation
```

Select your Simulation class (the only available here) with `1`, then press **Enter**.

```
Choose a team from the list:
Type the number corresponding to your choice and press enter
[0] <Quit>
[1] Team 'Default team', id='b236af88-1c24-4623-b872-cb8993651c15'
```

Select the default team with `1`, then press **Enter**.   
This will allow access to the simulation to users within this team.

```
Enter a simulation name, or just hit enter to accept the default name (ComputerDatabaseSimulation)
Waiting for user input...
```

Enter the name of your simulation, for instance `GettingStartedSimulation`, then press **Enter**.

```
Do you want to create a new package or upload your project to an existing one?
Type the number corresponding to your choice and press enter
[0] <Quit>
[1] Create a new package on Gatling Enterprise
[2] Choose an existing package on Gatling Enterprise
```
The package is an archive containing your simulation classes.
Choose `1` to create a package, then press **Enter**.  
It will be reusable by other simulations later on. 

```
Enter a package name, or just hit enter to accept the default name (gatling:bundle)
Waiting for user input...
```

Enter the name of your package : `GettingStartedPackage`, then press **Enter**.

```
Choose the load generators location
Type the number corresponding to your choice and press enter
[0] <Quit>
[1] Location AP - Hong kong
[2] Location AP - Tokyo
[3] Location AP Pacific - Mumbai
[4] Location AP SouthEast - Sydney
[5] Location Europe - Dublin
[6] Location Europe - Paris
[7] Location SA East - São Paulo
[8] Location US East - N. Virginia
[9] Location US West - N. California
[10] Location US West - Oregon
```

Here we configure the Simulation itself.
Choose the location where you want to spawn your load generators and generate traffic from.  
For demonstration purposes we will choose `6`, then press **Enter**.

```
Enter the number of load generators
```

Define how many instances of load generators will be spawned in this region. Care, each load generator instance will consume credits while it runs.   
Let's select only `1` load generator, then press **Enter**.

```
Start simulation using simulation class name: computerdatabase.ComputerDatabaseSimulation
Created simulation named gettingStartedSimulation with ID '<simulation-id>'

Specify --simulation-id <simulation-id> if you want to start a simulation on Gatling Enterprise,
or --simulation computerdatabase.ComputerDatabaseSimulation if you want to create a new simulation on Gatling Enterprise.
See https://gatling.io/docs/gatling/reference/current/core/configuration/#cli-options/ for more information.

Simulation successfully started; once running, reports will be available at https://cloud.gatling.io/o/<organization-id>/simulations/reports/<report-id>
```

Click on the provided link to see the live reporting of your simulation run. 

You will be able to start again the same simulation if you specify its ID:

{{< code-toggle console >}}
bundle: ./bin/gatling.sh --api-token <CREATED_API_TOKEN> --simulation-id <CREATED_SIMULATION_ID>
gradle: gradle gatlingEnterpriseStart -Dgatling.enterprise.apiToken=<CREATED_API_TOKEN> -Dgatling.enterprise.simulationId=<CREATED_SIMULATION_ID>
maven: mvn gatling:enterpriseStart -Dgatling.enterprise.apiToken=<CREATED_API_TOKEN> -Dgatling.enterprise.simulationId=<CREATED_SIMULATION_ID>
sbt: sbt "Gatling / enterpriseStart" -Dgatling.enterprise.apiToken=<CREATED_API_TOKEN> -Dgatling.enterprise.simulationId=<CREATED_SIMULATION_ID>
{{</ code-toggle >}}

{{< alert tip >}}
Windows users must replace `gatling.sh` by `gatling.bat`
{{< /alert >}}

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
- The [Gatling Academy](https://gatling.io/academy/), where you can follow our own training program!
- For an exhaustive list of all you can do with Gatling, the [Reference documentation](https://gatling.io/docs/gatling/reference/current/) is your best friend

## Additional resources

You have successfully started load testing, you can read our documentation if you need more information about our product:

- [Gatling documentation](https://gatling.io/docs/gatling/)
- [Gatling Enterprise documentation]({{< ref "../../" >}})
- [Documentation about the creation of a package]({{< ref "../../reference/user/package_gen" >}})
- [Documentation about how to use the reports]({{< ref "../../reference/user/reports" >}})
