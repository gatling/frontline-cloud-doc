---
title: "Artifact Generation"
description: "Learn how to generate an artifact for Gatling Enterprise from the Gatling zip bundle, or from a Maven, SBT, or Gradle project."
lead: "Generate an artifact from your Gatling bundle or with a Maven, SBT, or Gradle project."
date: 2021-03-08T12:50:32+00:00
lastmod: 2021-08-05T13:13:30+00:00
weight: 10040
---

## Generating Artifacts for FrontLine

Gatling Enterprise deploys packages containing your compiled Simulations and resources.
Those packages have to be generated upstream.
Gatling Enterprise is compatible with Gatling 3.3, 3.4, 3.5 and 3.6.

### Gatling zip bundle

Run the script `artifact.sh` (on Linux or macOS) or `artifact.bat` (on Windows), found in the `bin` directory of your unzipped Gatling bundle.
This will generate a `target/artifact.jar` file. You can then upload this file in the [Artifacts section]({{< ref "../artifact_conf" >}}).

{{< alert warning >}}
The scripts are included in the Gatling bundle version 3.6.0 or superior. For older versions, you will need to download and copy the
[`artifact.sh`](https://raw.githubusercontent.com/gatling/gatling/master/gatling-bundle/src/universal/bin/artifact.sh)
or [`artifact.bat`](https://raw.githubusercontent.com/gatling/gatling/master/gatling-bundle/src/universal/bin/artifact.bat)
files to the `bin` directory of your unzipped Gatling bundle.
{{< /alert >}}

### Maven Project

In your `pom.xml` file, you must add:

- the Gatling dependencies
- the Scala Maven plugin for Maven (`scala-maven-plugin`), which compiles your code
- the Gatling Enterprise Maven plugin (`frontline-maven-plugin`), which packages your code into a deployable artifact

The `pom.xml` file should contain this:

```xml
<dependencies>
  <dependency>
    <groupId>io.gatling.highcharts</groupId>
    <artifactId>gatling-charts-highcharts</artifactId>
    <version>{{< var gatlingVersion >}}</version>
    <scope>test</scope>
  </dependency>
</dependencies>

<build>
  <!-- so maven compiles src/test/scala and not only src/test/java -->
  <testSourceDirectory>src/test/scala</testSourceDirectory>
  <plugins>
    <plugin>
      <artifactId>maven-jar-plugin</artifactId>
      <version>{{< var mavenJarPluginVersion >}}</version>
    </plugin>
    <!-- so maven can compile your scala code -->
    <plugin>
      <groupId>net.alchim31.maven</groupId>
      <artifactId>scala-maven-plugin</artifactId>
      <version>{{< var scalaMavenPluginVersion >}}</version>
      <executions>
        <execution>
          <goals>
            <goal>testCompile</goal>
          </goals>
          <configuration>
            <recompileMode>all</recompileMode>
            <jvmArgs>
              <jvmArg>-Xss100M</jvmArg>
            </jvmArgs>
            <args>
              <arg>-target:jvm-1.8</arg>
              <arg>-deprecation</arg>
              <arg>-feature</arg>
              <arg>-unchecked</arg>
              <arg>-language:implicitConversions</arg>
              <arg>-language:postfixOps</arg>
            </args>
          </configuration>
        </execution>
      </executions>
    </plugin>

    <!-- so maven can build a package for FrontLine -->
    <plugin>
      <groupId>io.gatling.frontline</groupId>
      <artifactId>frontline-maven-plugin</artifactId>
      <version>{{< var frontLineMavenPluginVersion >}}</version>
      <executions>
        <execution>
          <goals>
            <goal>package</goal>
          </goals>
        </execution>
      </executions>
    </plugin>
  </plugins>
</build>
```

Run the `mvn clean package -DskipTests` command. This will generate the `target/<artifactId>-<version>-shaded.jar` file.
You can then upload this file in the [Artifacts section]({{< ref "../artifact_conf" >}}).

{{< alert tip >}}
To make the artifact lighter, you can also exclude dependencies you don't want to ship, eg:

```xml
<plugin>
  <groupId>io.gatling.frontline</groupId>
  <artifactId>frontline-maven-plugin</artifactId>
  <version>{{< var frontLineMavenPluginVersion >}}</version>
  <executions>
    <execution>
      <goals>
        <goal>package</goal>
      </goals>
      <configuration>
        <excludes>
          <exclude>
            <groupId>org.scalatest</groupId>
            <artifactId>scalatest_{{< var scalaMajorVersion >}}</artifactId>
          </exclude>
        </excludes>
      </configuration>
    </execution>
  </executions>
</plugin>
```

{{< /alert >}}

### SBT Project

In your SBT project configuration, you must add:

- the Gatling dependencies
- the Gatling Enterprise SBT plugin (`"io.gatling.frontline" % "sbt-frontline"`), which packages your code into a deployable artifact

{{< alert warning >}}
We only support sbt 1+, not sbt 0.13.
{{< /alert >}}

The `build.sbt` file should contain this:

```scala
enablePlugins(GatlingPlugin, FrontLinePlugin)

// If you want to package simulations from the 'it' scope instead. You will need the gatling-sbt plugin, see further below.
// inConfig(IntegrationTest)(_root_.io.gatling.frontline.sbt.FrontLinePlugin.frontlineSettings(IntegrationTest))

scalaVersion := "{{< var scalaVersion >}}"
scalacOptions := Seq(
  "-encoding", "UTF-8", "-target:jvm-1.8", "-deprecation",
  "-feature", "-unchecked", "-language:implicitConversions", "-language:postfixOps")

val gatlingVersion = "{{< var gatlingVersion >}}"

libraryDependencies += "io.gatling.highcharts" % "gatling-charts-highcharts" % gatlingVersion % "test"
// only required if you intend to use the gatling-sbt plugin:
libraryDependencies += "io.gatling"            % "gatling-test-framework"    % gatlingVersion % "test"
```

{{< alert tip >}}
The `gatling-test-framework` dependency is only needed if you intend to run locally and use the `gatling-sbt` plugin.
{{< /alert >}}

You will also need the following lines in the `project/plugins.sbt` file:

```scala
// only if you intend to use the gatling-sbt plugin for running Gatling locally:
addSbtPlugin("io.gatling" % "gatling-sbt" % "{{< var gatlingSbtPluginVersion >}}")
// so that sbt can build a package for Gatling Enterprise:
addSbtPlugin("io.gatling.frontline" % "sbt-frontline" % "{{< var frontLineSbtPluginVersion >}}")
```

{{< alert tip >}}
The `gatling-sbt` plugin is optional.
{{< /alert >}}

To package your code, please run one of the following commands in your terminal.

In general (typically, your Gatling simulations are written inside the `src/test/scala` directory):

```console
sbt test:assembly
```

If you are using the integration test (`it`) configuration provided by the `gatling-sbt` plugin (typically, your Gatling simulations are written inside the `src/it/scala` directory):

```console
sbt it:assembly
```

Either command will generate the `target/<artifactId>-<version>.jar` file, which you can then upload in the [Artifacts section]({{< ref "../artifact_conf" >}}).

{{< alert warning >}}
If you use very long method calls chains in your Gatling code, you might have to increase sbt's thread stack size before you can run the `assembly` command:
```bash
export SBT_OPTS="-Xss100M"
```
{{< /alert >}}

### Gradle Project

In your `build.gradle` file, you must add:

- the Gatling dependencies
- the Gatling Enterprise Gradle plugin (`io.gatling.frontline.gradle`), which packages your code into a deployable artifact

The `build.gradle` file should contain this:

```groovy
plugins {
  // The following line allows to load io.gatling.gradle plugin and directly apply it
  id 'io.gatling.frontline.gradle' version '{{< var frontLineGradlePluginVersion >}}'
}

// This is needed to let io.gatling.gradle plugin to loads gatling as a dependency
repositories {
  jcenter()
  mavenCentral()
}

gatling {
  toolVersion = '{{< var gatlingVersion >}}'
}
```

Run the `gradle frontLineJar` command. It will generate the `build/libs/artifactId.jar` file.
You can then upload this file in the [Artifacts section]({{< ref "../artifact_conf" >}}).

### Multi-Module Support

If you have a multi-module project, make sure to only configure the modules which contain Gatling Simulations with the Gatling related plugins described above.
Your Gatling modules can, however, depend on other modules.

## Note on Feeders

A typical mistake with Gatling and Gatling Enterprise is to rely on having an exploded Maven/Gradle/SBT project structure, and to try to load files from the project filesystem.

This filesystem structure will be gone once your project has been packaged and deployed to Gatling Enterprise.

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
