---
title: "Gitlab CI/CD"
description: "Learn how to configure Gitlab CI/CD to run your simulations on Gatling Enterprise."
lead: "Run your Gatling Enterprise simulations from Gatling CI."
date: 2023-02-17T14:00:00+00:00
lastmod: 2023-02-17T14:00:00+00:00
weight: 23020
---

We do not yet offer a dedicated Docker image for Gitlab CI/CD, but you can easily run your simulations on Gatling Enterprise from GitLab CI/CD, using either one of the supported build tools or our CI shell script.

This will not create a new Gatling Enterprise simulation, you have to create it using the Gatling Enterprise Dashboard before, or do it using the options provided by our build tools plugins:
- [Maven](https://gatling.io/docs/gatling/reference/current/extensions/maven_plugin/#working-with-gatling-enterprise-cloud)
- [Gradle](https://gatling.io/docs/gatling/reference/current/extensions/gradle_plugin/#working-with-gatling-enterprise-cloud)
- [SBT](https://gatling.io/docs/gatling/reference/current/extensions/sbt_plugin/#working-with-gatling-enterprise-cloud)

## Pre-requisites

You must first [create an API token]({{< ref "../../admin/api_tokens" >}}). It will be used to authenticate with Gatling Enterprise. You can store the API Token in a [Gitlab CI Variable](https://docs.gitlab.com/ee/ci/variables/#define-a-cicd-variable-in-the-ui) (make sure to check "Mask variable") with the name `GATLING_ENTERPRISE_API_TOKEN`. Or if you [use a vault to store secrets](https://docs.gitlab.com/ee/ci/secrets/), store the API Token in your vault and retrieve its value to an environment variable named `GATLING_ENTERPRISE_API_TOKEN` in your Gitlab CI/CD configuration file.

In the following examples, we assume the API Token is available in an environment variable named `GATLING_ENTERPRISE_API_TOKEN`, which our tools will detect automatically.

We also assume that you have already [configured a simulation]({{< ref "../../user/simulations" >}}) on Gatling Enterprise. You can copy the simulation ID from the simulations list view. In the following examples, we will show the simulation ID as `00000000-0000-0000-0000-000000000000`.

## Using a build tool plugin

You can build you Simulation, and then run the updated Simulation on Gatling Enterprise, using the `enterpriseRun` command with any of our supported build tools.

With the `waitForRunEnd=true` option, it will display live metrics until the end of the run, and exit with an error code if the run fails on Gatling Enterprise (e.g. if the run crashes or if the assertions fail).

Configure your CI build to run the command corresponding to the build tool you use. Here are some examples:

{{< include-file >}}
Maven: includes/run-with-build-tool.maven.md
Gradle: includes/run-with-build-tool.gradle.md
Gradle Wrapper: includes/run-with-build-tool.gradlew.md
Sbt: includes/run-with-build-tool.sbt.md
{{< /include-file  >}}

## Using a shell script

This script launches an existing simulation on Gatling Enterprise and displays live metrics.

It can be [downloaded here](https://downloads.gatling.io/releases/frontline-ci-script/{{< var ciPluginsVersion >}}/frontline-ci-script-{{< var ciPluginsVersion >}}.zip).

### Shell script requirements

This script runs with:
- the `bash` shell
- the `curl` HTTP client, [see here for more information](https://curl.se/)
- the `jq` JSON processor, [see here for more information](https://stedolan.github.io/jq/)

These tools must be installed on the machine or container where your CI system will execute the script.

### Shell script usage

You need to give 3 parameters to the script:

- Gatling Enterprise url: `https://cloud.gatling.io`.
- API token: the [API token]({{< ref "../../admin/api_tokens" >}}) will allow the script to authenticate to Gatling Enterprise. The API token needs the Start permission.
- Simulation ID: the ID of the simulation you want to start. You can get this ID on the [Simulations table]({{< ref "../../user/simulations" >}}), with the {{< icon clipboard >}} icon.

Configure your CI build to call the script, for example::

```yaml
image: ubuntu:latest

stages:
  - run-gatling-enterprise

run-gatling-enterprise:
  stage: run-gatling-enterprise
  script:
    # Assuming the script is present at the root of the repository
    - ./start_simulation.sh 'https://cloud.gatling.io' "$GATLING_ENTERPRISE_API_TOKEN" '00000000-0000-0000-0000-000000000000'
```
