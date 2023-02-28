---
title: "ECS and Fargate"
description: "How to install a Gatling Control Plane on AWS using Elastic Container Service (ECS) and Fargate, to set up your Private Locations and run load generators in your own AWS network"
lead: "Run a Control Plane on AWS using Elastic Container Service (ECS) and Fargate, to set up your Private Locations and run load generators in your own AWS network"
date: 2021-11-15T16:00:00+00:00
lastmod: 2021-11-15T16:00:00+00:00
weight: 22052
---

AWS [Elastic Container Service (ECS)](https://aws.amazon.com/ecs/) is a managed container orchestration service available on AWS. In this example:

- we use **Amazon ECS** to configure a service to run the Gatling Control Plane
- ECS runs our Docker containers on the **AWS Fargate** infrastructure
- the Control Plane configuration file is loaded from an **AWS S3 bucket**

This is only an example. You could, for instance, use ECS to run containers on Amazon EC2, or mount the configuration file from Amazon EFS.

## S3 bucket

{{< alert info >}}
This section shows how to create a new S3 bucket, skip if you already have a bucket you want to use.
{{< /alert >}}

In the AWS management console, from the Services menu, open S3 (or search for "S3" in the search bar). Click Create bucket.

Choose a name for the bucket, and the region where it will be stored.

Configure other options as preferred. If in doubt, keep "ACLs disabled" and "Block all public access"; we will need to allow access to the bucket with a policy.

Click Create bucket.

{{< img src="ecs-s3-configuration.png" alt="Configuring the S3 bucket" >}}

You can then upload your Control Plane configuration file to the bucket.

## IAM role

We need an IAM role which will allow an ECS task to:

- download the Control Plane's configuration file stored in an S3 bucket
- spawn new load generators on EC2 when running a simulation

In the AWS management console, from the Services menu, open IAM (or search for "IAM" in the search bar). Click on Access management > Roles, and then on Create role.

Choose the "AWS service" entity type, and the "Elastic Container Service Task" use case, then click Next.

{{< img src="ecs-iam-role-1.png" alt="Choosing to create an IAM role for an Elastic Container Service Task use case" >}}

Select the policies:

- `AmazonS3ReadOnlyAccess` (or you can create a custom policy based on this one, to only allow access to the specific S3 bucket where you stored the Control Plane configuration)
- You can use `AmazonEC2FullAccess` for testing, but you will most likely want to create a more restrictive policy with only the privileges needed to spawn load generator instances on EC2:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Action": [
        "ec2:Describe*",
        "ec2:CreateTags",
        "ec2:RunInstances"
      ],
      "Effect": "Allow",
      "Resource": "*"
    }
  ]
}
```

If you have configured an IAM instance profile in the Control plane configuration file (with the optional key `iam-instance-profile`), you also need to add the `Action`s: `"iam:GetInstanceProfile"`, `"iam:ListInstanceProfiles"`, and `"iam:PassRole"`.

Then click Next.

Enter a name for the role. Add a description and tags if you want. Click on Create role.

{{< img src="ecs-iam-role-2.png" alt="Reviewing the IAM role to create" >}}

## ECS cluster

{{< alert info >}}
This section shows how to create a new ECS cluster, skip if you already have a cluster you want to use.
{{< /alert >}}

In the AWS management console, from the Services menu, open Elastic Container Service (or search for "ECS" in the search bar). Click on Create Cluster. To run your containers on Fargate, choose a Networking Only cluster.

{{< img src="ecs-cluster-template.png" alt="Choosing a Networking Only cluster template" >}}

Then enter a cluster name (and any custom tags you may want) and click on Create.

{{< img src="ecs-cluster-configuration.png" alt="Configuring the cluster name and tags" >}}

## ECS Task definition

{{< alert info >}}
Task definitions are versioned. When creating a new task definition, you create its revision 1. When you want to modify it, you need to create a new revision.
{{< /alert >}}

In the AWS management console, from the Services menu, open Elastic Container Service (or search for "ECS" in the search bar). Click on Amazon ECS > Task Definitions, then on Create new Task Definition.

Select the "Fargate" launch type compatibility and click on Next step.

{{< img src="ecs-task-definition-type.png" alt="Choosing the Fargate launch type compatibility" >}}

Configure:

- Task definition name: a name of your choice for this task definition
- Task role: the IAM role we created earlier
- Operating system family: Linux
- Task execution role: the ecsTaskExecutionRole automatically created by AWS (or an equivalent custom role)
- Task size: the minimum values (0.5 GB of RAM and 0.25 vCPU) should be enough

{{< img src="ecs-task-definition-configuration-1.png" alt="Configuring the task definition's main parameters" >}}

### Shared volume

In the Volumes section, add a volume with a name of your choice and the "Bind mount" volume type.

{{< img src="ecs-task-definition-configuration-2.png" alt="Configuring the task definition's volume" >}}

### Side-car container (configuration loader)

In the Container definitions section, click on Add a container.

Configure:

- Container name: a name of your choice
- Image: `amazon/aws-cli`

{{< img src="ecs-task-definition-containers-side-car-1.png" alt="Configuring the side car container's main parameters" >}}

In the Environment section:

- Essential : no
- Entry point: `aws,s3,cp,s3://<bucket name>/<file name>.conf,/app/conf/control-plane.conf` (replace the origin bucket name and file name with the values you used; the destination file *must* be `/app/conf/control-plane.conf`)
- Working directory: `/app/conf`

