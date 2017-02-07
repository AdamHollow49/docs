---
title: Auditing
description: Octopus Deploy captures audit information whenever significant events happen in the system.
position: 0
---

For team members to collaborate in the deployment of software, there needs to be trust and accountability. Octopus Deploy captures audit information whenever significant events happen in the system.

## What does Octopus capture? {#Auditing-WhatdoesOctopuscapture?}

Below is a short list of just some of the things that Octopus captures:

- Changes to [deployment processes](/docs/deploying-applications/index.md) and [variables](/docs/deploying-applications/variables/index.md)
- Create/modify/delete events for [projects](/docs/key-concepts/projects/index.md), [environments](/docs/key-concepts/environments/index.md), [deployment targets](/docs/deployment-targets/index.md), releases, and so on
- Environment changes, such as adding new deployment targets or modifying the environment a deployment target belongs to
- Queuing and cancelling of deployments and other tasks

## Viewing the audit history {#Auditing-Viewingtheaudithistory}

You can view the full audit history by navigating to the **Audit** tab in the **Configuration** area.

![](/docs/images/3048138/3278051.png "width=500")

Some audit events will also include details, which you can see by clicking the **show details** link. For example:

![](/docs/images/3048138/3278050.png "width=500")

![](/docs/images/3048138/3278049.png "width=500")

This feature makes it extremely easy to see who made what changes on the Octopus Deploy server.

:::success
**Special audit view permission**
In Octopus 3.4 we have introduced a new Permission called **AuditView** which allows someone to view the audit logs without needing other permissions. Learn about [managing users and teams](/docs/administration/managing-users-and-teams/index.md).
:::
