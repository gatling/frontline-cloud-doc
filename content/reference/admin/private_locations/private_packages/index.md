---
title: "Private Packages"
description: "How to install and use a private repository on your Control Plane"
lead: "Store your simulations' packages privately in your infrastructure, and use them with private locations"
date: 2023-08-09T12:00:00+00:00
lastmod: 2023-08-09T12:00:00+00:00
weight: 22033
---

# Introduction

Using Private Locations allows you to run your simulations in your private cloud. Now, what about your simulations' packages? They may hold private information, so you may want to also store them in your private cloud.

Your Control Plane has a private repository feature, allowing you to manage your simulation packages privately, you just have to enable it.

# Private packages

## Infrastructure

{{< img src="private_packages_general_architecture.png" alt="Infrastructure schema" >}}

{{< alert info >}}
Private Packages are based on Private Locations. You can not use them with managed locations.
{{< /alert >}}

## Installation

First step: follow the control plane installation guide for private locations.

{{< alert info >}}
Before going further, ensure that your AWS S3 bucket is ready to hold your packages.
{{< /alert >}}

{{< alert warning >}}
Control plane with private repository needs AWS permissions `s3:PutObject`` and `s3:DeleteObject` on the bucket.
{{< /alert >}}

Once it is done, add the private repository configuration section in your [control plane configuration]({{< ref "../introduction" >}})  file :
```bash
control-plane {
	repository {
		# S3 Bucket configuration
		bucket = "bucket-name"
		path = "folder/to/upload" # Optional, default to root, do not set trailing slash
		upload {
			directory = "/tmp" # Optional, default value /tmp
		}
		# Server configuration
		server {
			port = 8080 # Optional, default value 8080
			bindAddress = "0.0.0.0" # Optional, default value 0.0.0.0
		} 
	}
}
```

This configuration includes the following parameters:

- **bucket**: The name of the bucket where packages are uploaded to on AWS S3.
- **path:** The path of a folder in AWS S3. (optional)
- **upload.directory**: This directory temporarily stores uploaded JAR files. (optional)
- **server.port**: The port on which the control plane is listening for private package uploads.
- **server.bindAddress**: The network interface to bind to. The default is 0.0.0.0, which means all available network IPv4 interfaces.

After configuration, restart your control plane to start the server.

## Usage

You have to use Gatling plugins to create and upload your private packages. See Gatling documentation :
* [Maven plugin](https://gatling.io/docs/gatling/reference/current/extensions/maven_plugin/)
* [Gradle plugin](https://gatling.io/docs/gatling/reference/current/extensions/gradle_plugin/)
* [SBT plugin](https://gatling.io/docs/gatling/reference/current/extensions/sbt_plugin/)

You have to set the `controlPlaneUrl` setting to use your control plane for private packages storage with the plugins. See your plugin documentation for details.

Use plugin tasks for creating and updating your private packages :
* Creation : enterpriseStart
* Update : enterpriseUpload
