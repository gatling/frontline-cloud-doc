---
title: "Private Locations"
description: "Load Generators on your private AWS account"
lead: "Private Locations on your AWS account"
date: 2021-11-07T14:29:04+00:00
lastmod: 2021-11-07T14:29:04+00:00
weight: 22050
---

A location describes where to start load generators:
- Public locations are available by default, and listed by regions.
- Private locations must be configured, and allow you to deploy load generators on your own infrastructure.

{{< alert tip >}}
Private locations allow you to load test in a private infrastructure, without any inbound connection or credentials sharing.
To access this feature, please contact our [technical support](https://gatlingcorp.atlassian.net/servicedesk/customer/portal/8/group/12/create/59?summary=Private+Locations&description=Contact%20email%3A%20%3Cemail%3E%0A%0AHello%2C%20we%20would%20like%20to%20enable%20the%20private%20locations%20feature%20on%20our%20organization.).
{{< /alert >}}

## Introduction

To configure private locations, you must configure an agent in your infrastructure, called a **Control Plane**.

This control plane will be in charge of spawning load generators in different locations, based on the configuration you'll provide.

{{< img src="schema.png" alt="Infrastructure schema" >}}

The control plane will periodically poll our API gateway to find out if a new simulation run has been started using locations handled by this control plane.

If so, it will start new instances (based on the locations configurations) and start the simulation run on them. 
Those instances will send stats through the API gateway as well.

{{< alert info >}}
Only two outbound domains must be allowed in your network:
- The API Gateway: `api.gatling.io` on port `443`
- Download of the Gatling simulation on S3: `eu-west-3.amazonaws.com` on port `443`
{{< /alert >}}

## Control plane configuration

### Control plane token

Access the private locations section by clicking on the Private locations in the navigation bar (only visible if the feature is activated on your organization).

Click on create control plane, and fill a unique identifier. (only lowercase, separated by underscores)

{{< img src="create-control-plane.png" alt="Create control plane" >}}

On the next modal, make sure to copy the control plane token, you’ll need it later.

{{< img src="copy-token.png" alt="Copy control plane token" >}}

You can see the control plane you just created under the Control planes tab.

{{< img src="control-planes-uninitialized.png" alt="Control Planes table" >}}

It's defined by a unique identifier, a description (provided later on by its configuration) and a status.

The status has three possible values:
- **Uninitialized:** the control plane has never contacted the application
- **Up:** the control plane is properly configured, has uploaded the names of its configured private locations, and periodically calls the application to fetch new runs.
- **Down:** the control plane has been up, but hasn't called the application for a while.

### Control plane deployment

The control plane agent is distributed as a docker image: [`gatlingcorp/control-plane`](https://hub.docker.com/r/gatlingcorp/control-plane).
It is configured by a file mounted at `/app/conf/control-plane.conf`.

{{< alert info >}}
The configuration file uses the [HOCON format (Human-Optimized Config Object Notation)](https://github.com/lightbend/config/blob/master/HOCON.md):
{{< /alert >}}

Example with an AWS location (only supported type at the moment):
{{< include-code "control-plane.conf" bash >}}

Only control plane and locations ID, description and type are transferred to Gatling Enterprise Cloud.

## Managing Control Planes on Gatling Enterprise Cloud

Once configured and running, your control plane status should go {{< badge success >}}Up{{< /badge >}} within a few seconds, details are available by clicking on the {{< icon eye >}} button.
Tokens can be refresh by clicking on the {{< icon undo >}} button.

{{< img src="control-plane-details.png" alt="Control Planes details" >}}

Private locations can be seen in the Locations tab.
You can see their relations to their control plane and simulations.

{{< img src="locations-table.png" alt="Locations table" >}}

They can be deleted if they are neither linked to a control plane that is currently `Up` nor to any simulation.

# Simulation configuration

When configuring a simulation, on the locations step, click on private.

{{< img src="simulation-config.png" alt="Simulation configuration of private locations" >}}

{{< alert warning >}}
At the moment, it's not possible to use private locations along with public ones.
{{< /alert >}}

# Credit consumption

Private locations consume credits like public locations: one credit, per load generator, per minute.
