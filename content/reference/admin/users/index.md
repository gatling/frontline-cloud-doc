---
title: "Users"
description: "Learn how to administrate users and their permissions."
lead: "Users that can authenticate and their permissions."
date: 2021-03-10T13:47:07+00:00
lastmod: 2021-08-05T13:13:30+00:00
weight: 20010
---

## Managing Users

To access the Users administration, click on the {{< icon user >}} icon in the navigation bar.

{{< img src="users-table.png" alt="Users table" >}}

To invite a user to your organization, click on the **Create** button and fill in the user information:

- **GitHub Username**: must match the user GitHub username in order to connect and *cannot be updated*.
- **Firstname** | **Lastname** | **Email address**: explicit.  The user will be able to change these once connected.
- **Roles**: either choose a global role, or select none and specify each role by team according to the permissions you want to grant.

The user invited will have to connect with its Github account. A user can join only one organization at the moment.

{{< img src="users-create.png" alt="User creation" >}}

To edit a user, click on the {{< icon pencil-alt >}} icon. To remove them from your organization, select them using the checkbox on the left of the table and click on the **Remove** button.

## Permissions

There are 4 different user roles in FrontLine:

- System Admin
- Team Admin
- Tester
- Viewer

|                                          | Viewer             | Tester             | Team Admin         | System Admin       |
|------------------------------------------|:------------------:|:------------------:|:------------------:|:------------------:|
| Access own profile and Organization page | {{< icon check >}} | {{< icon check >}} | {{< icon check >}} | {{< icon check >}} |
| Access Reports and Trends                | Own team           | Own team           | Own team           | Own team           |
| Start Simulation                         |                    | Own team           | Own team           | Own team           |
| Generate Public Links                    |                    | Own team           | Own team           | Own team           |
| Create Simulation                        |                    |                    | Own team           | Own team           |
| Administrate Artifacts                   |                    |                    | Own team           | Own team           |
| Administrate API Tokens, Users and Teams |                    |                    |                    | Own team           |

Each role can be global or team-specific.
