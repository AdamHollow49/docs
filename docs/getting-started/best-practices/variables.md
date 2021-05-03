---
title: Variable Recommendations
description: Guidelines and recommendations for configuring variables in Octopus Deploy.
position: 60
hideInThisSection: true
---

[Variables](/docs/projects/variables/index.md) allow you to parameterize your deployment and runbook process.  That allows your processes to work across your infrastructure without having to hard-code or manually update configuration settings that differ across environments, deployment targets, channels, or tenants.

There are multiple levels of variables in Octopus Deploy:

1. Project 
2. Tenant 
3. Step Templates
4. Library Set
5. System Variables

Project, Tenant, and Step Template variables are associated with their specific item and cannot be shared.  Library set variables can be shared between 1 to N Projects and Tenants.  System variables are variables provided by Octopus Deploy you can use during deployments.

During a deployment, Octopus will gather all the variables for the project, Tenant (when applicable), step template, associated library sets, and system variables and create a "variable manifest" for each step to use.

:::hint
Multi-tenancy is an advanced topic, with its own set of recommendations.  Tenants were mentioned here so you could see the bigger picture of variables.
:::

Octopus Deploy provides the ability to replace values in your configuration files using the [structured configuration variables](/docs/projects/steps/configuration-features/structured-configuration-variables-feature.md) or the [.NET XML configuration variables feature](/docs/projects/steps/configuration-features/xml-configuration-variables-feature.md).  In addition, Octopus Deploy supports the ability to perform [.NET Configuration transforms](/docs/projects/steps/configuration-features/configuration-transforms/index.md) during deployment time.

In addition to having the above levels of variables, there are also two categories of variables.

1. Variables used in configuration file replacement (connection strings, version number, etc.)
2. Variables specific to the deployment or runbook run (output variables, messages, accounts, etc.)

## Variable Naming

It is entirely possible for a name collision to occur, where a project and a library variable set have the same variable name scoped to the same environment.  When a name collision occurs, Octopus Deploy will do its best to pick the ["right one" using an algorithm](//docs/projects/variables/index.md#Scopingvariables-Scopespecificity).

However, the recommendation is to avoid name collisions in the first place by following these naming standards.

1. Project: `Project.[Component].[Name]` - for example, **Project.Database.UserName.**
2. Tenant: `[ProjectName].[Component].[Name]` - for example, **OctoPetShopWebUI.URL.Port**.
3. Step Template: `[TemplateName].[Component].[Name]` - for example, **SlackNotification.Message.Body**.
4. Library Set: `[LibrarySetName].[Component].[Name]` - for example, **Notification.Slack.Message**.

These naming conventions only apply to variables used for a deployment or runbook run.  Variables used for configuration file replacement have a specific naming convention to follow.  The above naming convention makes it easier to distinguish between the two.

## Configuration File Replacement Variables

One of Octopus Deploy's most used features is the variable scoping.  And with good reason, having the same process, with only needing a specific value such as a connection string or domain name changed, ensures consistency during the deployment process.  

That ability to scope variables to environments and replace them in configuration files can result in a some anti-patterns which include:

- Dozens or hundreds of variables being replaced in a configuration file during deployment.
- Needing a process to automatically add variables in Octopus Deploy based on a custom structure in source control.
- Needing the ability to only edit one library variable set associated with a specific project.
- Desiring the ability to source control variables from Octopus Deploy.
- Wanting to be able to loop through all project variables and add them to a file during a deployment.

Our recommendation is to use a combination of Octopus Deploy and configuration files stored in source control.  You'd have three levels of configuration files:

- Main configuration file (appSettings.json)
- Environment specific configuration file (appSettings.Development.json)
- Tenant specific configuration file (appSettings.MyTenantName.json)

Octopus Deploy can set an environment variable or configuration value during deployment to indicate which environment-specific configuration file to use.  Or, if you are using .NET Framework, you can leverage [configuration file transforms](/docs/projects/steps/configuration-features/configuration-transforms/index.md).

When someone proposes storing all the configuration variables in Octopus Deploy, we will ask the following questions:
- Which variables does Octopus require for a deployment or runbook run?
- Which variables contain sensitive information?
- Who should own the maintenance of these variables?
- Which variables are the most volatile?

Examples of configuration variables that should be stored in Octopus Deploy.  These variables are required for a successful deployment, and in the case of database connection strings, they shouldn't be stored in source control.  They are also unlikely to change between releases.
- Database Connection Strings, including username and password
- Domain names
- Server ports
- Service URLs

There are also variables only Octopus Deploy knows about.  These include the release version number, environment name, and deployment date.  

Examples of configuration variables that should be stored in the main configuration file, environment specific, or tenant-specific configuration files.  These items are typically more volatile, and you'd want a history of any changes.
- Feature flags
- CSS Settings
- Log Levels

## Library Variable Sets

[Library variable sets](/docs/projects/variables/library-variable-sets.md) are a great way to share variables between projects.  We recommend the following when creating library variable sets.

- Don't have a single "global" library variable set.  This becomes a "junk drawer" of values and quickly becomes unmanageable.  And not every project will need all those variables.
- Group common variables into a library variable set.  Examples include Notifications, Azure, AWS, Naming, and so on.
- Have application-specific library variable sets to share items such as service URLs, database connection strings, etc. across the multiple projects that make up an application.

<span><a class="btn btn-secondary" href="/docs/getting-started/best-practices/project-and-project-groups">Previous</a></span>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span><a class="btn btn-success" href="/docs/getting-started/best-practices/step-templates-and-script-modules">Next</a></span>