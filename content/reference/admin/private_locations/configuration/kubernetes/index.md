---
title: "Kubernetes Load Generators"
description: "Load Generators on your private Kubernetes cluster"
lead: "Private Locations on your Kubernetes cluster"
date: 2023-01-12T16:46:04+00:00
lastmod: 2023-01-12T16:29:04+00:00
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
      image {
        type = certified
        java = latest # Possible values : 8, 11, 17 or latest
      }
      # Custom image configuration
      # image {
      #   type = custom
      #   image = "gatlingcorp/classic-openjdk:latest"
      # }
      # Clean up finished jobs resources after given time (optional)
      ttl-after-finished = 10 minutes
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
    }
  ]
}
```
