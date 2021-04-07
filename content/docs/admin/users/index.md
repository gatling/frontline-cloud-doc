---
title: "Users"
description: "Learn how to administrate users and their permissions."
lead: "Users that can authenticate and their permissions."
date: 2021-03-10T08:47:07-05:00
lastmod: 2021-03-10T08:47:07-05:00
draft: false
images: []
menu:
  docs:
    parent: "admin"
weight: 20010
---

## Managing Users

To access the Users administration, click on the {{< icon user-cog >}} icon in the navigation bar.

{{< img src="users-table.png" alt="Users table" >}}

To create a user, click on the **Create** button and fill the user information:

- **Username**: must match user Github username in order to connect and *cannot be updated*.
- **Firstname** | **Lastname** | **Email**: explicit
- **Roles**: either choose a global role, or select none and specify each role by team according to the permissions you wan't to grant.

{{< img src="users-create.png" alt="User creation" >}}

You can edit the user by clicking on the {{< icon pencil-alt >}} icon and delete them using the checkboxes on the table's right part.

## Permissions

There are 4 different user roles in FrontLine:

- System Admin
- Team Admin
- Tester
- Viewer

|                                          | Viewer             | Tester             | Team Admin         | System Admin       |
|------------------------------------------|:------------------:|:------------------:|:------------------:|:------------------:|
| Access own profile                       | {{< icon check >}} | {{< icon check >}} | {{< icon check >}} | {{< icon check >}} |
| Access Reports and Trends                | Own team           | Own team           | Own team           | Own team           |
| Start Simulation                         |                    | Own team           | Own team           | Own team           |
| Generate Public Links                    |                    | Own team           | Own team           | Own team           |
| Create Simulation                        |                    |                    | Own team           | Own team           |
| Administrate Artifacts                   |                    |                    | Own team           | Own team           |
| Administrate API Tokens, Users and Teams |                    |                    |                    | Own team           |

Each role can be global or team-specific.
