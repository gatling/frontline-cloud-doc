---
title: "Artifact Configuration"
description: "Learn how to configure and upload an artifact to Gatling Enterprise."
lead: "Configure and upload your artifact to Gatling Enterprise."
date: 2021-03-10T09:29:36-05:00
lastmod: 2021-03-10T09:29:36-05:00
weight: 10050
---

## Managing

To access the Artifacts section, click on **Artifacts** in the navigation bar.

The Artifacts view contains all the artifacts you have configured with the given name, team, and filename of uploaded artifact if not empty.

The Gatling version of the artifact is displayed in a badge next to the filename.

{{< img src="artifact-table.png" alt="Artifact table" >}}

## Creation

In order to add an artifact, click on the "Create" button above the artifacts table.

{{< img src="artifact-create.png" alt="Artifact creation" >}}

- **Name**: the name that will appear on the simulations build step.
- **Team**:
  - you can select **Global** so that all teams have access to the artifact;
  - or select **Specific** and choose a single team which will own the artifact.

## Upload

### Option 1: Manual Upload

In order to fill the artifact with your bundled simulation, click on the {{< icon upload >}} icon on the right side of the table.

{{< alert tip >}}
In order to package a bundle of your simulation, refer to the [Artifact Generation documentation]({{< ref "../artifact_gen" >}}).
{{< / alert >}}

{{< img src="artifact-upload-empty.png" alt="Artifact upload empty" >}}

You can then drag and drop your artifact to fill the form and click on the **Upload** button.

{{< img src="artifact-upload-filled.png" alt="Artifact upload filled" >}}

You can upload multiple artifacts concurrently. Uploads progress will be shown in the Artifacts table.

You can cancel an upload in progress by clicking on the cancel icon.

{{< img src="artifact-upload-progress.png" alt="Artifact upload progress" >}}

### Option 2: API Upload

You can also upload artifacts programmatically with our REST API.

You'll need:
* an [API token]({{< ref "../../admin/api_tokens" >}}) with at least the `Artifacts` permission
* the Artifact's ID, which you can copy from the WebUI.

You can then upload your artifact, eg with `curl`:

```
curl -X PUT --upload-file <ARTIFACT_LOCAL_PATH> \
  "https://<DOMAIN>/api/public/artifacts/<ARTIFACT_ID>/content?filename=<ARTIFACT_FILE_NAME>" \
  -H "Authorization:<API_TOKEN>"
```

## Usage

You can configure which artifact to use for a simulation in the simulation's **Build** step.

{{< img src="artifact-simulation-step.png" alt="Artifact upload filled" >}}
