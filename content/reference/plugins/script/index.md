---
title: "Manual scripting"
description: "Learn how to configure a script to run your simulations."
lead: "Run your Gatling Enterprise simulations from your CI."
date: 2021-03-08T12:50:17+00:00
lastmod: 2021-08-05T13:13:30+00:00
weight: 30040
---

## Purpose of this script

This script has the same purpose as the Jenkins, Bamboo or Teamcity plugin. It enables you to launch a Gatling Enterprise simulation and display live metrics.

This script doesn’t create a new Gatling Enterprise simulation, you have to create it using the Gatling Enterprise Dashboard before.

## Requirement

jq is needed to run this script. It is a JSON processor available for download [here](https://stedolan.github.io/jq/download/).

## Use

You need to give 3 parameters to the script:

- gatling enterprise url: please enter: https://cloud.gatling.io
- api token: The [API token]({{< ref "../../admin/api_tokens" >}}) will allow Jenkins to authenticate to Gatling Enterprise. The API token needs the All permission.
- simulation id: id of the simulation you want to start. You can get this id on the simulation table, with the {{< icon clipboard >}} icon.

## Script

It can be found [here](https://downloads.gatling.io/releases/frontline-ci-script/{{< var externalPluginsVersion >}}/frontline-ci-script-{{< var externalPluginsVersion >}}.zip)
