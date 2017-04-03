---
title: Subscriptions
description: Subscriptions allow you to subscribe to events that are happening within Octopus, so you can be notified when events have occurred and react accordingly.
position: 16
---

:::hint
Subscriptions are available in Octopus Deploy 3.5 and later
:::

Subscriptions allow you to subscribe to events that are happening within Octopus, so you can be notified when events have occurred and react accordingly. Both email and webhook notifications are currently supported.

Subscriptions can be accessed from the Configuration menu.

## Email Notifications {#Subscriptions-EmailNotifications}

Email notifications can be setup to send an email periodically to the users of one or more [teams](/docs/administration/managing-users-and-teams/index.md). Emails will be sent periodically according to the frequency you specify, and the email will include a digest of events that have occurred (up to a maximum of 100 events). For example, this can be useful if your team has setup automated deployments with the [Elastic and Transient Environment](/docs/guides/elastic-and-transient-environments/index.md) features of Octopus and wish to be notified if an auto-deployment is ever blocked or has failed.

Emails may also include a link to your Octopus Audit screen, filtered to match the events delivered in the email. To include this link, you need to have set the publicly-accessible URL of your Octopus instance (see the {{Configuration,Nodes,Configuration Settings}} menu or the [Server Configuration](/docs/administration/server-configuration.md) documentation for more details).

## Webhook Notifications {#Subscriptions-WebhookNotifications}

!partial <webhooks>

## Event Visibility and Permissions {#Subscriptions-EventVisibilityandPermissions}

Because certain teams may be restricted to only see certain events, subscriptions give you the ability to scope to one or more teams. Teams may be restricted to certain criteria, such as project(s) and/or environment(s). Combine these restrictions with team roles and you can successfully control which events get seen for a given subscription. See more information on [Managing Users and Teams](/docs/administration/managing-users-and-teams/index.md) as well as our [User Roles](/docs/administration/managing-users-and-teams/user-roles.md) documentation if you wish to learn more.

## Troubleshooting

Logs for subscriptions can be found in the `Configuration` menu under `Diagnostics` (see the `Subscription logs` tab).