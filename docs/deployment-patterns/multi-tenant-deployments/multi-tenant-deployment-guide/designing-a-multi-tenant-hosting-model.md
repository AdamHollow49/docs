---
title: Designing a multi-tenant hosting model
description: Design and implement both dedicated and shared multi-tenant hosting models.
position: 5
---

Previous step: [Working with groups of tenants using tags](/docs/deployment-patterns/multi-tenant-deployments/multi-tenant-deployment-guide/working-with-groups-of-tenants-using-tags.md)

This page describes how to design and implement both **dedicated** and **shared** multi-tenant hosting models using [environments](/docs/infrastructure/environments/index.md), [deployment targets](/docs/infrastructure/index.md), and [tenant tags](/docs/deployment-patterns/multi-tenant-deployments/tenant-tags.md).

:::hint
**Tenanted and untenanted deployments**
In this section, we will focus on tenanted deployments, but untenanted deployments deserve some explanation with regards to hosting. Untenanted deployments provide a way for you to start introducing tenants into your existing Octopus configuration. An untenanted deployment is just like good old Octopus - a deployment to an environment **without** a tenant. Octopus decides which deployment targets to include in a deployment like this:

- **Tenanted deployments** will use **matching tenanted deployment targets**.
- **Untenanted deployments** will only use **untenanted deployment targets**.

We talk more about tenanted and untenanted deployments in [Deploying a simple multi-tenant project](/docs/deployment-patterns/multi-tenant-deployments/multi-tenant-deployment-guide/deploying-a-simple-multi-tenant-project.md).
:::

:::warning
**Mixing tenanted and untenanted deployments on the same machine**
Between **Octopus 3.4** and **Octopus 3.14**, you couldn't mix tenanted and untenanted deployments on the same machine(s). You can work around this by creating a single dummy tenant for your untenanted project.
:::

From **Octopus 3.15.0** you can mark a deployment target for as:

1. Exclude from tenanted deployments.
2. Include only in tenanted deployments
3. Include in both tenanted and untenanted deployments.

![](images/multi-tenant-deployment-target-tenancy-selection.png "width=500")

:::hint
In this guide, we focus on only tenanted deployment targets, and we generally recommend keeping tenanted and untenanted deployment targets separate, particularly in Production. You could use the same deployment targets for other environments but it's better to avoid this situation.
:::

## Multi-tenant hosting

The hosting model you want to achieve will vary depending on your application, customers, and sales model. Instead of trying to cover every scenario, we will cover some of the most common scenarios:

- **Dedicated hosting**: You create new dedicated servers for each customer.
- **Shared hosting**: You create farms or pools of servers to host all of your customers, achieving higher density.

## How Octopus Deploy chooses deployment targets for a tenanted deployment

When you deploy a project, you can deploy to one environment and a selection of tenants. Octopus creates one deployment for each environment/tenant combination, and calculates which deployment targets to include in each deployment using logic like this:

1. Find deployment targets in the target environment with the [roles](/docs/infrastructure/deployment-targets/index.md#target-roles) required by the deployment process.
2. Filter those deployment targets, selecting only those matching the tenant.

Each deployment will then proceed independently with the resulting set of deployment targets. We are going to leverage this behavior to implement dedicated and shared hosting in our sample.

## Scenario: Dedicated hosting

In this case, we want to ensure the applications for some tenants are completely isolated from those of other tenants. You may want to do this to provide security or performance guarantees that would be problematic in a shared hosting model. To implement dedicated hosting, you need to create the dedicated servers and indicate which tenant will be hosted on those servers.

### Step 1: Configure the dedicated deployment targets

Let's configure some deployment targets as dedicated hosts for the tenant we created earlier:

1. Create one or more deployment targets that will be used to host the applications for the tenant. *This could be any type of deployment target.*
2. Configure each deployment target as a dedicated host for the tenant:
   ![](images/multi-tenant-dedicated-beverley.png "width=500")

### Step 2: Deploy

That's it! Now let's deploy the project for this tenant and see the results. You will see how Octopus includes these specific deployment targets in that tenant's deployments, creating an isolated hosting environment for that tenant.

![](images/multi-tenant-deployment-dedicated.png "width=500")

## Scenario: Shared hosting

In this case, we are willing to host the applications of multiple tenants on the same machines to reduce hosting costs by increasing density. To implement shared hosting, you need to create the shared server farm and indicate which tenants will be hosted on that farm. *This is very similar to the dedicated hosting scenario. Instead of choosing a single-tenant, we will use a tenant tag to indicate these servers will be hosting applications for multiple tenants.*

### Step 1: Create a "Hosting" tag set

Firstly let's create a tag set to identify which tenants should be hosted on which shared server farms:

1. Go to **{{Library,Tenant Tag Sets}}** and create a new tag set called **Hosting**.
2. Add a tag called **Shared-Farm-1** and set the color to green to help identify tenants on shared hosting more quickly:
   ![](images/multi-tenancy-shared-tag.png "width=500")

### Step 2: Configure the shared server farm

Now let's configure some shared servers in a farm:

1. Create one or more deployment targets that will be used to host the applications for these tenants. *This could be any type of deployment target.*
2. Select the **Hosting/Shared-Farm-1** tag:

![](images/multi-tenant-infra.png "width=500")

These deployment targets will now be included in deployments for any tenants matching this filter; that is, any tenants tagged with **Hosting/Shared-Farm-1**.

### Step 3: Configure the Tenants to deploy onto the shared server farm

Now let's select some tenants that should be hosted on **Shared-Farm-1**:

1. Create some new tenants (or find existing ones) and tag them with **Hosting/Shared-Farm-1**:
![](images/multi-tenant-shared-server.png "width=500")

### Step 4: Deploy

That's it! Now let's deploy the project for one of these tenants and see the results. You will see how Octopus includes any matching deployment targets in that tenant's deployments, creating a shared hosting environment for your tenants.

![](images/multi-tenant-shared-deployment.png "width=500")

## Next Steps

We will [design and implement a multi-tenant upgrade process](/docs/deployment-patterns/multi-tenant-deployments/multi-tenant-deployment-guide/designing-a-multi-tenant-upgrade-process.md).

## Learn more

- [Deployment patterns blog posts](https://octopus.com/blog/tag/Deployment%20Patterns).
