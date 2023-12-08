---
title: "Private Packages"
description: "How to install and use a private repository on your Control Plane"
lead: "Store your simulations' packages privately in your infrastructure, and use them with private locations"
date: 2023-08-09T12:00:00+00:00
lastmod: 2023-08-09T12:00:00+00:00
weight: 22038
---

# Introduction

Private Locations facilitate running simulations in your dedicated cloud environment. 
Ensure secure storage of sensitive simulation packages in your private cloud. 

The control plane offers a private repository; enable it for confidential package management!

# Private packages

A private package is uploaded through the control plane into a private repository.
_Gatling Enterprise Cloud only receives the Gatling version associated with the package and the names of simulation classes, which helps in simulation configuration_

When initiating a Gatling run, the control plane generates a temporary signed link to allow the download of the private package from the load generators.

{{< img src="private_packages_general_architecture.png" alt="Infrastructure schema" >}}

{{< alert info >}}
Private Packages are based on Private Locations. You can not use them with managed locations.
{{< /alert >}}

## Infrastructure

Currently, Private Packages can be integrated with either AWS S3 or GCP Cloud Storage repositories.

{{< alert info >}}
Before going further, ensure that your repository is ready to hold your packages.
{{< /alert >}}

### Control plane server

The control plane with a private repository has a server to manage uploads to the repository, secured by a Gatling Enterprise Cloud API Token with `Configure` role.

The server is accessible on port 8080 by default when a repository is configured.
The following **optional server configuration** with the default settings is provided for your reference.

```bash
control-plane {
  repository {
    # Upload configuration (optional)
    upload {
      directory = "/tmp" # (optional, default: /tmp)
    }
    # Server configuration (optional)
    server {
      port = 8080 # (optional, default: 8080)
      bindAddress = "0.0.0.0" # (optional, default: 0.0.0.0)
      
      # PKCS#12 certificate (optional)
      certificate {
        path = "/path/to/certificate.p12"
        password = ${CERTIFICATE_PASSWORD} # (optional)
      }
    }
  }
}
```

This configuration includes the following parameters:
- **upload.directory**: This directory temporarily stores uploaded JAR files. (optional)
- **server.port**: The port on which the control plane is listening for private package uploads.
- **server.bindAddress**: The network interface to bind to. The default is `0.0.0.0`, which means all available network IPv4 interfaces.
- **server.certificate**: The server P12 certificate for secure connection without SSL reverse proxy. (optional)

### Control Plane repository

#### AWS S3

{{< alert warning >}}
Control plane with private repository needs AWS permissions `s3:PutObject`, `s3:DeleteObject` and `s3:GetObject` on the bucket.
{{< /alert >}}

Once it is done, add the private repository configuration section in your [control plane configuration]({{< ref "../introduction" >}}) file:

```bash
control-plane {
  repository {
    # S3 Bucket configuration
    type = "aws"
    bucket = "bucket-name"
    path = "folder/to/upload" # (optional, default: root)
  }
}
```

This configuration includes the following parameters:
- **bucket**: The name of the bucket where packages are uploaded to on AWS S3.
- **path:** The path of a folder in AWS S3 bucket. (optional)

#### GCP Cloud Storage

{{< alert warning >}}
Control plane with private repository needs GCP service account role with permissions `storage.objects.create`, 
`storage.objects.delete` and `iam.serviceAccounts.signBlob` on the bucket.
{{< /alert >}}

```bash
control-plane {
  repository {
    # Cloud Storage Bucket configuration
    type = "gcp"
    bucket = "bucket-name"
    path = "folder/to/upload" # (optional, default: root)
  }
}
```

This configuration includes the following parameters:
- **bucket**: The name of the bucket where packages are uploaded to on GCP Cloud Storage.
- **path:** The path of a folder in Cloud Storage bucket. (optional)

#### Azure Blob Storage

{{< alert warning >}}
Control plane with private repository needs to be associate with Azure storage account role `Storage Blob Data Contributor`.

For more information, check [Authenticate to Azure and authorize access to blob data](https://learn.microsoft.com/en-us/azure/storage/blobs/storage-quickstart-blobs-java?tabs=powershell%2Cmanaged-identity%2Croles-azure-portal%2Csign-in-azure-cli#authenticate-to-azure-and-authorize-access-to-blob-data)
{{< /alert >}}

```bash
control-plane {
  repository {
    # Azure Blob Storage configuration
    type = "azure"
    storage-account = "storage-account-name"
    container = "container-name"
    path = "folder/to/upload" # (optional, default: root)
  }
}
```

#### Filesystem Storage
This option allows the storage of simulations directly on the control-plane filesystem.
```bash
  repository = {
    # Filesystem configuration
    type = "filesystem"
    # Directory to store your private packages
    directory = "/data/gatling-repository"
    upload {
      # Directory to temporarily store your incoming simulation during the upload process
      directory = "/tmp" # (optional, default: /tmp)
    }
    location {
      # URL of your control-plane from your private locations
      download-base-url = "http://www.example.com:8080"
    }
  }
```

{{< alert warning >}}
Please note that the optional `upload.directory` configuration, which defaults to /tmp, will be used to temporarily store your incoming simulation during the upload process.
Once the upload is complete, the file will be stored in your configured directory (`/data/gatling-repository` in the provided example).
{{< /alert >}}

This configuration includes the following parameters:
- **directory**: The directory where the simulations will be stored.
- **location.download-base-url**: The access URL for the control-plane. This URL will be provided to the load-generators so that they can download your simulations. 

## Usage

After configuration, restart your control plane to start the server.

### Creating a private package

To create a private package, you can use Gatling Enterprise Cloud's public API. Follow these steps:

1. Obtain an API Token with `Configure` permission for the relevant team.
2. Navigate to the Swagger package section for further instructions on creating a private package.
3. When creating the package, select a private storage type.
4. Use Gatling plugins to create and upload your private packages. You can refer to the Gatling documentation for specific plugin usage:
  - [Maven plugin](https://gatling.io/docs/gatling/reference/current/extensions/maven_plugin/)
  - [Gradle plugin](https://gatling.io/docs/gatling/reference/current/extensions/gradle_plugin/)
  - [SBT plugin](https://gatling.io/docs/gatling/reference/current/extensions/sbt_plugin/)
5. For each plugin, you will need to configure it with the following details:
  - Your API token with `Configure` permission for the private package team.
  - The private package ID.
  - The control plane repository URL.

{{< alert tip >}}
To create a private package easily from the plugin instead of the API, you can use the interactive enterprise start command.
{{< /alert >}}

### Deleting a private package

To delete a private package, delete the package within Gatling Enterprise Cloud. 
The control plane will receive the order to delete the package on the configured private repository.
