---
title: "Organization"
description: "Learn how to access organization details."
date: 2021-08-05T13:13:30+00:00
lastmod: 2021-08-05T13:13:30+00:00
weight: 10081
---

To view information about your organization, click on the **Organization name link**.

{{< img src="organization-page-link.png" alt="Organization page link" >}}

## Profile information

{{< img src="organization-profile.png" alt="Organization profile information" >}}

* **Avatar** - Composed by default from the two first characters of your **Organization name**.
* **Organization Name** - The display name for your organization.
* **Organization Slug** - Unique string name, in lowercase and spaced by dashes `-`.


{{< alert tip >}}
Click on the pen icon to edit the **Organization name**.
{{< /alert >}}

## Credit consumption

{{< img src="organization-credit.png" alt="Organization credit informations" >}}

* **Blue** - Available credits.
* **Orange** - Consumed credits.

### Admin users

Shows all System Admins in your organization.

{{< img src="organization-admin-user.png" alt="Organization users admin" >}}

For each System Admin, you will find their GitHub username, first name, and last name.

### Credits

{{< alert warning >}}
This section is only available to System Admins.
{{< /alert >}}

Credits consumption history.

{{< img src="organization-credits-row.png" alt="Organization credits view in row" >}}

By clicking on a row, you will see all the details of the credit consumption for each month.

{{< img src="organization-credits-detail.png" alt="Organization credits view in detail" >}}

* **Type** - Which type of event occurred, with a link redirecting to the run when credits were used to run a simulation.
* **Date** - The day the event took effect.
* **Credits** - Number of credits gained or used from the event.

### Plans

{{< alert warning >}}
This section is only available to the System Admin.
{{< /alert >}}

Plans view history.

{{< img src="organization-plan.png" alt="Organization plan" >}}

* **Status** - Current status of the payment plan: **Terminated**, **Active**, or **PaymentFailure**.
* **From** - Start date of the plan.
* **To** - End date of the plan, if there is one.
* **Credits** - Number of credits awarded each month by the plan.
