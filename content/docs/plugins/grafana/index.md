---
title: "Grafana Datasource"
description: ""
lead: ""
date: 2021-03-08T13:50:08+01:00
lastmod: 2021-03-08T13:50:08+01:00
draft: false
images: []
menu:
  docs:
    parent: "plugins"
weight: 050
---

## Requirements

Download and install link:http://grafana.org/download/[Grafana].

## Grafana datasource installation

The FrontLine datasource for Grafana is packaged as a zip bundle that you can found at this URL:

```
https://downloads.gatling.io/releases/frontline-grafana-bundle/REPLACE_WITH_LATEST_FRONTLINE_VERSION/frontline-grafana-bundle-REPLACE_WITH_LATEST_FRONTLINE_VERSION-bundle.zip
```

You can install it using the grafana-cli:

`grafana-cli --pluginUrl GRAFANA_DATASOURCE_BUNDLE_URL plugins install frontline`

## Adding the datasource

- In the Grafana side menu you will find the `Data Sources` link under `Configuration` (link name may depend on the grafana version).

{{< img src="datasource-menu.png" alt="" >}}

TIP: NOTE: If this link is missing in the side menu, it means that your current user does not have the `Admin` role for the current organization.

- Click the `Add data source` link in the top header.
- Select `FrontLine`.

| Name      | Description                                                                           |
|-----------|---------------------------------------------------------------------------------------|
| Name      | The datasource name.                                                                  |
| Default   | Should be checked if you want that datasource to be selected by default in new panels |
| URL       | URL of your FrontLine server. Default port is 10542. Don't forget the http(s)://      |
| Access    | Server  access via Grafana backend, Browser  access directly from browser.            |
| Auth      | FrontLine datasource ignore these fields.                                             |
| API Token | Token generated through the FrontLine dashboard.                                      |

{{< img src="configure-datasource.png" alt="pdfheight=20%" >}}

## Templating

TIP: Our team provides some dashboards that you can import if you don't want to bother about setting up all the charts and these template variables.

TIP: Samples are in the `dashboardSamples` directory in your Grafana bundle.  +
They are built with a datasource named *FrontLine*. Make sure this datasource exists or modify the json file accordingly.

To use the FrontLine datasource in Grafana, you will need to set template variables.

These are global dashboard parameters that can be used in your graphs with the query builder.

To create a variable on your dashboard, Click the settings button on the top right menu of the dashboard, go to the Variables tab and click on the `+ New` button.

For example, to create the simulation template variable and use them in your graphs:

{{< img src="templating-edit.png" alt="" >}}

You will need for the scopes to use `Custom` type as shown below :

{{< img src="templating-edit-custom.png" alt="" >}}

At the end, your template variables should be something like:

{{< img src="templating.png" alt="" >}}

{{< img src="dashboard.png" alt="" >}}

## FrontLine Metrics

TIP: You can get the list of metrics and test the results via FrontLine's Swagger UI, available through `Documentation` and `FrontLine API Documentation` in the FrontLine dashboard.

FrontLine offers a large amount of metrics:

- **Requests metrics:**
    - **req.<Percentiles>**: Response time percentiles metric.
    - **requests**: Requests count per second.
    - **responsesOk**: Responses OK count per second.
    - **responsesKo**: Responses KO count per second.
    - **responsesByStatus**: Requests count by status per second.
    - **errors**: Erros count by error name per second.
- **Group metrics:**
    - **groupCumulated.<Percentiles>**: Group cumulated response time percentiles.
    - **groupDuration.<Percentiles>**: Group duration percentiles metric.
    - **groupCountsOk**: Group count OK per second.
    - **groupCountsKo**: Group count KO per second.
- **Users metrics:**
    - **usr+**: Users arrival count per second.
    - **usr-**: Users temination count per second.
    - **usrActive**: Concurrent users count per second.
- **Injectors metrics:**
    - **gcCount**: GC count per second.
    - **gcTime**: GC time per second.
    - **heapUsed**: Heap used in Mb.
    - **heapMax**: Heap max in Mb.
    - **heapCommitted**: Heap committed in Mb.
    - **tcpSeg**: TCP events count per second.
    - **tcpConn**: TCP connection events count per second.
    - **cpuSys**: CPU system usage in percent.
    - **cpuUser**: CPU user usage in percent.
