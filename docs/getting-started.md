---
title: Getting Started
description: A Conceptual Overview of Octopus Deploy.
position: 0
---

Welcome! 

This section provides an overview of Octopus Deploy by explaining how Octopus Deploy fits into your application delivery pipeline. It also provides an overview of the major concepts in Octopus and links to the relevant documentation, which explore the concepts further and guides you through implementing them with your own Octopus. 

!toc

## Octopus in Your Delivery Process {#Gettingstarted-Octopusinyourdeliveryprocess}

Octopus Deploy is an automated deployment server that makes it easy to automate the deployment of ASP.NET web applications, Java applications, database updates, NodeJS application, and custom scripts into development, test, and production environments.


!!!! Rework, not just tentacles.
The Octopus Deploy server works together with lightweight agent services, called Tentacles, that are installed on the machines you plan to deploy your web apps, applications, or services to. With Octopus and Tentacle, you can easily deploy to your own servers or cloud services from providers like Amazon Web Services or Microsoft Azure.

### The Delivery Pipeline

We designed Octopus Deploy to fit into teams that follow agile delivery practices. A typical workflow would be:

1. Commit Code to Your Existing Source Control System.

   You might be using Git, Team Foundation Server, Subversion, or Mercurial; the choice is yours.

1. Your CI/Build Server Compiles the Code and Runs Unit Tests.

   You might be using TeamCity, Jenkins, Bamboo, Team Foundation Server, or CruiseControl.NET, again, the choice is yours.

1. Package Your Application.

   When the build is ready, your CI/build server bundles all of the files (binaries, images, scripts, configuration files, and so on) needed to deploy your application into a supported package.

1. Octopus Deploy Pushes Your Packaged Application.

   Octopus deploys the applications to the environments you've configured. These could be testing, dev, or production environments. 

### Consistent Releases

As an Octopus user, you define the process for deploying the software. You specify the environments the applications are deployed to and who on your team can deploy to which environments; for instance, you might want testers to deploy to test environments, but not to production. Taking this approach means that even if different members of the team are triggering deployments, the deployment process remains consistent.

The rest of this guide goes into more detail about working with  Octopus Deploy and links to the relevant sections of the documentation for more information.

## Install Octopus Deploy

Installing [Octopus Deploy](/docs/installation/index.md) sets up the central [Octopus Deploy Server](/docs/installation/installing-octopus/index.md), which provides the **Octopus Web Portal** and the [Octopus REST API](/docs/api-and-integration/octopus-rest-api.md).

The [installation documentation](/docs/installation/index.md) provides the instructions for installing and configuring your Octopus.

### Access the Octopus Web Portal

Once Octopus is installed you can access the Octopus Web Portal. This is where you'll manage your infrastructure, projects, access the built-in repository, and deploy your applications from.

![Octopus web portal sign in](octopus-login.png "width=500")

## Configure Your Infrastructure

### Environments

Octopus organizes your infrastructure into environments. Environments are groups of machines that you deploy to at the same time; for instance, you might have an environment of test servers that you deploy your apps to as part of your QA process, a UAT environment, and a production environment. Grouping machines in this way lets you define your deployment processes (no matter how many machines are involved) and have Octopus deploy the right versions of your apps to the right environments at the right time.

:::info
Throughout this guide we refer to both your servers and the cloud services you deploy your applications to as **machines**. 
:::

Learn how to add and manage environments in the [environments documentation](/docs/deployment-targets/environments/index.md).

### Machine Roles

Each environment that you work with, for instance, Dev, Test, or Production, will likely have multiple machines. In production, your web sites and Windows Services might run on different physical servers; perhaps dozens of them. However, unless you are extremely lucky, it's unlikely that you have the budget to have an equal number of servers in any of your pre-production environments. When testing your software in a test environment, you might only have a single virtual machine which will run all of the web sites and services on the same machine.

Octopus handles these differences by applying roles to the machines within your environments.

![](sample-environments.png)

Instead of saying:

> The trading website ASP.NET application should be deployed to PWEB01, PWEB02, ...

We say:

> The trading website ASP.NET application should be deployed to machines that are tagged with the **web-server** role.

In production, perhaps you have 10 machines with the web-server role. In staging, perhaps you have only 4. In test, perhaps there is a single machine. Roles make defining your deployment process much easier.

You can define as many environments, machines, and roles as you need; it all depends on how your applications are deployed.

Learn more about [machine roles](/docs/deployment-targets/machine-roles/index.md).

### Deployment Targets

Deployment targets represent the servers, virtual machines, and cloud services where your application and services will be deployed. The central Octopus Deploy server communicates with  different types of deployment targets in different ways.

Learn more about [deployment targets](/docs/deployment-targets/index.md).

#### Windows Targets

For Windows systems, we have tentacles. Tentacles are a secure, lightweight agent service that Octopus uses to deploy software to your Windows infrastructure.

Learn more about installing and configuring [Tentacles](/docs/installation/index.md) .

#### SSH Targets

For Linux and Unix systems, you can use configure Octopus Deploy to communicate with your deployment targets through SSH.

Learn more about configuring [SSH Targets](/docs/deployment-targets/ssh-targets/index.md).

#### Offline Package Drop

For scenarios where it is not possible to connect directly with the deployment target, Octopus can be configured to bundle all of the files needed to perform the deployment on the target server. These bundles can be copied directly to the target server to execute the deployment.

Learn more about [Offline Package Drops](/docs/deployment-targets/offline-package-drop.md).

#### Azure Cloud Services

Octopus Deploy includes steps that let you deploy your applications to the Azure Cloud Service.

