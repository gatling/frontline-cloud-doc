---
title: "Package Generation"
description: "Learn how to generate a package for Gatling Enterprise from the Gatling zip bundle, or from a Maven, SBT, or Gradle project."
lead: "Generate a package from your Gatling bundle or with a Maven, SBT, or Gradle project."
aliases:
 - artifact_gen
date: 2021-03-08T12:50:32+00:00
lastmod: 2021-11-22T16:00:00+00:00
weight: 21040
---

## Generating Packages for Gatling Enterprise

Gatling Enterprise deploys packages containing your compiled Simulations and resources. Those packages have to be generated
upstream, using one of the methods below, before you can run them with Gatling Enterprise.

Gatling Enterprise is compatible with Gatling 3.3, 3.4, 3.5, 3.6 and 3.7.

{{< alert tip >}}
If you go to the [Simulations page]({{< ref "../simulations" >}}) in the Gatling Enterprise Cloud app, you can click on
"Sample simulations" to download sample projects for all the options below.
{{< /alert >}}

### Gatling zip bundle

Once you have created a simulation you want to upload, run the script `enterprisePackage.sh` (on Linux or macOS) or
`enterprisePackage.bat` (on Windows), found in the `bin` directory of your unzipped Gatling bundle. This will generate a
`target/package.jar` file. You can then upload this file in the [Packages section]({{< ref "../package_conf" >}}).

```shell
$ ./bin/enterprisePackage.sh
```

You can also create a package and upload it in a single command, using the script `enterpriseDeploy.sh` (on Linux or
macOS) or `enterprisePackage.bat` (on Windows). You must have already [configured a package]({{< ref "../package_conf" >}})
(copy the package ID from the Packages table). You also need [an API token]({{< ref "../../admin/api_tokens" >}}) with
appropriate permissions to upload a package.

Run the script:

```shell
$ ./bin/enterpriseDeploy.sh --packageId YOUR_PACKAGE_ID --apiToken YOUR_API_TOKEN
```

Alternatively, the API token can be set with the environment variable `GATLING_ENTERPRISE_API_TOKEN`:

```shell
$ export GATLING_ENTERPRISE_API_TOKEN=YOUR_API_TOKEN
$ ./bin/enterpriseDeploy.sh --packageId YOUR_PACKAGE_ID
```

{{< alert warning >}}
These scripts are included in the Gatling bundle version 3.7.0 or later. For older versions, you will need to download
the `enterprisePackage` and `enterpriseDeploy` script files
[from the Gatling GitHub repository](https://github.com/gatling/gatling/tree/main/gatling-bundle/src/universal/bin)
and copy them to the `bin` directory of your unzipped Gatling bundle.
{{< /alert >}}

### Maven, Gradle or SBT project

To set up you project, and to learn how to use you preferred build tool to upload your simulations to Gatling Enterprise
Cloud, please refer to the documentation pages of the respective plugins:

- [Gatling plugin for Maven](https://gatling.io/docs/gatling/reference/current/extensions/maven_plugin/) (for Java, Kotlin and Scala)
- [Gatling plugin for Gradle](https://gatling.io/docs/gatling/reference/current/extensions/gradle_plugin/) (for Java, Kotlin and Scala)
- [Gatling plugin for SBT](https://gatling.io/docs/gatling/reference/current/extensions/sbt_plugin/) (for Scala)

{{< alert warning >}}
The Gatling build plugins now include everything you need to work with Gatling Enterprise. Previous versions required an
additional plugin to work with Gatling Enterprise. If you have one of the old "FrontLine" plugins
(`io.gatling.frontline:frontline-maven-plugin`, `io.gatling.frontline:frontline-gradle-plugin` or
`io.gatling.frontline:sbt-frontline`) in your build, we recommend removing it and updating to the latest version of the
Gatling plugin instead.
{{< /alert >}}

## Note on Feeders

A typical mistake with Gatling and Gatling Enterprise is to rely on having an exploded Maven/Gradle/SBT project structure, and to try to load files from the project filesystem.

This filesystem structure will not be accessible once your project has been packaged and deployed to Gatling Enterprise.

If your feeder files are packaged with your test sources, you must resolve them from the classpath. This will work both
when you run simulations locally and when you deploy them to Gatling Enterprise.

```scala
// incorrect
val feeder = csv("src/test/resources/foo.csv")

// correct
val feeder = csv("foo.csv")
```

## Load Sharding

Injection rates and throttling rates are automatically distributed amongst nodes.

However, Feeders data is not automatically sharded, as it might not be the desired behavior.

If you want data to be unique cluster-wide, you have to explicitly tell Gatling to shard the data, e.g.:

```scala
val feeder = csv("foo.csv").shard
```

Assuming the CSV file contains 1000 entries, and you run your simulation on 3 Gatling nodes, the entries will be distributed as follows:

- First node will access the first 333 entries
- Second node will access the next 333 entries
- Third node will access the last 334 entries

{{< alert tip >}}
`shard` is available in Gatling OSS DSL but is a noop there. It's only effective when running tests with Gatling Enterprise.
{{< /alert >}}

## Resolving Injector Location in Simulation

When running a distributed test from multiple locations, you could be interested in knowing where a given injector is deployed in order to trigger specific behaviors depending on the location.

For example, you might want to hit `https://example.fr` if the injector is deployed in the `Europe - Paris` region, and `https://example.com` otherwise.

In your simulation code, you can resolve the name of the pool in which the injector running the code is deployed:

```scala
val poolName = System.getProperty("gatling.frontline.poolName")
val baseUrl = if (poolName == "Europe - Paris") "https://example.fr" else "https://example.com"
```

{{< alert tip >}}
This System property is only defined when deploying with Gatling Enterprise.
It is not defined when running locally with any Gatling OSS launcher.
{{< /alert >}}
