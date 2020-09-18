---
title: Run a health check
description: An example script that creates and runs a health check task.
---

This script demonstrates how to programmatically create and run a [health check](/docs/infrastructure/deployment-targets/machine-policies.md) task in Octopus Deploy.

## Usage

Provide values for:

- Octopus URL
- Octopus API Key
- Description for the health check task
- Timeout value (in minutes) for the task
- Machine timeout value (in minutes) for the health check to use when run against machines
- One of:
  - An environment name to run the health check task against or
  - A list of machine names to run the health check task against or
  - A combination of both environment and machines

## Script

!include <run-healthcheck-scripts>
