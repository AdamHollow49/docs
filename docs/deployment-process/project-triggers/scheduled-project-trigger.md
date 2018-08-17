---
title: Scheduled Project Triggers
description: Automatic deployment triggers allow you to define unattended behavior for your project that will cause an automatic deployment of a release into an environment.
position: 2
version: 2018.4
---

Scheduled Deployment Triggers were introduced in **Octopus 2018.4**.

Scheduled Deployment Triggers allow you to define an unattended behavior for your [Projects](/docs/deployment-process/projects/index.md) that will cause an automatic deployment of a release based on a defined recurring schedule.

## Schedule

Scheduled deployment triggers provide a way to configure your projects to create, deploy, and promote releases on a defined schedule.
You can define a schedule in the following ways:

### Daily Schedule

The Daily Schedule is run every day of the week. It can be configured to run once per day, every x hours, or every x minutes.
Examples of when this option would be useful is if you wanted to:
* Run a deployment to clean up your test environments once a day at 9:00pm.
* Run a deployment to health check your services every hour.

![](/docs/images/scheduled-project-triggers/scheduled-project-triggers-daily-schedule.png "width=500")

### Days Per Week
The Days Per Week schedule works the same way as the daily schedule, however, you can also choose the days of the week you want the schedule to run on.

Example:

* Run a deployment to provision a new test environment at 6:00am, Monday - Friday.

![](/docs/images/scheduled-project-triggers/scheduled-project-triggers-days-per-week-schedule.png "width=500")

### Days Per Month
The Days Per Month schedule allows you to configure a trigger that will run on a specific date of the month or specific day of week of every month.

Example:

* Run a deployment to promote the latest build from staging to production on the 1st day of the month.
* Run a deployment to perform maintenance on the last Saturday of the month.

![](/docs/images/scheduled-project-triggers/scheduled-project-triggers-days-per-month-schedule.png "width=500")

### CRON Expression

CRON expressions allow you to configure a trigger that will run according to the specific CRON expression.

Example:

`0 0 06 * * Mon-Fri`

Runs at 06:00 AM, Monday through Friday.

![](/docs/images/scheduled-project-triggers/scheduled-project-triggers-cron-expression.png "width=500")

:::success
The CRON expression must consist of all 6 fields, there is an optional 7th field for "Year".
:::

| Field name    | Allowed values       | Allowed special characters  | Required |
| ------------- |:-------------------- |:--------------------------- | :------: |
| Seconds       | 0-59                 | * , - /                     | Y        |
| Minutes       | 0-59                 | * , - /                     | Y        |
| Hours         | 0-23                 | * , - /                     | Y        |
| Day of month  | 1-31                 | * , - / ? L W               | Y        |
| Month         | 1-12 or JAN-DEC      | * , - /                     | Y        |
| Day of week   | 0-6 or SUN-SAT       | * , - / ? L #               | Y        |
| Year          | 0001–9999            | * , - /                     | N        |

## Action

### Deploy Latest Release

The Deploy Latest Release action allows you to re-deploy a release or promote a release between environments.
* **Source Environment**: The latest successful deployment in this environment will be used to deploy to the destination environment.
* **Destination Environment**: The release selected from the source environment will be deployed to this environment.

If you are using [channels](/docs/deployment-process/channels/index.md) you may also select the channel to use when deploying the release. The latest successful deployment for the specified channel and source environment will be deployed to the same channel and destination environment. If no channel is specified, the latest successful release from any channel and source environment will be selected for deployment.

If you are using [tenants](/docs/deployment-patterns/multi-tenant-deployments/index.md) you can select the tenants that will receive a deployment. For each tenant, the latest successful release in the source environment will be deployed to the destination environment. When a tenant is not connected to the source environment, the latest successful release that has been deployed to the source environment and meets the lifecycle requirements for promotion to the destination environment will be deployed.

:::success
If the same environment is selected for both source and destination the latest successful deployment will be re-deployed to that environment. If different environments are selected, the latest successful release in the source environment will be promoted to the destination environment.
:::

### Deploy New Release
The Deploy New Release action will create a new release and deploy it to the selected destination environment. If you are using channels and tenants you can also select which channel the release will be created in, and which tenants will receive the deployment.

The newly created release will select the latest package versions that match the channel rules that the release is created in.