- **Bandwidth metrics:**
    - **bitsSent**: Bits sent per second.
    - **bitsReceived**: Bits received per second.
- **Connections metrics:**
    - **connectionOpened**: Connection opened count per second.
    - **connectionClosed**: Connection closed count per second.
    - **connectionTcpState**: TCP connection count by state.
    - **tcp.<Percentiles>**:  TCP connect duration percentiles metric.
    - **tls.<Percentiles>**: TLS handshake duration percentiles metric.
- **DNS metrics:**
    - **dns.<Percentiles>**: DNS resolution duration percentiles metric.

NOTE: <Percentiles> are: min, p25, p50, p75, p80, p85, p90, p95, p99, p999, p9999, max, mean and pAll. (pAll is useful if you want to display in the same graph).

## Metric Requirements

| Metrics name                 | simulation   | scenario     | group        | request      | remote       | hostname     |injector      |scope           |
|------------------------------|--------------|--------------|--------------|--------------|--------------|--------------|--------------|----------------|
| req.<Percentiles>            | icon:check[] | icon:check[] | icon:check[] | icon:check[] |              |              |              | icon:check[]   |
| requests                     | icon:check[] | icon:check[] | icon:check[] | icon:check[] |              |              |              |                |
| responsesOk                  | icon:check[] | icon:check[] | icon:check[] | icon:check[] |              |              |              |                |
| responsesKo                  | icon:check[] | icon:check[] | icon:check[] | icon:check[] |              |              |              |                |
| responsesByStatus            | icon:check[] | icon:check[] | icon:check[] | icon:check[] |              |              |              |                |
| errors                       | icon:check[] | icon:check[] | icon:check[] | icon:check[] |              |              |              |                |
| groupCumulated.<Percentiles> | icon:check[] | icon:check[] | icon:check[] |              |              |              |              | icon:check[]   |
| groupDuration.<Percentiles>  | icon:check[] | icon:check[] | icon:check[] |              |              |              |              | icon:check[]   |
| groupCountsOk                | icon:check[] | icon:check[] | icon:check[] |              |              |              |              |                |
| groupCountsKo                | icon:check[] | icon:check[] | icon:check[] |              |              |              |              |                |
| usr+                         | icon:check[] |              |              |              |              |              |              |                |
| usr-                         | icon:check[] |              |              |              |              |              |              |                |
| usrActive                    | icon:check[] |              |              |              |              |              |              |                |
| gcCount                      | icon:check[] |              |              |              |              |              | icon:check[] |                |
| gcTime                       | icon:check[] |              |              |              |              |              | icon:check[] |                |
| heapUsed                     | icon:check[] |              |              |              |              |              | icon:check[] |                |
| heapMax                      | icon:check[] |              |              |              |              |              | icon:check[] |                |
| heapCommitted                | icon:check[] |              |              |              |              |              | icon:check[] |                |
| tcpSeg                       | icon:check[] |              |              |              |              |              | icon:check[] |                |
| tcpConn                      | icon:check[] |              |              |              |              |              | icon:check[] |                |
| cpuSys                       | icon:check[] |              |              |              |              |              | icon:check[] |                |
| cpuUser                      | icon:check[] |              |              |              |              |              | icon:check[] |                |
| bitsSent                     | icon:check[] |              |              |              | icon:check[] |              |              |                |
| bitsReceived                 | icon:check[] |              |              |              | icon:check[] |              |              |                |
| connectionOpened             | icon:check[] |              |              |              | icon:check[] |              |              |                |
| connectionClosed             | icon:check[] |              |              |              | icon:check[] |              |              |                |
| connectionTcpState           | icon:check[] |              |              |              | icon:check[] |              |              |                |
| tcp.<Percentiles>            | icon:check[] |              |              |              | icon:check[] |              |              | icon:check[]   |
| tls.<Percentiles>            | icon:check[] |              |              |              | icon:check[] |              |              | icon:check[]   |
| dns.<Percentiles>            | icon:check[] |              |              |              |              | icon:check[] |              | icon:check[]   |