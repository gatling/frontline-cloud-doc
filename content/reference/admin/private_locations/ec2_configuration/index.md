---
title: "Configuring AWS EC2 Locations"
description: "Load Generators on your private AWS account"
lead: "Private Locations on your AWS account"
date: 2023-01-12T16:46:04+00:00
lastmod: 2023-01-12T16:29:04+00:00
weight: 22053
---

## AWS EC2
AWS private locations require the control plane to have access to AWS credentials from the default credential provider chain.

See [the AWS documentation for the Default Credential Provider Chain](https://docs.aws.amazon.com/sdk-for-java/v1/developer-guide/credentials.html#credentials-default).

```bash
control-plane {
  # Control plane token
  token = "cpt_example_c7oze5djp3u14a5xqjanh..."
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
      # Custom tags
      tags {
       # ExampleKey = ExampleValue
      }
    }
  ]
}
```
