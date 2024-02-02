---
title: "Gatling as Code with Maven"
description: "Setup tutorial"
lead: "Set up your project for automatic configuration of your simulation in Gatling Enterprise."
date: 2024-01-31T18:30:00+02:00
lastmod: 2024-02-01T11:00:00+00:00
weight: 2090700
private: true
---

{{<alert tip>}}
If you need a package to test this, you can use the following one: [`gatling-maven-plugin-demo-java`](https://github.com/gatling/gatling-maven-plugin-demo-java)
{{</alert>}}
# Configuration

## 1. Edit your `pom.xml` file

 * In `project.properties`, make sure you use the following version:
   ```xml
   <gatling-maven-plugin.version>4.8.0</gatling-maven-plugin.version>
   ```


## 2. Create your Gatling configuration

Add a new directory at the root of your project: `.gatling` (sibling of your `pom.xml` file).

Create a new file `.gatling/package.conf`.

```console
.
├── .gatling/
│   └── package.conf
├── src/
│   ├── main/
│   └── test/
└── pom.xml
```

This file is in [`HOCON` format (Human-Optimized Config Object Notation)](https://github.com/lightbend/config/blob/main/HOCON.md), which means you can also write `JSON` if you prefer.

{{<alert tip>}}
Managed location name must be an available region (see list in example file below).

Private location name must be an id configured in your control plane ([see documentation]({{<ref "../../reference/admin/private_locations/introduction/#configuration">}})).
{{</alert>}}

{{<alert warning>}}
Mixing managed and private locations is not supported by Gatling Enterprise. Ensure all your locations are either managed or private for a given simulation.
{{</alert>}}

```bash
gatling.enterprise {
  # The name of the package (Mandatory)
  name = "My package name"
  # The team UUID (Mandatory)
  # You can copy it from Organization / Teams / More / Copy ID
  team = "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX"

  # Simulations based on the package (Optional)
  # List of objects [{...}, {...}, {...}]
  simulations = [{
    # The name of the simulation (Mandatory)
    name = "My simulation name"

    # The FQDN of the class that extends Simulation (Mandatory)
    classname = "com.example.MySimulation"

    ## Using or not dedicated IPs (optional)
    ## Only available if dedicated IPs are configured in your organization
    # useDedicatedIps = false

    # Locations configuration (mandatory)
    # Map of objects "key1" {...}, "key2" {...}, "key3"{...}
    locations {
      # Mixing managed and private locations is not supported.
      # Ensure all your locations are either managed or private for a given simulation.

      # Managed location:
      # Configuration by region
      # Available regions:
      #  - "US West - N. California"
      #  - "Europe - Paris"
      #  - "Europe - Dublin"
      #  - "AP Pacific - Mumbai"
      #  - "US West - Oregon"
      #  - "AP SouthEast - Sydney"
      #  - "US East - N. Virginia"
      #  - "SA East - São Paulo"
      #  - "AP - Tokyo"
      #  - "AP - Hong Kong"

      "Europe - Paris" {
        # Amount of load generators in this region (mandatory)
        size = 1
        ## Weight of this region (optional)
        ## (total sum of all locations MUST be 100 for a simulation)
        # weight = 100
      }

      ## Private location:
      ## The name must be an id configured in your control plane
      
      # "prl_example" {
      #   # Amount of load generators in this location (mandatory)
      #   size = 1
      #   ## Weight of this location (optional)
      #   ## (total sum of all locations MUST be 100 for a simulation)
      #   # weight = 100
      # }
    }

    # Configure specific parameters for this simulation (optional)
    parameters {
      ## Ignore parameters configured at the organization level (optional)
      # ignoreDefaults = false

      ## Java System properties (optional)
      ## key = value
      # systemProperties {
      #   "com.example.prop" = "value frop system prop"
      # }

      ## Environment variables (optional)
      ## key = value
      # environmentVariables {
      #   MY_SIMULATION_ENV_VAR = "value from environment"
      # }
    }

    # Meaningful time window (optional)
    timeWindow {
      ## Time to exclude from assertions at the beginning of the run (optional)
      # rampUp = 0

      ## Time to exclude from assertions at the end of the run (optional)
      # rampDown = 0
    }
  }]
}
```

## 3. Upload your project to Gatling Enterprise

(package configuration, package upload, simulations configuration)

{{<alert tip>}}
Ensure you referenced the API token for your [maven extension](https://docs.gatling.io/gatling/reference/current/extensions/maven_plugin/#api-tokens).

To create an API token: [documentation]({{<ref "../../reference/admin/api_tokens">}})
(must have the **Configure** role).
{{</alert>}}

Use the following command when using Maven:

`mvn gatling:enterpriseUpload`

Or the following if you’re using the provided wrapper:

`./mvnw gatling:enterpriseUpload`

A successful upload results in the following:

```bash
[INFO] Package configuration file detected, applying it.
[INFO] Package id: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
[INFO] Package uploaded
[INFO] Package successfully uploaded
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  x.xxx s
[INFO] Finished at: yyyy-MM-ddThh:mm:ss+01:00
[INFO] ------------------------------------------------------------------------
```

If you encounter any problems, please contact our support team.

# Result

Your package and configured simulation are now uploaded to Gatling Enterprise 👍.

Go to [Gatling Enterprise](https://cloud.gatling.io/) for further use. 
