---
title: "Notifications"
description: "Get notified about your simulation runs"
lead: "Get notified about your simulation runs"
date: 2021-11-07T14:29:04+00:00
lastmod: 2023-04-03T12:00:00+00:00
weight: 22030
---

## Introduction

You can configure your Gatling Cloud to send notifications about your simulation runs results directly in your favourite text communication tool.

Notifications will be sent as soon as a simulation run ends, and will display:
- A summary of your simulation info
- The run result (Successful, Assertions failure, etc.)
- Assertions results, if you configured any in your simulation

Navigate to your organization configuration page, and to the "Notifications" tab.

From there, you will be able to activate and configure notifications for your communication tools:

{{< img src="notifications-configuration-1.png" alt="Notifications Tab 1" >}}

{{< img src="notifications-configuration-2.png" alt="Notifications Tab 2" >}}

## Slack

{{< img src="notifications-slack-example-1.png" alt="Slack example 1" >}}

### Preparation

Slack notifications are based on Slack webhooks, so you will have to configure one before using it in Gatling Cloud.

Follow [the official Slack documentation](https://api.slack.com/messaging/webhooks) to create one targeting the channel on which you want your notifications.

Keep your new webhook URL for the next step. It will be a URL that looks like the following:
`https://hooks.slack.com/services/T00000000/B00000000/XXXXXXXXXXXXXXXXXXXXXXXX`

### Configuration

Click on the toggle to activate notifications for Slack.

Paste your webhook URL in the text field, then you can test and save it:

{{< img src="notifications-configuration-slack-1.png" alt="Notifications Slack 1" >}}

- The "Test" button sends a hello world message to your webhook before saving it.
- "Save" persists your configuration. The next simulation runs will send a notification at the end.

To deactivate Slack notifications, click on the toggle, and confirm your choice.

{{< alert warning >}}
Deactivating notifications deletes the webhook URL from configuration, we do not keep it in our database.
{{< /alert >}}

## Microsoft Teams

{{< img src="notifications-teams-example-1.png" alt="Teams example 1" >}}

### Preparation

Microsoft Teams notifications are based on Microsoft Teams webhooks, so you will have to configure one before using it in Gatling Cloud.

Follow [the official Microsoft Teams documentation](https://learn.microsoft.com/en-us/microsoftteams/platform/webhooks-and-connectors/how-to/add-incoming-webhook#create-incoming-webhooks-1) to create one targeting the channel on which you want your notifications.

Keep your new webhook URL for the next step. It will be a URL that looks like the following:
`https://xxxxx.webhook.office.com/xxxxxxxxx`

### Configuration

Click on the toggle to activate notifications for Microsoft Teams.

Paste your webhook URL in the text field; then you can test and save it:

{{< img src="notifications-configuration-teams-1.png" alt="Notifications Teams 1" >}}

- The "Test" button sends a hello world message to your webhook before saving it.
- "Save" persists your configuration. The next simulation runs will send a notification at the end.

To deactivate Microsoft Teams notifications, click on the toggle, and confirm your choice.

{{< alert warning >}}
Deactivating notifications deletes the webhook URL from configuration, we do not keep it in our database.
{{< /alert >}}
