---
title: Configure Runbook Environments
description: Step by step guide on how to configure environments in Octopus Deploy.
position: 10
hideInThisSection: true
---

!include <creating-environments>

:::hint
Try to reuse the same environments as your deployments whenever possible.  You will often runbooks runbooks on the same deployment targets as your deployment process.  Creating runbook only environments can saturate your dashboards and lifecycles.  If you need to have an environment for runbooks, we recommend limiting it to one or two environments at most with a name similar to `Maintenance`.
:::

The next step will [create a project to house the runbook](docs/getting-started/first-runbook-run/create-runbook-projects.md).

**Further Reading**

For further reading on deployment targets in Octopus Deploy please see:

- [Deployment Targets](/docs/infrastructure/deployment-targets/index.md)
- [Runbook Documentation](/docs/runbooks/index.md)
- [Runbook Examples](/docs/runbooks/runbook-examples/index.md)

<span><a class="btn btn-success" href="/docs/getting-started/first-runbook-run/create-runbook-projects">Next</a></span>