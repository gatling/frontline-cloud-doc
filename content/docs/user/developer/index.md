---
title: "Developer"
description: "Learn how to configure your projects and package it with Maven, SBT and Gradle."
lead: "Configure and package your projects with Maven, SBT and Gradle."
date: 2021-03-08T13:50:32+01:00
lastmod: 2021-03-08T13:50:32+01:00
draft: false
images: []
menu:
  docs:
    parent: "user"
weight: 040
---

## FrontLine Gatling Versions

FrontLine actually uses custom versions of the Gatling components. Those binaries are not open sources and their usage is restricted to FrontLine.
When you'll be deploying tests with FrontLine, it will replace your Gatling OSS dependencies with their custom counterparts.

## Configuring Gatling Projects

### Maven

In your `pom.xml`, you have to add in:

- pull Gatling dependencies
- add the maven plugin for Scala, so your code gets compiled
- add the maven plugin for FrontLine, so it can package your code into a deployable artifact

```xml
<dependencies>
  <dependency>
    <groupId>io.gatling.highcharts</groupId>
    <artifactId>gatling-charts-highcharts</artifactId>
    <version>{gatlingVersion}</version>
    <scope>test</scope>
  </dependency>
</dependencies>

<build>
  <!-- so maven compiles src/test/scala and not only src/test/java -->
  <testSourceDirectory>src/test/scala</testSourceDirectory>
  <plugins>
    <plugin>
      <artifactId>maven-jar-plugin</artifactId>
      <version>{mavenJarPluginVersion}</version>
    </plugin>
    <!-- so maven can compile your scala code -->
    <plugin>
      <groupId>net.alchim31.maven</groupId>
      <artifactId>scala-maven-plugin</artifactId>
      <version>{scalaMavenPluginVesrion)</version>
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
      <version>{frontLineMavenPluginVersion}</version>
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

You can run `mvn package -DskipTests` in your terminal and check you get a jar containing all the dependencies of the simulation.

You can also exclude dependencies you don't want to ship, eg:

.pom.xml:
```xml
<plugin>
  <groupId>io.gatling.frontline</groupId>
  <artifactId>frontline-maven-plugin</artifactId>
  <version>{frontLineMavenPluginVersion}</version>
  <executions>
    <execution>
      <goals>
        <goal>package</goal>
      </goals>
      <configuration>
        <excludes>
          <exclude>
            <groupId>org.scalatest</groupId>
            <artifactId>scalatest_{scalaMajorVersion}</artifactId>
          </exclude>
        </excludes>
      </configuration>
    </execution>
  </executions>
</plugin>
```

### SBT

In a sbt project, you have to:

- pull Gatling dependencies
- add the sbt plugin for FrontLine, so it can package your code into a deployable artifact

A `build.sbt` file should look like this:

```scala
enablePlugins(GatlingPlugin, FrontLinePlugin)

// If you want to package simulations from the 'it' scope instead
// inConfig(IntegrationTest)(_root_.io.gatling.frontline.sbt.FrontLinePlugin.frontlineSettings(IntegrationTest))

scalaVersion := "{scalaVersion}"
scalacOptions := Seq(
  "-encoding", "UTF-8", "-target:jvm-1.8", "-deprecation",
  "-feature", "-unchecked", "-language:implicitConversions", "-language:postfixOps")

val gatlingVersion = "{gatlingVersion}"

libraryDependencies += "io.gatling.highcharts" % "gatling-charts-highcharts" % gatlingVersion % "test"
// only required if you intend to use the gatling-sbt plugin
libraryDependencies += "io.gatling"            % "gatling-test-framework"    % gatlingVersion % "test"
```

{{< alert warning >}}
We only support sbt 1+, not sbt 0.13.
{{< /alert >}}

{{< alert warning >}}
If you use the 'it' config, you have to use a custom build command as the defauit one is for the 'test' config: `sbt -J-Xss100M ;clean;it:assembly -batch --error`
{{< /alert >}}

{{< alert warning >}}
We recommend disabling Coursier for now. There are several bugs in the sbt/Coursier integration that makes our plugin work in a suboptimal fashion.
{{< /alert >}}

{{< alert tip >}}
The `gatling-test-framework`dependencies is only needed if you intend to run locally and use the gatling-sbt plugin.
{{< /alert >}}

{{< alert tip >}}
If you use very long method calls chains in your Gatling code, you might have to increase sbt's thread stack size:
{{< /alert >}}

```bash
$ export SBT_OPTS="-Xss100M"
```

You will also need the following lines in the `project/plugins.sbt` file:

.project/plugins.sbt:

```scala
// only if you intend to use the gatling-sbt plugin for running Gatling locally
addSbtPlugin("io.gatling" % "gatling-sbt" % "{gatlingSbtPluginVersion}")
// so sbt can build a package for FrontLine
addSbtPlugin("io.gatling.frontline" % "sbt-frontline" % "{frontLineSbtPluginVersion}")
```

You can run `sbt test:assembly` (or `sbt it:assembly` if you've configured the plugin for integration tests) in your terminal and check you get a jar containing all the dependencies of the simulation.

{{< alert ip >}}
The `gatling-sbt` is optional.
{{< /alert >}}

### Gradle

In a Gradle project, you have to:

- pull Gatling dependencies
- add the gradle plugin for FrontLine, so it can package your code into a deployable artifact

A `build.gradle` file should look like this:

.build.gradle:
```groovy
plugins {
    // The following line allows to load io.gatling.gradle plugin and directly apply it
    id 'io.gatling.frontline.gradle' version '{frontLineGradlePluginVersion}'
}

// This is needed to let io.gatling.gradle plugin to loads gatling as a dependency
repositories {
    jcenter()
    mavenCentral()
}

gatling {
    toolVersion = '{gatlingVersion}'
}
```

You can run `gradle frontLineJar` in your terminal and check you get a jar containing all the dependencies of the simulation.

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

## Specific Gatling Features

### Load Sharding

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
