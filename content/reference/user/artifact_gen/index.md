---
title: "Artifact Generation"
description: "Learn how to generate an artifact for FrontLine from Gatling zip bundle or a maven, sbt or gradle project."
lead: "Generate an artifact from your Gatling bundle or a maven, sbt or gradle project."
date: 2021-03-08T13:50:32+01:00
lastmod: 2021-03-08T13:50:32+01:00
weight: 10040
---

## Generating Artifacts for FrontLine

FrontLine deploys packages containing your compiled Simulations and resources.
Those packages have to be generated upstream.
FrontLine is compatible with Gatling 3.3, 3.4 and 3.5.

### Gatling zip bundle

Please copy the [`artifact.sh`](https://raw.githubusercontent.com/gatling/gatling/master/gatling-bundle/src/universal/bin/artifact.sh) or [`artifact.bat`](https://raw.githubusercontent.com/gatling/gatling/master/gatling-bundle/src/universal/bin/artifact.bat) files in the `bin` directory of your Gatling unzipped bundle.
Those files will be shipped in the bundle in a future official Gatling release.

Please run the script matching your operating system and generate the `target/artifact.jar` file.
You'll have to upload this file in the new [Artifacts section](/docs/user/artifacts_conf).

### Maven Project

In your `pom.xml`, you have to add:

- the Gatling dependencies
- the maven plugin for Scala, so your code gets compiled
- the maven plugin for FrontLine, so it can package your code into a deployable artifact

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

Please run the `mvn clean package -DskipTests` command  in your terminal and generate the `target/<artifactId>-<version>-shaded.jar` file.
You'll have to upload this file in the new [Artifacts section](/docs/user/artifacts_conf).

{{< alert tip >}}
You can also exclude dependencies you don't want to ship and make the artifact lighter, eg:

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

In a sbt project, you have to add:

- the Gatling dependencies
- the sbt plugin for FrontLine, so that it can package your code into a deployable artifact

{{< alert warning >}}
We only support sbt 1+, not sbt 0.13.
{{< /alert >}}

{{< alert warning >}}
We recommend disabling Coursier for now. There are several bugs in the sbt/Coursier integration that make our plugin work in a suboptimal fashion.
{{< /alert >}}

Your `build.sbt` file should look like this:

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
// only required if you intend to use the gatling-sbt plugin
libraryDependencies += "io.gatling"            % "gatling-test-framework"    % gatlingVersion % "test"
```

{{< alert tip >}}
The `gatling-test-framework` dependency is only needed if you intend to run locally and use the `gatling-sbt` plugin.
{{< /alert >}}

You will also need the following lines in the `project/plugins.sbt` file:

```scala
// only if you intend to use the gatling-sbt plugin for running Gatling locally
addSbtPlugin("io.gatling" % "gatling-sbt" % "{{< var gatlingSbtPluginVersion >}}")
// so sbt can build a package for FrontLine
addSbtPlugin("io.gatling.frontline" % "sbt-frontline" % "{{< var frontLineSbtPluginVersion >}}")
```

{{< alert tip >}}
The `gatling-sbt` plugin is optional.
{{< /alert >}}

To package your code, please run one of the following commands in your terminal.

In general (typically, your Gatling simulations are written inside the `src/test/scala` directory):

```bash
$ sbt test:assembly
```

If you are using the integration test (`it`) configuration provided by the `gatling-sbt` plugin (typically, your Gatling simulations are written inside the `src/it/scala` directory):

```bash
$ sbt it:assembly
```

Either command will generate the `target/<artifactId>-<version>.jar` file, which you will then have to upload in the new [Artifacts section](/docs/user/artifacts_conf).

{{< alert warning >}}
If you use very long method calls chains in your Gatling code, you might have to increase sbt's thread stack size before you can run the `assembly` command:
```bash
$ export SBT_OPTS="-Xss100M"
```
{{< /alert >}}

### Gradle Project

In a Gradle project, you have to:

- pull Gatling dependencies
- add the gradle plugin for FrontLine, so it can package your code into a deployable artifact

A `build.gradle` file should look like this:

.build.gradle:
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

Please run the `gradle frontLineJar` command in your terminal and generate the `build/libs/artifactId.jar` file.
You'll have to upload this file in the new [Artifacts section](/docs/user/artifacts_conf).

### Multi-Module Support

If your project is a multi-module one, make sure that only the one containing the Gatling Simulations gets configured with the Gatling related plugins describes above.
FrontLine will take care of deploying all available jars so you can have Gatling module depend on the other ones.

## Note on Feeders

A typical mistake with Gatling and FrontLine is to rely on having an exploded maven/gradle/sbt project structure and try loading files from the project filesystem.

This filesystem structure will be gone once FrontLine will have compiled your project and uploaded your binaries on the injectors.

If your feeder files are packaged with your test sources, you must resolve them from the classpath.
This way will always work, both locally and with FrontLine.

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

Assuming a CSV file contains 1000 entries, and 3 Gatling nodes, the entries will be distributed the following way:

- First node will access the first 333 entries
- Second node will access the next 333 entries
- Third node will access the last 334 entries

{{< alert tip >}}
`shard` is available in Gatling OSS DSL but is a noop there. It's only effective when running tests with FrontLine.
{{< /alert >}}

## Resolving Injector Location in Simulation

When running a distributed test from multiple locations, you could be interested in knowing where a given injector is deployed in order to trigger specific behaviors depending on location.

For example, you might want to hit `https://mydomain.co.uk` `baseUrl` if injector is deployed on AWS London, and `https://mydomain.com` otherwise.

You can resolve in your simulation code the name of the pool a given injector is deployed on:

```scala
val poolName = System.getProperty("gatling.frontline.poolName")
val baseUrl = if (poolName == "London") "https://domain.co.uk" else "https://domain.com"
```

{{< alert tip >}}
This System property is only defined when deploying with FrontLine.
It's not defined when running locally with any Gatling OSS launcher.
{{< /alert >}}
