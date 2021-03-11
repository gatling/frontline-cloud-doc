---
title: "Simulations"
description: ""
lead: ""
date: 2021-03-10T09:29:43-05:00
lastmod: 2021-03-10T09:29:43-05:00
draft: false
images: []
menu:
  docs:
    parent: "user"
weight: 050
---

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

WARNING: FrontLine has a hard run duration limit of 7 days and will abort any test running for longer than that.
This limit exists for both performance (data who grow too humongous to be presented in the dashboard) and security (forgotten test running forever) reasons.

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

NOTE: JVM options and Java System Properties will be saved in a snapshot that will be available in the run. This information will be visible by anyone who has read access.
You can exclude some properties from being copied if you prefix them with `sensitive.`.

NOTE: You can configure the `gatling.frontline.groupedDomains` System property to group connection stats from multiple subdomains and avoid memory issues when hitting a very large number of subdomains.
For example, setting this property as `.foo.com, .bar.com` will consolidate stats for `sub1.foo.com`, `sub2.foo.com`, `sub1.bar.com`, `sub2.bar.com` into `*****.foo.com` and `*****.bar.com`.

### Step 6: Time window

Configuring a ramp up or ramp down means that the start and end of your simulation won't be used for calculating metrics and assertions.

{{< img src="create-simulation6.png" alt="Create simulation - Step 6" >}}

- **Ramp Up**: the number of seconds you want to exclude at the beginning of the run.
- **Ramp Down**: the number of seconds you want to exclude at the end of the run.

NOTE: Ramps parameters will only be applied if the run duration is longer than the sum of the two.

## Simulations table

Now that you have created a simulation, you can start it by clicking on the icon:play[] icon in the **Start** column of the table.

{{< img src="start.png" alt="Start" >}}

A run have the following life cycle:

- **Building**: in which it will download the simulation artifact and prepare the hosts
- **Deploying**: in which it will deploy the simulation to run on all the hosts
- **Injecting**: in which the simulation is running and viewable from the Reports

{{< img src="injecting.png" alt="Injecting" >}}

By clicking on the icon:file-alt[] icon in the **Build Start** column, Frontline will display the build logs of the simulation. There is a limit of 1000 logs for a run.

{{< img src="logs.png" alt="Logs" >}}

You can click on the icon:search[] icon next to the status (if there is one) to display the assertions of the run.
Assertions are the assumptions made at the beginning of the simulation to be verified at the end:

{{< img src="assertions.png" alt="Assertions" >}}

## Useful tips

- You can edit the simulation by clicking on the icon:pencil-alt[] icon next to his name
- You can search a simulation by his name, or its team name
- You can sort the simulations by any column except the **Start** one
- A **Delete** button will appear on the action bar when you select a simulation, you will be able to delete all the selected simulations
- When a simulation is running, you can abort the run by clicking on the Abort button
- You can copy a simulation ID by clicking on the icon:clipboard[] icon next to his name

Be aware that deleting a simulation will delete all the associated runs.

## Run / Trends

Runs list and trends can be accessed by clicking on the icon:history[] icon in the <<simulation-table, simulations table>>.

This view contains the list of your simulation's runs which can be filtered by name and/or status and the Trends which are displaying information between those runs.
{{< img src="run-trends.png" alt="Run trends" >}}

### Runs table

{{< img src="run-table.png" alt="Run table" >}}

Like the result of the latest run in the <<simulation-table, simulations table>> you have access to the <<logs, logs>> of the run by clicking on the icon:file-alt[] icon and you can sort the table by each columns. The logs are only available for run which are not flagged as "Successful".

If there is one, You can click on the icon:search[] icon next to the status to display the <<assertions, assertions>> of the run.
You can delete runs by selecting them and click on the **Delete** button in the action bar above the table.

You can comment a run by clicking on the icon:comment-alt[] icon on the right side of the table.

{{< img src="comment.png" alt="Comment" >}}

You can also click on the icon:info-circle[] icon to see a snapshot of the run configuration. The system properties beginning with `sensitive.` are not displayed.

