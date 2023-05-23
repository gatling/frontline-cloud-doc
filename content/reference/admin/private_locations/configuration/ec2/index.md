---
title: "AWS EC2 Load Generators"
description: "Load Generators on your private AWS account"
lead: "Private Locations on your AWS account"
date: 2023-01-12T16:46:04+00:00
lastmod: 2023-01-12T16:29:04+00:00
weight: 22053
---

## AWS EC2
AWS private locations require the control plane to have access to AWS credentials from the default credential provider chain.

See [the AWS documentation for the Default Credential Provider Chain](https://docs.aws.amazon.com/sdk-for-java/v1/developer-guide/credentials.html#credentials-default).

{{< alert warning >}}
AWS EC2 private locations rely on [cloud-init](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/user-data.html) integration.
If you configure a custom image, make sure it supports cloud-init.
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
      description = "Private Location on AWS"
      # Private location type
      type = "aws"
      # Configuration specific to AWS type configuration
      region = "eu-west-1"
      # Certified AMI configuration
      ami {
        type = "certified"
        java = latest # Possible values : 8, 11, 17 or latest
      }
      # Custom AMI configuration (alternative to certified AMI)
      # ami = {
      #   type = custom
      #   id = "ami-00000000000000000"
      # }
      # Security groups
      security-groups = ["sg-mysecuritygroup"]
      # Instance type
      instance-type = "c5.xlarge"
      # VPC
      vpc = "vpc-a"
      # Subnets
      subnets = ["subnet-a", "subnet-b"]
      # Profile name (optional)
      # profile-name = ""
      # IAM Instance profile (optional)
      # iam-instance-profile = ""
      # Custom tags (optional)
      tags {
       # ExampleKey = "ExampleValue"
      }
      # Custom tags for each AWS resource type (optional)
      # Only resources types mentioned further are managed
      tags-for {
        instance {
          # ExampleKey = "ExampleValue"
        }
        volume {
          # ExampleKey = "ExampleValue"
        }
        network-interface {
          # ExampleKey = "ExampleValue"
        }
      }
      
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
      #  Based on your instance configuration, you may want to update Xmx and Xms values.
      jvm-options = ["-Xmx8G", "-Xms512M"]
    }
  ]
}
```


