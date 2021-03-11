---
title: "Artifacts"
description: ""
lead: ""
date: 2021-03-10T09:29:36-05:00
lastmod: 2021-03-10T09:29:36-05:00
draft: false
images: []
menu:
  docs:
    parent: "user"
weight: 040
---

To access the Artifacts section, click on **Artifacts** in the navbar.

The Simulations view contains all the artifacts you have configured with the given name, team, and filename of uploaded artifact if not empty.

{{< img src="artifact-table.png" alt="Artifact table" >}}

## Creation

In order to create a simulation click on the "Create" button in the simulations table.
FrontLine Cloud private beta is limited to 5 artifacts

{{< img src="artifact-create.png" alt="Artifact creation" >}}

- **Name**: the name that will appear on the simulations build step.
- **Team**: the team which owns the artifact.

## Upload

In order to filled the artifact with your bundled simulation, click on the icon:upload[] icon on the right side of the table.

{{< img src="artifact-upload-empty.png" alt="Artifact upload empty" >}}

You can then drag and drop your artifact to filled the form and proceed to upload.

{{< img src="artifact-upload-filled.png" alt="Artifact upload filled" >}}

You can upload multiple artifact concurrently, progress can be tracked from anywhere in the application through **Uploads** in the navbar.

{{< img src="artifact-upload-sidenav.png" alt="Artifact upload progress button" >}}
{{< img src="artifact-upload-progress.png" alt="Artifact upload progress" >}}

## Usage

You can configure the artifact to use on the simulation build step.

{{< img src="artifact-simulation-step.png" alt="Artifact upload filled" >}}