{{< img src="snapshot.png" alt="Snapshot" >}}

### Run Comparison

{{< img src="compare-runs.png" alt="Compare runs" >}}

You can compare the results of two runs if you click on the "Compare runs" button in the table. It allows you to compare the response time and errors of the two runs for each request.

You can choose the specific metric you want to compare by clicking on the metric name, and the specific run you want to compare by clicking on the run number.

The delta and variance will be displayed, so you can check if there is a progression or a degradation in performance.

### Trends charts

The trends are charts that will display some globals statistics for each runs (eg: requests count) so that you can easily see how well your runs went compared to each other.
Each run is represented by his number in the chart and the chart won't display the statistics of a failed run (eg: Timeout, broken, etc..).

{{< img src="trends.png" alt="Trends" >}}

You can filter the statistics shown by filtering through scenarios, groups or requests that are involved in each runs.
You can chose how many runs will be compared by changing the limit (10, 25, 50, 100):

{{< img src="trends-bar.png" alt="Trends bar" >}}

## Reports

The reports can be accessed by clicking on the icon:chart-area[] icon in the <<simulation-table, simulation table>> or in the <<runs-table, runs table>>.


This view introduce all the metrics available for a specific run.
This page consists of:

- <<top-navigation, The top navigation bar>>
- <<timeline, The timeline>>
- <<tabs, Tabs>>
- <<run-bar, The run bar>>
- <<charts, Charts area>>
- <<summary, The summary>> (only for requests and groups tabs)
- <<export, Export PDF>>

{{< img src="reports.png" alt="Reports" >}}

### Top Navigation Bar

The navigation bar enable you to choose the simulation time range.

{{< img src="timewindow.png" alt="Timewindow" >}}

### Timeline

The timeline contains metrics of the full run providing an overview of the run.
Global informations are available such as the resolution and the simulation name.

The resolution indicates the number of seconds per data point in the graph.

You can change the time range with control buttons or by selecting a region on the timeline:

{{< img src="timeline.png" alt="Timeline" >}}

### Assertions

The label below is used to display the status of the simulation (Ongoing, successful, timeout...).
If your simulation has assertions, this label will be clickable to show the assertions results.
You can comment the run run by clicking on the icon:comment-alt[] icon.

{{< img src="timeline-assertions.png" alt="Timeline assertions" >}}

### Tabs

Below the navigator chart, there are tabs to switch charts.
Each tab has the same structure except the summary that is available only for requests and groups tabs.

{{< img src="tabs.png" alt="Tabs" >}}

### Run Bar

This bar is a combination of buttons:

- **Start / Abort**: Use this button to start a new run of the simulation, or stop the ongoing run (not available if you have a Viewer permission)
- **Grafana**: Link to the Grafana dashboard if you have filled in the configuration in frontline.conf
- **Generate public link**: To create a public link
- **Switch to Summary**: Switch to <<summary, summary>> view for Requests & Groups tabs
- buttons to filter the metrics drawn in the charts area

{{< img src="run-bar.png" alt="Run bar" >}}

### Generate Public Links

A public link is a link of the current reports which will be accessible to anyone, without having to log-in to FrontLine. To generate a public link, click on the *Generate public link* button and choose the expiration date of your link.

{{< img src="generate-public-link.png" alt="Generate public links" >}}

The maximum allowed lifetime for a public link is 1 year.

Once you have chosen an expiration date, you can proceed by clicking on the generate button.

{{< img src="successful-generation-public-link.png" alt="Successful public link generation" >}}

You can copy the public link to share your reports to non-FrontLine users, or click on the "Go" Button to access it yourself. You can click on the "OK" button to close this modal.

### Charts

Each charts in FrontLine are connected to each other, so if you select a time window on a chart it will automatically change it
for all other charts. Metrics are drawn in multiple charts.

{{< img src="charts.png" alt="Charts" >}}

Some of them have an icon to update the chart settings:

{{< img src="distrib-chart.png" alt="Distribution chart" >}}

