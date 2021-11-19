---
title: "Package Generation"
description: "Learn how to generate a package for Gatling Enterprise from the Gatling zip bundle, or from a Maven, SBT, or Gradle project."
lead: "Generate a package from your Gatling bundle or with a Maven, SBT, or Gradle project."
aliases:
 - artifact_gen
date: 2021-03-08T12:50:32+00:00
lastmod: 2021-08-05T13:13:30+00:00
weight: 10040
---

## Generating Packages for Gatling Enterprise

Gatling Enterprise deploys packages containing your compiled Simulations and resources. Those packages have to be generated
upstream, using one of the methods below, before you can run them with Gatling Enterprise.

Gatling Enterprise is compatible with Gatling 3.3, 3.4, 3.5 and 3.6.

### Gatling zip bundle

Once you have created a simulation you want to upload, run the script `artifact.sh` (on Linux or macOS) or `artifact.bat` (on Windows), found in the `bin` directory of your unzipped Gatling bundle.
This will generate a `target/artifact.jar` file. You can then upload this file in the [Packages section]({{< ref "../package_conf" >}}).

{{< alert warning >}}
These scripts are included in the Gatling bundle version 3.6.0 or later. For older versions, you will need to download and copy the
[`artifact.sh`](https://raw.githubusercontent.com/gatling/gatling/master/gatling-bundle/src/universal/bin/artifact.sh)
or [`artifact.bat`](https://raw.githubusercontent.com/gatling/gatling/master/gatling-bundle/src/universal/bin/artifact.bat)
files to the `bin` directory of your unzipped Gatling bundle.
{{< /alert >}}

### Maven Project

Check [Gatling Maven Plugin](https://gatling.io/docs/gatling/reference/current/extensions/maven_plugin/) documentation to setup your project.

{{< alert warning >}}
Gatling Maven Plugin version must be at least 4.0.0
{{< /alert >}}

#### Gatling Enterprise Packaging

`mvn gatling:enterprisePackage` command will generate the `target/<artifactId>-<version>-shaded.jar` package.

{{< alert tip >}}
To make the package lighter, you can also exclude dependencies you don't want to ship, eg:

```xml
<plugin>
  <groupId>io.gatling</groupId>
  <artifactId>gatling-maven-plugin</artifactId>
  <version>{{< var gatlingMavenPluginVersion >}}</version>
  <executions>
    <execution>
      <goals>
        <goal>package</goal>
      </goals>
      <configuration>
        <excludes>
          <exclude>
            <groupId>groupId</groupId>
            <artifactId>artifactId</artifactId>
          </exclude>
        </excludes>
      </configuration>
    </execution>
  </executions>
</plugin>
```

{{< /alert >}}

#### Gatling Enterprise Upload

To upload generated package, refer to the [packages section]({{< ref "../package_conf" >}}).

Alternatively, you can configure Gatling Maven Plugin to upload your package on Gatling Enterprise.

You'll need to [create a package]({{< ref "../package_conf" >}}), and [create an API token]({{< ref "../../admin/api_tokens/#managing-api-tokens" >}}) with at lease `Packages` permission on te package team.

API Token need to be configured as:
- `GATLING_ENTERPRISE_API_TOKEN` environment variable
- `gatling.enterprise.apiToken` java property

Artifact ID must be configured on the plugin:
```xml
<plugin>
  <groupId>io.gatling</groupId>
  <artifactId>gatling-maven-plugin</artifactId>
  <version>${gatling-maven-plugin.version}</version>
  <configuration>
    <packageId>00000000-0000-0000-0000-000000000000</packageId>
  </configuration>
</plugin>
```

### SBT Project

Check [Gatling SBT Plugin](https://gatling.io/docs/gatling/reference/current/extensions/sbt_plugin/) documentation to setup your project.

{{< alert warning >}}
Gatling SBT Plugin version must be at least 4.0.0
{{< /alert >}}

#### Gatling Enterprise Packaging

`sbt "Gatling / enterprisePackage"` or `GatlingIt / enterprisePackage` command will generate the `target/${project}-${version}.jar` package.

{{< alert warning >}}
If you use very long method calls chains in your Gatling code, you might have to increase sbt's thread stack size before packaging:
```bash
export SBT_OPTS="-Xss100M"
```
{{< /alert >}}

#### Gatling Enterprise Upload

To upload generated package, refer to the [packages section]({{< ref "../package_conf" >}}).

Alternatively, you can configure Gatling Maven Plugin to upload your package on Gatling Enterprise.

You'll need to [create a package]({{< ref "../package_conf" >}}), and [create an API token]({{< ref "../../admin/api_tokens/#managing-api-tokens" >}}) with at lease `Packages` permission on te package team.

API Token need to be configured as:
- `GATLING_ENTERPRISE_API_TOKEN` environment variable
- `gatling.enterprise.apiToken` java property

Artifact ID must be configured on the plugin:
```scala
Gatling / enterprisePackageId = "00000000-0000-0000-0000-000000000000"
// or
GatlingIt / enterprisePackageId = "00000000-0000-0000-0000-000000000000"
```

### Gradle Project

Check [Gatling Gradle Plugin](https://gatling.io/docs/gatling/reference/current/extensions/gradle_plugin/) documentation to setup your project.

{{< alert warning >}}
Gatling Gradle Plugin version must be at least 3.7.0
{{< /alert >}}

#### Gatling Enterprise Packaging

`gradle gatlingEnterprisePackage` command will generate the `build/libs/gradle.jar` package.

#### Gatling Enterprise Upload

To upload generated package, refer to the [packages section]({{< ref "../package_conf" >}}).

Alternatively, you can configure Gatling Maven Plugin to upload your package on Gatling Enterprise.

You'll need to [create a package]({{< ref "../package_conf" >}}), and [create an API token]({{< ref "../../admin/api_tokens/#managing-api-tokens" >}}) with at lease `Packages` permission on te package team.

API Token need to be configured as:
- `GATLING_ENTERPRISE_API_TOKEN` environment variable
- `gatling.enterprise.apiToken` java property

Artifact ID must be configured on the plugin:
```groovy
gatling {
    enterprise {
        artifactId '00000000-0000-0000-0000-000000000000'
    }
}
```

### Multi-Module Support

If you have a multi-module project, make sure to only configure the modules which contain Gatling Simulations with the Gatling related plugins described above.
Your Gatling modules can, however, depend on other modules.

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

For example, you might want to hit `https://example.fr` if the injector is deployed in the `Europe - Paris` AWS region, and `https://example.com` otherwise.

In your simulation code, you can resolve the name of the pool in which the injector running the code is deployed:

```scala
val poolName = System.getProperty("gatling.frontline.poolName")
val baseUrl = if (poolName == "Europe - Paris") "https://example.fr" else "https://example.com"
```

{{< alert tip >}}
This System property is only defined when deploying with Gatling Enterprise.
It is not defined when running locally with any Gatling OSS launcher.
{{< /alert >}}
