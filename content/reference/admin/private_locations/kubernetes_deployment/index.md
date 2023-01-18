---
title: "Deploying a Control Plane on Kubernetes"
description: "How to deploy a Gatling Control Plane on Kubernetes, to set up your Private Locations and run load generators in your own Kubernetes cluster"
lead: "Run a Control Plane on Kubernetes, to set up your Private Locations and run load generators in your own Kubernetes network"
weight: 22054
---

## Introduction

A control plane, deployed on a kubernetes cluster with the needed roles will be able to deploy kubernetes private locations.

To better understand what roles are needed, here's a quick overview of the mechanisms involved when deploying a private location on Kubernetes.

First, the control plane regularly polls the Gatling Enterpise API, and it is notified to deploy a number of instances in a given Kubernetes location.

Then, it creates a config map for each namespace of each Kubernetes location requested for this run. 
The config map includes the load generator's start script, so these scripts don't need to be embedded in the Docker images used for the load generators.

Finally, the control plane initiates a batch job, with the number of instances configured for the simulation run, and mounts the appropriate config map on each pod.

### Roles
For each namespace configured for a private location, the control plane needs the following roles:

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: control-plane-role
  namespace: <namespace>
rules:
  - apiGroups: [""]
    resources: ["configmaps"]
    verbs: ["get", "create", "update", "delete"]
  - apiGroups: ["batch"]
    resources: ["jobs"]
    verbs: ["create"]
```

### Example

The control plane requires a configuration under `/app/conf/control-plane.conf`, see [control plane configuration]({{< ref "../configuration#control-plane-configuration-1" >}}).

{{< alert tip >}}
To do so, we'll create a config map named `control-plane-config` with a file named `control-plane.conf`, which will be mounted on the container:
```bash
kubectl create configmap control-plane-config \
        --from-file=control-plane.conf
```
{{< /alert >}}
```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: control-plane-service-account
  namespace: gatling
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: control-plane-role
  namespace: gatling
rules:
  - apiGroups: [""]
    resources: ["configmaps"]
    verbs: ["get", "create", "update", "delete"]
  - apiGroups: ["batch"]
    resources: ["jobs"]
    verbs: ["create"]
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: control-plane-role-binding
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: control-plane-role
subjects:
- kind: ServiceAccount
  name: control-plane-service-account
  namespace: gatling
---
apiVersion: apps/v1 
kind: Deployment 
metadata:
  name: control-plane
spec:
  replicas: 1 
  selector: 
    matchLabels: 
      name: control-plane-template
  template: 
    metadata:
      labels:
        name: control-plane-template
    spec:
      serviceAccountName: control-plane-service-account
      containers:
        - name: control-plane
          image: gatlingcorp/control-plane:latest
          volumeMounts:
            - mountPath: /app/conf/
              name: control-plane-conf-volume
      volumes:
        - name: control-plane-conf-volume
          configMap:
            name: control-plane-config
```
