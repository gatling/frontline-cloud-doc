---
title: "Kubernetes Load Generators"
description: "Load Generators on your private Kubernetes cluster"
lead: "Private Locations on your Kubernetes cluster"
date: 2023-01-12T16:46:04+00:00
lastmod: 2023-04-03T12:00:00+00:00
weight: 22055
---

## Kubernetes
To use Kubernetes private locations, the control plane must have access to your Kubernetes cluster.

If the control plane is launched from outside the cluster, you have to give access to a valid Kubernetes file. See [Organizing Cluster Access Using kubeconfig Files](https://kubernetes.io/docs/concepts/configuration/organize-cluster-access-kubeconfig/).
The `.kube` folder can be mounted in `/app`, and absolute path must be set in a `KUBECONFIG` environment variable (e.g. `KUBECONFIG=/app/.kube/config`)

If the control plane is launched from inside the cluster, please refer to our [Kubernetes Control plane deployment documentation]({{< ref "../../installation/kubernetes" >}})

{{< alert tip >}}
When connecting to the cluster using HTTPS, if a custom truststore and/or keystore is needed, `KUBERNETES_TRUSTSTORE_FILE`,
 `KUBERNETES_TRUSTSTORE_PASSPHRASE` and/or `KUBERNETES_KEYSTORE_FILE`, `KUBERNETES_KEYSTORE_PASSPHRASE` environment variables should be set.
{{< /alert >}}

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
      description = "Private Location on Kubernetes"
      # Private location type
      type = "kubernetes"
      # Namespace (optional, default based on kubernetes configuration)
      namespace = "gatling"
      # Certified image configuration
      # They are hosted on Docker Hub, and available for the linux/amd64 and linux/arm64 platforms
      image {
        type = certified
        java = latest # Possible values : 8, 11, 17 or latest
      }
      # Custom image configuration
      # You can build your own images from https://github.com/gatling/frontline-injector-docker-image
      # image {
      #   type = custom
      #   image = "gatlingcorp/classic-openjdk:latest"
      # }
      # Clean up finished jobs resources after given time (optional)
      ttl-after-finished = 10 minutes
      # Service account used for load generator pods (optional)
      service-account-name = "myServiceAccount"
      # Labels of initiated resources (optional)
      labels {
        # ExampleKey = ExampleValue
      }
      # Annotations of initiated resources (optional)
      annotations {
        # ExampleKey = ExampleValue
      }
      # Environment variables of initiated pods (optional)
      environment-variables {
        # ExampleKey = ExampleValue
      }
      # Resources configuration for created pods (optional)
      # We recommend to set both requests and limits to the same values.
      # https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/#resource-requests-and-limits-of-pod-and-container
      resources {
        limits {
          # memory = "512Mi"
          # cpu = "2.0"
        }
        requests {
          # memory = "512Mi"
          # cpu = "2.0"
        }
      }
      # Tolerations (optional)
      tolerations = [
        {
          key = key1
          operator = Equal
          # Value is not needed when effect is Exists (optional)
          value = value1 
          # An empty effect matches all effects with key (optional)
          effect = NoSchedule
        }
      ]
      
      # Java configuration (following configuration properties are optional)
      # System properties (optional)
      system-properties {
        "java.net.preferIPv6Addresses" = "true"
      }
      # Overwrite JAVA_HOME definition (optional)
      java-home = "/usr/lib/jvm/zulu17"
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
      # Based on your instance configuration, you may want to update Xmx and Xms values.
      jvm-options = ["-Xmx8G", "-Xms512M"]
    }
  ]
}
```

### Custom image requirements

Kubernetes private locations image rely on some dependencies.

So when using a custom image, make sure following are available:

- [jq](https://jqlang.github.io/jq/download/) a lightweight and flexible command-line JSON processor.
- [curl](https://curl.se/download.html) a command line tool and library for transferring data with URLs
- [Java runtime environment](https://openjdk.org/install/): OpenJDK 64bits LTS versions: 8, 11 and 17 (see [Gatling prerequisites](https://gatling.io/docs/gatling/tutorials/installation/#java-version))

{{< alert tip >}}
Learn how to tune the OS for more performance, configure the open files limit, the kernel and the network [here](https://gatling.io/docs/gatling/reference/current/core/operations/).
{{< /alert >}}
