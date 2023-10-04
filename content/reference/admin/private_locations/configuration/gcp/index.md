---
title: "GCP Load Generators"
description: "Load Generators on your private GCP account"
lead: "Private Locations on your GCP account"
date: 2023-10-02T15:29:00+00:00
lastmod: 2023-10-02T15:29:00+00:00
weight: 22054
draft: true
---

## GCP Virtual Machines
GCP private locations require the control plane to have GCP access rights configured in order to instantiate virtual machines.

### Service account
Access rights can be set through a service account associated with your control plane.

Check GCP and Gatling documentation pages for more details:
* [GCP Service account](https://cloud.google.com/iam/docs/service-account-overview)
* [Gatling installation guide]({{< ref "../../installation/gcp/#service-account" >}})

### Control plane configuration file

```bash
control-plane {
  # Control plane token
  token = "cpt_example_c7oze5djp3u14a5xqjanh..."
  # Control plane token with an environment variable
  token = ${?CONTROL_PLANE_TOKEN}
  # Control plane token with a system property
  token = $?control.plane.token
  # Control plane description (optional)
  description = "my control plane description"
  # Locations configurations
  locations = [
    {
      # Private location ID, must be prefixed by prl_, only consist of numbers 0-9, 
      # lowercase letters a-z, and underscores, with a max length of 30 characters
      id = "prl_private_location_example"
      # Private location description (optional)
      description = "Private Location on GCP"
      # Private location type
      type = "gcp"
      # GCP location name, as listed by GCP CLI:
      # gcloud compute zones list
      zone = "europe-west3-a"
      machine {
        # Virtual machine type, as listed by GCP CLI:
        # gcloud compute machine-types list --filter="zone:( europe-west3-a )"
        type = "e2-micro"
        # Certified image configuration
        image {
          type = "certified"
          java = "latest" # Possible values : 8, 11, 17 or latest
        }
        # Storage configuration
        disk {
          # Disk size in Gb (mininum 20Gb)
          sizeGb = 20
        }
        # Network interface (optional)
        network-interface {
          # Network name on your project (optional)
          # Not needed if subnetwork is configured
          # network = "gatling-network"
          # Subnetwork name on your project (optional)
          # subnetwork = "gatling-subnetwork-europe-west3"
          # Associate external IP to instance (optional, default to true)
          # See Cloud NAT when set to false
          # with-external-ip = true
        }
      }
      # GCP project id as returned by GCP CLI:
      # gcloud projects list
      project = "my-project-id"
      # Java configuration (the following configuration properties are optional)
      # System properties (optional)
      system-properties {
        "java.net.preferIPv6Addresses" = "true"
      }
      # Overwrite JAVA_HOME definition (optional)
      # java-home = "/usr/lib/jvm/zulu"
      # JVM Options (optional)
      # Default ones, that can be overriden with precedence:
      # [
      #   "-Xmx4G", 
      #   "-XX:MaxInlineLevel=20", 
      #   "-XX:MaxTrivialSize=12", 
      #   "-XX:+IgnoreUnrecognizedVMOptions", 
      #   "--add-opens=java.base/java.nio=ALL-UNNAMED", 
      #   "--add-opens=java.base/jdk.internal.misc=ALL-UNNAMED"
      # ]
      #  Based on your instance configuration, you may want to update Xmx and Xms values.
      # jvm-options = ["-Xmx4G", "-Xms512M"]
    }
  ]
}
```
