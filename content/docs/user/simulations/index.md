---
title: "Simulations"
description: "Learn how to configure and navigate through simulations."
lead: "Navigate through simulations."
date: 2021-03-10T09:29:43-05:00
lastmod: 2021-03-10T09:29:43-05:00
draft: false
images: []
menu:
  docs:
    parent: "user"
weight: 060
---

## Managing Simulations

To access the Simulations section, click on **Simulations** in the navbar.

The Simulations view contains all the simulations you have configured and the result of their last run.

{{< img src="simulations-table.png" alt="Simulation table" >}}

If you don't have any simulations configured yet and don't know how to start, you can download some FrontLine pre-configured projects by clicking on the "Download sample simulations" green button.

{{< img src="samples.png" alt="Samples" >}}

Those samples are ready to use maven, sbt and gradle projects with proper configuration for FrontLine. You can also download those samples with the download link in the Documentation section.

Back to the Simulations section, at the top, there is an action bar which allow several actions:

- Create a simulation
- Search by simulation or team name
- Edit global properties
- Delete selected simulations

{{< img src="action-bar.png" alt="Action bar" >}}

## Global Properties

Global properties contains every JVM options and system properties used by all of your simulations by default.
Editing those properties will be propagated to all the simulations.

If you don't want to use the default properties, check `Use custom global properties` and enter your own.

{{< img src="properties.png" alt="Properties" >}}

If you want specific properties for a simulation, you will be allowed to ignore those properties by checking the `Override Global Properties` box when creating or editing the simulation:

{{< img src="override.png" alt="Override" >}}

## Creating a simulation

{{< alert warning >}}
FrontLine has a hard run duration limit of 7 days and will abort any test running for longer than that.
This limit exists for both performance (data who grow too humongous to be presented in the dashboard) and security (forgotten test running forever) reasons.
{{< /alert >}}

In order to create a simulation click on the "Create" button in the simulations table. There are 6 steps to create a simulation, 3 of which are optional.

### Step 1: General

{{< img src="create-simulation1.png" alt="Create simulation - Step 1" >}}

- **Name**: the name that will appear on the simulations table.
- **Team**: the team which owns the simulation.
- **Class name**: the package and the name of your simulation scala class in the project that you want to start.

### Step 2: Build configuration

In this step, you'll configure the artifact of the Simulation to execute.

{{< img src="create-simulation2.png" alt="Create simulation - Step 2" >}}

### Step 3: Pools configuration

In this step, you'll configure the pools used for the FrontLine injectors.

FrontLine private beta pools are available in the following regions:

- Europe (Paris)
- US East (N. Virginia)
- US West (N. California)

{{< img src="create-simulation3.png" alt="Create simulation - Step 3" >}}

- **Weight distribution**: on even, every injector will produce the same load. On custom, you have to set the weight in % of each pool (eg the first pool does 20% of the requests, and the second does 80%). The sum of the weight should be 100%.
- **Pools**: defines the pools to be used when initiating the FrontLine injectors.

You can add many pools with a different number of hosts to run your simulation.

After this step, you can save the simulation, or click on *More options* to access optional configuration.

### Step 4 & 5: JVM options & Java System Properties

These steps allows you to defines JVM arguments and system properties used when running this particular simulation. You can choose to override the global properties.

{{< img src="create-simulation4.png" alt="Create simulation - Step 4" >}}
{{< img src="create-simulation5.png" alt="Create simulation - Step 5" >}}

{{< alert tip >}}
JVM options and Java System Properties will be saved in a snapshot that will be available in the run. This information will be visible by anyone who has read access.
You can exclude some properties from being copied if you prefix them with `sensitive.`.
{{< /alert >}}

{{< alert tip >}}
You can configure the `gatling.frontline.groupedDomains` System property to group connection stats from multiple subdomains and avoid memory issues when hitting a very large number of subdomains.
For example, setting this property as `.foo.com, .bar.com` will consolidate stats for `sub1.foo.com`, `sub2.foo.com`, `sub1.bar.com`, `sub2.bar.com` into `*****.foo.com` and `*****.bar.com`.
{{< /alert >}}

### Step 6: Time window

Configuring a ramp up or ramp down means that the start and end of your simulation won't be used for calculating metrics and assertions.

{{< img src="create-simulation6.png" alt="Create simulation - Step 6" >}}

- **Ramp Up**: the number of seconds you want to exclude at the beginning of the run.
- **Ramp Down**: the number of seconds you want to exclude at the end of the run.

{{< alert tip >}}
Ramps parameters will only be applied if the run duration is longer than the sum of the two.
{{< /alert >}}

## Simulations table

Now that you have created a simulation, you can start it by clicking on the {{< icon play >}} icon in the **Start** column of the table.

{{< img src="start.png" alt="Start" >}}

A run have the following life cycle:

- **Building**: in which it will download the simulation artifact and prepare the hosts
- **Deploying**: in which it will deploy the simulation to run on all the hosts
- **Injecting**: in which the simulation is running and viewable from the Reports

{{< img src="injecting.png" alt="Injecting" >}}

{{< anchor logs >}}

By clicking on the {{< icon file-alt >}} icon in the **Build Start** column, Frontline will display the build logs of the simulation. There is a limit of 1000 logs for a run.

{{< img src="logs.png" alt="Logs" >}}

You can click on the {{< icon search >}} icon next to the status (if there is one) to display the assertions of the run.
Assertions are the assumptions made at the beginning of the simulation to be verified at the end:

{{< img src="assertions.png" alt="Assertions" >}}

## Useful tips

- You can edit the simulation by clicking on the {{< icon pencil-alt >}} icon next to his name
- You can search a simulation by his name, or its team name
- You can sort the simulations by any column except the **Start** one
- A **Delete** button will appear on the action bar when you select a simulation, you will be able to delete all the selected simulations
- When a simulation is running, you can abort the run by clicking on the Abort button
- You can copy a simulation ID by clicking on the {{< icon clipboard >}} icon next to his name

Be aware that deleting a simulation will delete all the associated runs.
