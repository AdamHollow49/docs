---
title: Add a Space with environments
description: An example script to create a new space and populate it with some default environments.
---

This script is a starter for bootstrapping a new [Space](/docs/administration/spaces/index.md) in your Octopus instance.

It creates a new space with the provided name, description, and managers. At least one manager team or member must be provided.

Then the script will create the [Environments](/docs/infrastructure/environments/index.md) provided in the newly created space.

## Usage

Provide values for:

- Octopus URL
- Octopus API Key
- Space Name
- Environments
- A combination of Manager Teams and Manager Team Members

## Scripts

!include <add-a-space-with-environments-scripts>