Learn more about [deploying Azure Web Apps](/docs/guides/azure-deployments/cloud-services/cloud-service-concepts.md).

## Packaging Applications

Before you can deploy applications with Octopus, you need to package all the files needed for the application into a supported package and version the package. Octopus supports the following package types:

* NuGet
* Tar
* Tar Gzip
* Tar bzip2
* Zip
* Docker Image

Learn more about [packaging applications](/docs/packaging-applications/index.md)

### Versioning Packages

Versioning is an important part of how Octopus Deploy automates application deployment. Octopus recommends using [Semantic Versioning](http://semver.org) for your applications. 

Learn more about [Versioning in Octopus Deploy](/docs/packaging-applications/versioning-in-octopus-deploy.md).

### Uploading Packages 

Octopus Deploy includes a built-in package repository. We recommend configuring your existing tool chain to push packages automatically to the built-in repository; however, you can push packages manually to the repository if you choose to.

![Octopus built-in package repository](library-packages.png)

Learn more about the [package repository](/docs/packaging-applications/package-repositories/index.md) or how to automate your existing tool chain to push packages to your Octopus Deploy server with our [API and Integrations](/docs/api-and-integration/index.md).

## Deploying Your Applications

### Deployment Process

Because Octopus is designed to work with teams following agile software development methodologies, that is, continuously deploying software to environments, getting feedback, making changes, and redeploying, Octopus deploys software projects repeatedly.

After the initial setup, and with some tweaking, your deployment process shouldn't change between all of these deployments. Of course, the software that you're deploying will. You will make changes to code, commit them to source control, and have a [build server](/docs/api-and-integration/index.md) build them and run tests. Then the software will be [packaged](/docs/packaging-applications/index.md) and ready for deployment.

### Defining your Deployment Process

When you define your deployment process, you add different steps to be run in a specific order. Steps can be set to run automatically, or if certain conditions are met, or even to run parallel with each other. Octopus Deploy provides a range of built-in step templates:

* Deploy to IIS
* Deploy a Windows Service
* Deploy a Package
* Run a Script
* Deploy an Azure Cloud Service
* Deploy an Azure Web App
* Deploy a Service Fabric App
* Run a Service Fabric SDK PowerShell Script
* Run an Azure PowerShell Script
* Deploy an Azure Resource Group
* Create a Docker Network
* Run a Docker Container
* Stop a Docker Resource
* Send an Email
* Health Check
* Transfer a Package
* Manual Intervention Required
* Deploy a VHD
* Deploy a Java Archive
* Deploy to Tomcat Via Manager
* Start/Stop App in Tomcat
* Deploy to WildFly or EAP
* Enable/Disable Deployment in WildFly or EAP
* Import Certificate

In addition to the built-in steps, there are also community contributed steps which are available to install.

Learn more about [deploying applications](/docs/deploying-applications/index.md) and [adding steps](/docs/deploying-applications/adding-steps.md).

### Variables

As you deploy your applications, you'll need to change their configuration files based on the scope of the deployment. Octopus has advance support for managing and scoping variables. 

Learn more about [variables](/docs/deploying-applications/variables/index.md).

### Releases

A release in Octopus, is a snapshot of the packaged software, variables, and the deployment process.

A release is deployed to the environments defined in the deployment process. Typically, releases are deployed to one environment and then promoted to the next environment when they are successful.

Each time you have a new candidate build that is ready to test, you'll create a release. When you apply a release to an environment, that is referred to as a deployment.

### Projects

Octopus Deploy can manage the deployment of many applications across your organization. Projects within Octopus Deploy let you manage multiple software projects across different environments with deployment processes (the specific deployment steps) defined per project.

A project in Octopus can consist of many deliverable components (e.g., web sites, micro services, and database scripts). It's usually helpful to think of Octopus projects in terms of business projects: if you have 5 developers working together on the "HR Portal rewrite" project, then that's probably a single project in Octopus.

Learn more about [projects](/docs/deploying-applications/projects/index.md).

### Lifecycle

When you define a project, you also select a lifecycle. The lifecycle defines the rules around how releases of the project are deployed between environments.

Lifecycles are defined by phases, each phase can have one or more environments, and each environment can be defined as an automatic deployment environment or a manual deployment environment. Each phase can have a set number of environments that must be released to before the next phase is available for deployment.

Learn more about [lifecycles](/docs/deploying-applications/projects/lifecycles/index.md).

## Channels

When you start working with Octopus, you will typically create releases from your main source code branch that are considered to be release candidates for your final production environment. Over time you may find you want to start working on an experimental branch of code, perhaps to introduce a new feature, or an entirely new version of your software. In this case you can either create an entirely new project, or clone the existing project, to manage deployments of this experimental software - but that leads to a lot of possible duplication and rework. In Octopus 3.2 we introduced the concept of [channels](/docs/deploying-applications/projects/channels.md) which let you modify the entire deployment process on a per-release basis, all within the same project. For example, you can promote releases created from your main code branch through to your production environment, but restrict releases created from your experimental feature branch to a special test environment perhaps with extra steps and variables.

Learn more about [channels](/docs/deploying-applications/projects/channels.md).

## Tenants

Over time your software may become so successful that you on-sell it to some external customers, and due to the way the software is architected, you need to deploy a copy of the software once per customer. You could achieve this in Octopus by creating an environment-per-customer, or even a project-per-customer, but this leads to duplication and unnecessary complexity. In Octopus 3.4 we introduced the concept of tenants that you can manage alongside your existing projects and environments.

Learn more about tenants in our [Multi-tenant Deployments Guide](/docs/guides/multi-tenant-deployments/index.md). 
