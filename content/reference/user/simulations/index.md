---
title: "Simulations"
description: "Learn how to configure and navigate through simulations."
lead: "Navigate through simulations."
date: 2021-03-10T14:29:43+00:00
lastmod: 2021-08-05T13:13:30+00:00
weight: 10060
---

## Managing Simulations

To access the Simulations section, click on **Simulations** in the navigation bar.

The Simulations view contains all the simulations you have configured and the result of their last run.

{{< img src="simulations-table.png" alt="Simulation table" >}}

If you don't have any simulations configured yet and don't know how to start, you can download some Gatling Enterprise pre-configured projects by clicking on the "Sample simulations" button.

{{< img src="samples.png" alt="Samples" >}}

Those samples are ready to use maven, sbt and gradle projects with proper configuration for Gatling Enterprise. You can also download those samples with the download link in the Documentation section.

Back to the Simulations section, at the top-right, there is an action bar which allows several actions:

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
Gatling Enterprise has a hard limit for run durations of 7 days and will stop any test running for longer than that.
This limit exists for both performance reasons (to avoid data growing too large to be presented in the dashboard) and security
reasons (to avoid a forgotten test running forever).
{{< /alert >}}

In order to create a simulation click on the "Create" button in the simulations table. There are 6 steps to create a simulation, 3 of which are optional.

### Step 1: General

{{< img src="create-simulation1.png" alt="Create simulation - Step 1" >}}

- **Name**: the name that will appear on the simulations table.
- **Team**: the team which owns the simulation.
- **Class name**: the package and class name of the simulation scala class you want to start within your project.

### Step 2: Build configuration

In this step, you'll configure the artifact of the Simulation to execute.

{{< img src="create-simulation2.png" alt="Create simulation - Step 2" >}}

### Step 3: Pools configuration

In this step, you'll configure the pools used for the Gatling Enterprise injectors.

Gatling Enterprise pools are available in the following regions:

- Europe (Paris)
- US East (N. Virginia)
- US West (N. California)

{{< img src="create-simulation3.png" alt="Create simulation - Step 3" >}}

- **Weight distribution**: if set to even, every injector will produce the same load. If set to custom, you must set the weight in % for each pool (eg the first pool does 20% of the requests, and the second does 80%). The sum of the weight must be 100%.
- **Pools**: defines the pools to be used when initiating the Gatling Enterprise injectors.

You can add several pools with different numbers of injectors to run your simulation.

After this step, you can save the simulation, or click on *More options* to access optional configurations.

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
Ramp up/down parameters will only be applied if the run duration is longer than the sum of the two.
{{< /alert >}}

## Simulations table

Once you have created a simulation, you can start it by clicking on the {{< icon play >}} icon in the **Start** column of the table.

{{< img src="start.png" alt="Start" >}}

A run has the following life cycle:

- **Building**: in which it will download the simulation artifact and prepare the hosts
- **Deploying**: in which it will deploy the simulation to run on all the injectors
- **Injecting**: in which the simulation is running and can be viewed from the Reports

{{< img src="injecting.png" alt="Injecting" >}}

{{< anchor logs >}}

By clicking on the {{< icon file-alt >}} icon in the **Build Start** column, Gatling Enterprise will display the build logs of the simulation. There is a limit of 1000 logs for a run.

{{< img src="logs.png" alt="Logs" >}}

You can click on the {{< icon search >}} icon next to the status (if there is one) to display the assertions of the run.
Assertions are the assumptions made at the beginning of the simulation to be verified at the end:

{{< img src="assertions.png" alt="Assertions" >}}

## Useful tips

- You can edit the simulation by clicking on the {{< icon pencil-alt >}} icon next to its name
- You can search a simulation by its name, or its team name
- You can sort the simulations by any column except the **Start** one
- A **Delete** button will appear on the action bar when you select a simulation, you will be able to delete all the selected simulations
- When a simulation is running, you can stop the ongoing run by clicking on the Stop button
- You can copy a simulation ID by clicking on the {{< icon clipboard >}} icon next to its name

Be aware that deleting a simulation will delete all the associated runs.
