---
title: "Simulations"
description: "Learn how to configure and navigate through simulations."
lead: "Navigate through simulations."
date: 2021-03-10T14:29:43+00:00
lastmod: 2021-08-05T13:13:30+00:00
weight: 21060
---

## Managing Simulations

To access the Simulations section, click on **Simulations** in the navigation bar.

The Simulations view contains all the simulations configured by your organization and the results of their last run.

{{< img src="simulations-table.png" alt="Simulation table" >}}

If you don't have any simulations configured yet and don't know where to start, you can download some Gatling Enterprise pre-configured projects by clicking on the "Sample simulations" button.

{{< img src="samples-bar.png" alt="Samples" >}}

Samples are distributed under:
- **Scala** with [**Maven**](https://github.com/gatling/gatling-maven-plugin-demo-scala), [**Gradle**](https://github.com/gatling/gatling-gradle-plugin-demo-scala), [**sbt**](https://github.com/gatling/gatling-sbt-plugin-demo) and [**Gatling Enterprise Bundle**](https://gatling.io/open-source/#downloadgatling)
- **Java** with [**Maven**](https://github.com/gatling/gatling-maven-plugin-demo-java), [**Gradle**](https://github.com/gatling/gatling-gradle-plugin-demo-java) and [**Gatling Enterprise Bundle**](https://gatling.io/open-source/#downloadgatling)
- **Kotlin** with [**Maven**](https://github.com/gatling/gatling-maven-plugin-demo-kotlin) and [**Gradle**](https://github.com/gatling/gatling-gradle-plugin-demo-kotlin)

{{< img src="samples.png" alt="Samples" >}}

Back to the Simulations section, at the top-right, there is an action bar which allows several actions:

- Create a simulation
- Search by simulation or team name
- Edit global properties
- Delete selected simulations

{{< img src="action-bar.png" alt="Action bar" >}}

## Default Load Generator Parameters

Default load generator parameters contains every Java system properties and environment variables used by all of your simulations by default.
Editing those properties will be propagated to all the simulations. You can access it by clicking on the top right corner of the page.

If you don't want to use the default properties, check `Use custom global properties` and enter your own.

{{< img src="default-load-generator-properties.png" alt="Properties" >}}

If you want specific properties for a simulation, you will be allowed to ignore those properties by checking the `Ignore defaults` box when creating or editing the simulation:

{{< img src="override-load-generator-properties.png" alt="Override" >}}

## Creating a simulation

You have to upload a package first before creating a simulation.

See how to [generate a package]({{< ref "../package_gen" >}}) and [create it]({{< ref "../package_conf/#creation" >}}) on Gatling Enterprise Cloud.

{{< alert warning >}}
Gatling Enterprise has a hard limit for run durations of 7 days and will stop any test running for longer than that.
This limit exists for both performance reasons (to avoid data growing too large to be presented in the dashboard) and security
reasons (to avoid a forgotten test running forever).
{{< /alert >}}

In order to create a simulation click on the "Create" button in the simulations table. There are 6 steps to create a simulation, 3 of which are optional.

### Step 1: General

{{< img src="create-simulation-general.png" alt="Create simulation - Step 1" >}}

- **Name**: the name that will appear on the simulations table.
- **Team**: the team which owns the simulation.
- **Package**: the [uploaded package]({{< ref "../package_conf/#upload" >}}) (it must belong to the configured team)
- **Class name**: the simulation's fully qualified name, detected in configured package

### Step 2: Locations configuration

In this step, you'll configure the locations used for the Gatling Enterprise load generators.

You can either use the public locations provided by Gatling Enterprise Cloud, or use your own [private locations]({{< ref "../../admin/private_locations" >}})

{{< alert info >}}
It is not currently possible to mix public and private locations in the same simulation.
{{< /alert >}}

Gatling Enterprise public locations are available in the following regions:

- AP Pacific (Hong kong)
- AP Pacific (Tokyo)
- AP Pacific (Mumbai)
- AP SouthEast (Sydney)
- Europe (Dublin)
- Europe (Paris)
- SA East (São Paulo)
- US East (N. Virginia)
- US West (N. California)
- US West (Oregon)

If you want to use private locations, please refer to the [specific documentation]({{< ref "../../admin/private_locations" >}})

In order for the best results from your simulation you should select the load generators that best represent your user base.

{{< img src="create-simulation-locations.png" alt="Create simulation - Step 2" >}}

- **Location**: defines the locations to be used when initiating the Gatling Enterprise load generators.
- **Custom weight distribution**: by default, every load generator will produce the same load. If enabled, you must set the weight in % for each location (e.g. the first location does 20% of the requests, and the second does 80%). The sum of the weight must be 100%.
- **Dedicated IP Addresses**: Check if you want to enable [dedicated IP addresses]({{< ref "../dedicated_ips" >}}) for your load generators. Only available for public locations.

You can add several locations with different numbers of load generators to run your simulation.

After this step, you can save the simulation, or click on *Next* to access optional configurations.

### Step 3: Load Generator Parameters

This step allows you to define the Java system properties and environment variables used when running this particular simulation. Properties/variables entered here will add to the defaults, unless you choose to ignore the defaults. If you keep the defaults, and you add a property/variable with the same key as one from the defaults, the simulation's value will be used (it overrides the default).

{{< img src="create-simulation-load-generator-parameters.png" alt="Create simulation - Step 3" >}}

{{< alert tip >}}
JVM options, Java System Properties and environment variables will be saved in a snapshot that will be available in the run. This information will be visible by anyone who has read access.
You can exclude some system properties from being copied if you prefix them with `sensitive.`, and environment variables if you prefix them with `SENSITIVE_`.
{{< /alert >}}

{{< alert tip >}}
You can configure the `gatling.frontline.groupedDomains` Java System property to group connection stats from multiple subdomains and avoid memory issues when hitting a very large number of subdomains.

For example, setting this property as `.foo.com, .bar.com` will consolidate stats for `sub1.foo.com`, `sub2.foo.com`, `sub1.bar.com`, `sub2.bar.com` into `*****.foo.com` and `*****.bar.com`.
{{< /alert >}}

{{< alert tip >}}
System properties can be retrieved in your Gatling simulation with `System.getProperty("YOUR_PROPERTY_KEY")`.

Environment variables can be retrieved in your Gatling simulation with `System.getEnv("YOUR_ENV_VAR_KEY")`.
{{< /alert >}}


### Step 4: Time window

Configure some ramp up or ramp down time windows to be excluded when computing assertions. This is typically useful when you know that at the beginning of your test run you're going to expected higher response times than when your system is warm (JIT compiler has kicked in, autoscaling has done its work, caches are filled...) and don’t want them to cause your assertions to fail.

{{< img src="create-simulation-timewindow.png" alt="Create simulation - Step 4" >}}

- **Ramp Up**: the number of seconds you want to exclude at the beginning of the run.
- **Ramp Down**: the number of seconds you want to exclude at the end of the run.

{{< alert tip >}}
Ramp up/down parameters will only be applied if the run duration is longer than the sum of the two.
{{< /alert >}}

## Simulations table

Once you have created a simulation, you can start it by clicking on the {{< icon play >}} icon in the **Start** column of the table.

{{< img src="start.png" alt="Start" >}}

A run has the following life cycle:

- **Building**: in which it will download the simulation package and prepare the hosts
- **Deploying**: in which it will deploy the simulation to run on all the load generators
- **Injecting**: in which the simulation is running and can be viewed from the Reports. 

{{< img src="injecting.png" alt="Injecting" >}}

{{< anchor logs >}}

By clicking on the second icon on last column, Gatling Enterprise will display the build logs of the simulation. There is a limit of 1000 logs for a run.

Viewing the Log can also be helpful in determining why a run failed and what errors you will need to correct to successfully run your simulation.

The logs can also be viewed in the Reports, while the simulation is building.

{{< img src="logs.png" alt="Logs" >}}

You can click on the third icon on last column to display the assertions of the run.
Assertions are the assumptions made at the beginning of the simulation to be verified at the end:

{{< img src="assertions.png" alt="Assertions" >}}

## Useful tips

- You can edit, copy the ID, duplicate and delete the simulation by clicking on the kebab menu icon
- You can search a simulation by its name, or its team name
- You can sort the simulations by any column
- A **Delete** button will appear on the action bar when you select a simulation, you will be able to delete all the selected simulations
- When a simulation is running, you can stop the ongoing run by clicking on the Stop button

Be aware that deleting a simulation will delete all the associated runs.
