---
title: Delete project
description: An example script that deletes a project.
---

This script demonstrates how to programmatically delete a project in Octopus Deploy.

## Usage
Provide values for the following:
- Octopus URL
- Octopus API Key
- Name of the space to use
- Name of the project

:::warning
**This script will delete the project with the specified name. This operation is destructive and cannot be undone. Ensure you have a database backup and take care when running this script or one based on it**
:::

## Script

!include <delete-project-by-name-scripts>
