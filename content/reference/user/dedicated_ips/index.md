---
title: "Dedicated IPs"
description: "Dedicated IP addresses for your pools"
lead: "Dedicated IP addresses for your pools"
date: 2021-03-10T14:29:04+00:00
lastmod: 2021-08-05T13:13:30+00:00
weight: 10061
---

Dedicated IPs allows you to have control over the injectors IP addresses.
This is useful, for instance, if your target system performs some sort of IP address filtering.

{{< img src="dedicated-ips.png" alt="Dedicated IPs" caption="Dedicated IPs" >}}

## Managing

To access the dedicated IPs section, click on **Dedicated IPs** in the navigation bar.

{{< alert tip >}}
You can request dedicated IPs through [technical support](https://gatlingcorp.atlassian.net/servicedesk/customer/portal/8/group/12/create/59).

Please provide:
- Organization SLUG
- Desired number of dedicated IPs per region
- Contact Email
- GitHub username
A sales person will contact you.
{{< /alert >}}

The Dedicated IPs table shows your reserved dedicated IP addresses. Each one belongs to a specific pool.

## Usage

You can enable the use of dedicated IPs when [configuring simulation pools]({{< ref "../simulations#step-3-pools-configuration" >}}).

When starting a run of a simulation configured to use dedicated IPs,
if you have enough dedicated IPs available to satisfy the size of the configured pools,
they will be reserved for the run duration. Otherwise, the run won't start.
