---
title: "Private Locations"
description: "Host Load Generators on your private AWS account"
lead: "Private Locations on your AWS account"
date: 2021-11-07T14:29:04+00:00
lastmod: 2021-11-07T14:29:04+00:00
weight: 21070
---

A location is a configuration of load generators.
Private locations are a special type of locations, hosted on your own servers.

The goal of private locations is to load test websites behind a firewall, by hosting the load generators on the same network as the target website.

We currently only support private locations on AWS.

{{< alert tip >}}
Private locations can be required through our [technical support](https://gatlingcorp.atlassian.net/servicedesk/customer/portal/8/group/12/create/59).

Please provide:
- Organization Slug (click on the **Organization** in the navigation bar to retrieve this)
- Contact Email
A sales person will contact you.
{{< /alert >}}

## How it works

In order to use private locations, you'll need to install a component on your AWS account: the Gatling Enterprise control plane.

The control plane will be in charge of spawning load generators on your own AWS account.
Please note that the connections will always be initiated by the control plane and load generators.

## Configuration on Gatling Enterprise Cloud

To access the private locations section, click on **Private locations** in the navigation bar.

Click on **Create control plane** to create a new control plane.

{{< img src="create-control-plane.png" alt="Create control plane" >}}

On the next modal, make sure to copy the control plane token, you'll need it later.

{{< img src="copy-token.png" alt="Copy control plane token" >}}

You can see the control plane you just created under the **Control planes** tab.

{{< img src="control-planes-uninitialized.png" alt="Control Planes table" >}}

## Control Plane configuration

### Control Plane installation

The Gatling Enterprise control plane is distributed as a docker image, available [here](https://hub.docker.com/r/gatlingcorp/control-plane).

You need to install it on your own AWS account, for example on AWS Fargate.

### Control Plane configuration file

The control plane is configured by a hocon configuration file, see sample below:

{{< include-code "control-plane.conf" hocon >}}

First, you'll need to configure your own control plane token:
```hocon
  token = "YOUR_CONTROL_PLANE_TOKEN"
```

The description is a field that will be seen in the Gatling Enterprise Cloud UI.

The pools part contains a list of the wanted private locations, you can configure multiple locations by separating them with a **,**

Here is the configuration of a private location:
```hocon
  {
    id = "shp_my_private_lovation" <1>
    description = "My own Private Location" <2>
    type = "aws" <3>
    region = "eu-west-1" <4>
    ami = {
      type = "certified" <5>
      java = "17" # Possible values : "8", "11" or 17" <6>
    }
    security-groups = ["sg-mysecuritygroup"] <7>
    instance-type = "xlarge" <8>
    vpc = "vpc-abskdls" <9>
    subnets = ["subnet-abcd"] <10>
    profile-name = "profilename" <11>
    iam-instance-profile = "my iam instance profile" <12>
    tags {
      Type = gatling
    }
  }
```

- <1> Unique identifier for each location, must start with **shp_**
- <2> The description is seen in the Gatling Enterprise Cloud UI
- <3> We only support aws private locations at the moment
- <4> AWS region where you want to spawn the load generators, see the [AWS doc](https://docs.aws.amazon.com/en_us/AWSEC2/latest/UserGuide/using-regions-availability-zones.html#concepts-regions)
- <5> Specify if you want to use the certified AMIs distributed by Gatling Corp, or use your own. If you want to use your own, replace this block by:
```hocon
    ami = {
      type = "custom"
      id = "YOUR_CUSTOM_AMI_ID"
    }
```
- <6> Specify which JDK version the load generator should run with
- <7> List containing the security groups that will be used by the load generators, separate them with a **,**
- <8> Instance type that will be used by the load generators
- <9> Vpc that will be used by the load generators
- <10> Subnets that will be used by the load generators
- <11> Optional configuration of an AWS profile name (if the IAM instance profile is not set)
- <12> Optional configuration of an IAM instance profile (if the profile name is not set)

Please edit the sample file with your own configuration.

### URL allow list

The control plane and load generators need access to some Gatling Enterprise cloud components.
Please note that the connections will always be initiated by the control plane and load generators.

The following URLs will need to be added to the firewall domain list:

-

## Managing Control Planes on Gatling Enterprise Cloud

Once the control plane has been configured, and is up, you should be able to see it in Gatling Enterprise Cloud, under **Private locations**, in the **Control planes** tab.

You can see the details by clicking on the {{< icon eye >}} button, and refresh the control plane token by clicking on the {{< icon undo >}} button.

{{< img src="control-plane-details.png" alt="Control Plane details" >}}

{{< alert warning >}}
If the control plane is not up, you won't be able to use the associated locations.
{{< /alert >}}

Private locations can be seen directly in the **Locations** tab.
You can view each location details and simulation. You can only delete a private location if it is not linked to any control plane or simulation.

{{< img src="locations-table.png" alt="Private location table" >}}

## Usage

You can use private locations when [configuring simulation locations]({{< ref "../simulations#step-2-locations-configuration" >}}).

{{< img src="simulation-config.png" alt="Simulation Configuration" >}}

{{< alert info >}}
It is not possible to mix public and private locations in the same simulation.
{{< /alert >}}

## Note on credit consumption

Gatling Enterprise cloud credits will be used when using private locations, at the same rate as public locations (1 credit per load generator, per minute).