Moreover, histograms and pies are hidden behind each counts charts, accessible by clicking their top right corner icon below.

{{< img src="pie-button.png" alt="Pie button" >}}

WARNING: If your kernel version is too low (around below 3.10) you might not be able to get data from the TCP connection by state graph on the Connections tab. If you want to be able to get these data, you should upgrade your kernel.

### Summary (Requests and Groups only)

This view is available only from requests and groups tabs.
It is a summary of metrics drawn in the charts, and has two modes: flat, by default, and hierarchy.
The summary is also connected to the timeline and the time window selected, so if you change the time window the summary
will refresh his data to match the time window.

On Flat mode you can filter the data by clicking any column name of the table.

{{< img src="summary.png" alt="Summary" >}}

### Export PDF

When clicking on the green button in the navigation bar, you will have access to a page where you can configure and then export a PDF report of a specific simulation.

{{< img src="export-button.png" alt="Export button" >}}

This report is initialized with:

- a title element with the date of the run you were coming from
- the run status
- the run comments
- the run assertions
- the run requests summary
- 3 charts of the run:
* Requests and Responses per second
* Responses per Second by Status
* Response Time Percentiles

{{< img src="export-page.png" alt="Export page" >}}

This page is a configurable list of different elements that will be displayed in the report. You can click on the blue add button under every element
to add another one.

Every element can be moved up or down by clicking on the blue arrow on the top right of the element, or be removed by clicking on the red dash.

Those elements are composed of:

- **Title**: add a title element.
- **Text Area**: add an editable text element.
- **New Page**: allow you to skip a page in the report.
- **Run**:
* **Status**: add an editable text element with a predefined text set to the status of the selected run.
* **Comments**: add an editable text element with a predefined text set to the comments of the selected run.
* **Assertions**: add a table with the assertions of the selected run.
* **Summary**: add the summary table of the selected run in a new landscape page.
- **Chart**: add a chart element that you can interact with before exporting it to PDF.
- **Counts**: add a count chart element that you can interact with before exporting it to PDF.

As you can see below, every charts (or other elements) can be interact with individually. You can zoom on it, or select the run, the scenario,
the group, etc.. whose you want your data to be fetch. You do not need to have the same settings for each element.

{{< img src="export-charts.png" alt="Export charts" >}}

After adding all desired elements in the report you can click on the *Export PDF* button on the top right to get your PDF file.

{{< img src="export-actions.png" alt="Export actions" >}}

There are two more actions you can do:

- **Save**: save the current Export configuration:
  - **as a template**: this option will save the element list without the content
  - **as a save**: this option will save everything, including the content of the Text Area and the configuration of the graphs
- **Load**: load a previously saved template or save.

### Useful Tips

### Zoom

You can reset zoom by double clicking on a chart.
It is possible to change the time range window by the following actions:

- Clicking zoom icons of the control buttons
- Select a zone in any charts and timeline
- Select a range time from the top navigation bar

### Markers

To ease your analysis, you can create markers on all the charts by right clicking on them. And click on the top of the marker to delete it.

{{< img src="marker.png" alt="Marker" >}}

### Multiple Highlights

In the top right menu, you can activate the *Multiple Highlights* setting which allows the tooltip to be displayed on every chart at the same time.

{{< img src="multiplecheck.png" alt="Multiple check" >}}
{{< img src="multiple.png" alt="Multiple" >}}

### Percentiles Mask

In the top right menu, you can click on the **Percentiles** setting to be able to chose what percentiles to display in the chart.

{{< img src="percentilesmask.png" alt="Percentiles mask" >}}
{{< img src="percentileschart.png" alt="Percentiles chart" >}}

### Date Time / Elapsed Time

In the top right menu, you can activate the **Date Time** setting to be able to switch from elapsed time to date time.

### Highlight Legend

By hovering the label on the percentiles chart legend, you will be able to highlight the curve on the chart, leading to a better view of that curve.
The highlight legend options is enable to every "non stacked" graph.

{{< img src="highlightchart.png" alt="Highlight chart" >}}