{{< img src="ecs-task-definition-containers-side-car-2.png" alt="Configuring the side car container's environment parameters" >}}

In the Storage and logging section, select the Mount point's Source volume (choose the volume created earlier) and enter its Container path: `/app/conf`.

{{< img src="ecs-task-definition-containers-side-car-3.png" alt="Configuring the side car container's volume mount point" >}}

Click Add to add this container to the task definition.

### Control plane container

In the Container definitions section, click on Add a container.

Configure:

- Container name: a name of your choice
- Image: `gatlingcorp/control-plane:latest`

{{< img src="ecs-task-definition-containers-cp-1.png" alt="Configuring the control plane container's main parameters" >}}

In the Startup dependency ordering section, add a dependency:

- Container name: the name you chose for the side-car container
- Condition: COMPLETE

{{< img src="ecs-task-definition-containers-cp-2.png" alt="Configuring the control plane container's startup dependency" >}}

In the Storage and logging section, select the Mount point's Source volume (choose the volume created earlier) and enter its Container path: `/app/conf`. You can set it to read only too if desired.

{{< img src="ecs-task-definition-containers-cp-3.png" alt="Configuring the control plane container's volume mount point" >}}

Click Add to add this container to the task definition, the click Create to create the task definition.

## ECS Service

In the AWS management console, from the Services menu, open Elastic Container Service (or search for "ECS" in the search bar). Click on Amazon ECS > Clusters, then on the name of your cluster. In the Services tab, click on Create.

Configure:

- Launch type: FARGATE
- Operating system family: Linux
- Task definition: choose the task definition you created earlier
- Service name: a name of your choice for this service
- Number of tasks: 1 (to run a single instance of the control plane container)

You can modify other settings if needed for your use case, but the defaults will work. Then click Next step.

{{< img src="ecs-service-1.png" alt="Configuring the service's main parameters (part 1)" >}}
{{< img src="ecs-service-2.png" alt="Configuring the service's main parameters (part 2)" >}}

Choose the VPC, subnet(s) and security group to use for your control plane container. They **must** allow outbound connections to `api.gatling.io` on port `443`, so that the control plane can retrieve tasks to execute. The other parameters are not relevant to this type of task definition. Click Next step.

{{< img src="ecs-service-3.png" alt="Configuring the service's network parameters" >}}

You do not need to enable auto scaling for this. Click Next step.

{{< img src="ecs-service-4.png" alt="Configuring the service's auto scaling parameters" >}}

Review your service configuration and click Create service.

The service will automatically start; you should see it in your ECS cluster's Services tab. If you click on the service and check its Tasks tab, you should see the task's status change as it starts. It will show the status "RUNNING" once the Control Plane container has started.

## Your Control Plane is up and running!

If you kept the default logging configuration, the control plane's logs are sent to Amazon CloudWatch, in a log group named `/ecs/<task definition name>`.

After a short time, you should see your Control Plane get the "up" status in Gatling Enterprise Cloud.

{{< img src="ecs-control-plane-status.png" alt="Checking out the Control Plane's status in Gatling Enterprise Cloud" >}}

You can now configure a simulation to run one one or more of this Control Plane's pool!
