---
title: Create a tenant
description: An example script that creates a tenant.
---

This script demonstrates how to programmatically create a new [tenant](/docs/deployments/patterns/multi-tenant-deployments/index.md) in Octopus.

## Usage

Provide values for:

- Octopus URL
- Octopus API Key
- Name of the space to use
- Name of the tenant to create
- A list of Project names to connect the new tenant with
- A list of Environment names to connect the new tenant with
- A list of Tenant tags to use with the new tenant

:::hint
**Note:** 
In order for this script to execute correctly, please note the following:
- The projects provided must have the Multi-tenanted deployment setting enabled.
- The environments provided must exist.
- The optional tenant tags provided must exist.
:::

## Script

!include <create-a-tenant-scripts>