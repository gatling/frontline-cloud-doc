---
title: "Installation"
description: "How to install a Gatling Control Plane on AWS using Elastic Container Service (ECS) and Fargate, to set up your Private Locations and run load generators in your own AWS network"
lead: "Run a Control Plane on AWS using Elastic Container Service (ECS) and Fargate, to set up your Private Locations and run load generators in your own AWS network"
date: 2021-11-15T16:00:00+00:00
lastmod: 2021-11-15T16:00:00+00:00
weight: 220310
---

AWS [Elastic Container Service (ECS)](https://aws.amazon.com/ecs/) is a managed container orchestration service available on AWS. In this example:

- we use **Amazon ECS** to configure a service to run the Gatling Control Plane
- ECS runs our Docker containers on the **AWS Fargate** infrastructure
- the [Control Plane configuration]({{< ref "../configuration" >}}) file is loaded from an **AWS S3 bucket**

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

You can then upload your [Control Plane configuration]({{< ref "../configuration" >}}) file to the bucket.

## IAM role

We need an IAM role which will allow an ECS task to:

- download the Control Plane's configuration file stored in an S3 bucket
- spawn new load generators on EC2 when running a simulation
- when IAM profile configured, allow the Control Plane to pass role 

### Policies

In the AWS management console, from the Services menu, open IAM (or search for "IAM" in the search bar). 
Click on Access management > Policies, and then on Create policy.

`GatlingControlPlaneConfSidecarPolicy` allows sidecar to download [Control Plane configuration]({{< ref "../configuration" >}}) from S3.
```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "s3:GetObject"
            ],
            "Resource": "arn:aws:s3:::{BucketName}/{ObjectName}"
        }
    ]
}
```

{{< alert info >}}
Replace `{BucketName}` with the bucket where you uploaded the [Control Plane configuration]({{< ref "../configuration" >}}) and `{ObjectName}` with the name of the entry in the bucket.
{{< /alert >}}

`GatlingControlPlaneEc2Policy` allows the Control Plane to deploy a load generator on EC2.
```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Action": [
                "ec2:Describe*",
                "ec2:CreateTags",
                "ec2:RunInstances",
                "ec2:TerminateInstances",
                "ec2:AssociateAddress",
                "ec2:DisassociateAddress"
            ],
            "Effect": "Allow",
            "Resource": "*"
        }
    ]
}
```

{{< alert tip >}}
Next policy is only required when you configured some iam-instance-profile on AWS private location in [Control Plane configuration]({{< ref "../configuration" >}}).

IAM Instance Profile on AWS private location allow to assign that role to all load generator instances spawned for that private location
{{< /alert >}}

`GatlingControlPlaneIAMPolicy` allows the Control Plane to pass an IAM instance profile role to a deployed a load generator on EC2.
```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Action": [
                "iam:PassRole"
            ],
            "Effect": "Allow",
            "Resource": [
                "arn:aws:iam::{Account}:role/{RoleNameWithPath}"
            ]
        }
    ]
}
```
{{< alert info >}}
The resources are the Amazon Resource Names (ARN) of the IAM instance-profile roles you configured on some of your private locations.
{{< /alert >}}

### Role 

Create a role `GatlingControlPlaneRole` that will be the Task IAM role: the permissions granted in the IAM role are assumed by the containers running in the task.
{{< img src="ecs-iam-task-role.png" alt="IAM task role with policies" >}}

## Amazon Elastic Container Service (ECS)

### Tasks

Go to **Amazon Elastic Container Service**, under Task definitions, click on **Create a new task definition > Create new task definition with JSON**.

Use the following JSON for the Amazon ECS Task, but first replace the following values:
* Replace `{BucketName}` with the bucket where you uploaded the Control Plane Configuration, and `{ObjectName}` with the name of the entry in the bucket.
* Replace `{GatlingControlPlaneRole ARN}`  with the ARN of the previously created GatlingControlPlaneRole . (ARN can be copied to the clipboard from the role page, IAM > Roles > GatlingControlPlaneRole )
* Replace `{ecsTaskExecutionRole}`  with the ARN of the AWS default role ecsTaskExecutionRole. (ARN can be copied to clipboard from the role page, IAM > Roles > ecsTaskExecutionRole)

```json
{
    "family": "gatling-control-plane-task",
    "containerDefinitions": [
        {
            "name": "conf-loader-side-car",
            "image": "amazon/aws-cli",
            "cpu": 0,
            "portMappings": [],
            "essential": false,
            "entryPoint": [
                "aws",
                "s3",
                "cp",
                "s3://{BucketName}/{ObjectName}",
                "/app/conf/control-plane.conf"
            ],
            "environment": [],
            "mountPoints": [
                {
                    "sourceVolume": "control-plane-conf",
                    "containerPath": "/app/conf",
                    "readOnly": false
                }
            ],
            "volumesFrom": [],
            "readonlyRootFilesystem": false
        },
        {
            "name": "control-plane",
            "image": "gatlingcorp/control-plane:latest",
            "cpu": 0,
            "portMappings": [],
            "essential": true,
            "environment": [],
            "mountPoints": [
                {
                    "sourceVolume": "control-plane-conf",
                    "containerPath": "/app/conf",
                    "readOnly": true
                }
            ],
            "volumesFrom": [],
            "dependsOn": [
                {
                    "containerName": "conf-loader-side-car",
                    "condition": "SUCCESS"
                }
            ],
            "workingDirectory": "/app/conf"
        }
    ],
    "taskRoleArn": "{GatlingControlPlaneRole ARN}",
    "executionRoleArn": "{ecsTaskExecutionRole}",
    "networkMode": "awsvpc",
    "volumes": [
        {
            "name": "control-plane-conf",
            "host": {}
        }
    ],
    "requiresCompatibilities": [
        "FARGATE"
    ],
    "cpu": "1024",
    "memory": "3072"
}
```

### Cluster and service

Create a cluster named `gatling-control-plane-cluster`.

{{< img src="ecs-cluster.png" alt="Cluster creation" >}}

In that cluster, create a service named `gatling-control-plane-service`.

Keep the configuration as default, and update deployment configuration with:
* **Family:** `gatling-control-plane-task`
* **Revision:** Latest (and only) one

{{< img src="ecs-service.png" alt="Service creation" >}}

## Your Control Plane is up and running!

If you kept the default logging configuration, the control plane's logs are sent to Amazon CloudWatch, in a log group named `/ecs/<task definition name>`.

After a short time, you should see your Control Plane get the "up" status in Gatling Enterprise Cloud.

{{< img src="ecs-control-plane-status.png" alt="Checking out the Control Plane's status in Gatling Enterprise Cloud" >}}

You can now configure a simulation to run on one or more of this Control Plane's locations!
