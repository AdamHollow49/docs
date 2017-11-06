---
title: Key Concepts
description: Key concepts to understanding how Octopus Deploy works.
position: 2
---

## Projects, deployment processes, lifecycles and variables {#KeyConcepts-Projects,deploymentprocesses,lifecyclesandvariables}


The deployment process consists of a number of steps. Octopus supports many different kinds of steps, such as:

- [Deploying NuGet packages](/docs/deploying-applications/deploying-packages/index.md) (this is how web sites and windows services are [packaged](/docs/packaging-applications/index.md) and deployed)
- Running ad-hoc [Custom scripts](/docs/deploying-applications/custom-scripts/index.md)
- [Sending an email](/docs/deploying-applications/email-notifications.md)
- Pausing for [manual intervention](/docs/deploying-applications/manual-intervention-and-approvals.md) by a human

Importantly, steps are run in a specific order, like following a recipe. It would be dreadful if we tried to deploy a new version of a web site before running the database migrations, as an example. Steps can also be set to run [conditionally](/docs/deploying-applications/index.md), as part of a [rolling deployment](/docs/patterns/rolling-deployments.md), or even in parallel with each other.

When you define a project, you also select a [lifecycle](/docs/key-concepts/lifecycles.md). The lifecycle defines the rules around how releases of the project are allowed to be deployed between environments.

