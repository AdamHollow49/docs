---
title: Considerations for Highly Availaible Octopus on Azure
description: Information on configuring Octopus High Availability for Microsoft Azure.
position: 10
---

This section will walk through the different options and considerations for setting up Octopus High Availability in [Microsoft Azure](https://azure.microsoft.com/en-us/). This document will deal with the high-level concepts of Octopus High-Availability Microsoft Azure.

:::warning
If you are setting Octopus up on AWS or on-Premises please see the following guides:

- [AWS](/docs/administration/high-availability/configuring-octopus-for-high-availability-in-aws.md)
- [On-Premises](/docs/administration/high-availability/configuring-octopus-for-high-availability-on-premises.md)
:::

## Setting up Octopus: High availability

This section will walk you through the different options and considerations for setting up Octopus: HA. For this document's sake, the guide assumes that all of the servers are in Azure, as this is the most common configuration.

:::hint
**Some assembly required**
A single server Octopus installation is straightforward; Octopus High Availability is designed for mission-critical enterprise scenarios and depends heavily on infrastructure and Windows components. At a minimum:

- You should be familiar with SQL Server failover clustering or have DBAs available to create and manage the database
- You should be familiar with SANs or other approaches to sharing storage between servers
- You should be familiar with load balancing for applications
:::

### Compute

For a highly available Octopus configuration, you will need a minimum of two Virtual Machines in Azure. Still, there are several items to consider when provisioning your Octopus Virtual Machines in Azure:

- [Number and type of Deployment Targets](https://octopus.com/docs/administration/retention-policies/)
- [Retention Policies](https://octopus.com/docs/administration/retention-policies/)
- [Number of concurrent tasks](https://octopus.com/docs/support/increase-the-octopus-server-task-cap/)

Each organization has its Octopus environment to consider, and you can see the [Azure Virtual Machines](https://docs.microsoft.com/en-us/azure/virtual-machines/sizes-general) and select the best for your requirements.

### Database

Octopus uses a SQL Database that stores environments, projects, variables, releases, and deployment history. To host the Octopus SQL database in Azure, and there are two options that you should consider, and these are:

- [SQL Server on a Virtual Machine](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-iaas-overview/)
- [Azure SQL Database as a Service](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-technical-overview/)

Each Octopus Server node stores project, environment, and deployment-related data in a shared Microsoft SQL Server Database. Since this database is shared, the database server must also be highly available.

From the Octopus perspective, how the database is made highly available is really up to you; to Octopus, it's just a connection string. We are not experts on SQL Server high availability, so if you have an on-site DBA team, we recommend using them. There are many [options for high availability with SQL Server](https://msdn.microsoft.com/en-us/library/ms190202.aspx), and [Brent Ozar also has a fantastic set of resources on SQL Server Failover Clustering](http://www.brentozar.com/sql/sql-server-failover-cluster/) if you are looking for an introduction and practical guide to setting it up.

Octopus HA works with:

- [SQL Server Failover Clusters](https://docs.microsoft.com/en-us/sql/sql-server/failover-clusters/high-availability-solutions-sql-server)
- [SQL Server AlwaysOn Availability Groups](https://docs.microsoft.com/en-us/sql/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server)
- [Azure SQL Database](https://azure.microsoft.com/services/sql-database/)

Octopus HA has not been tested with Log Shipping or Database Mirroring and does not support SQL Server replication. [More information](/docs/administration/data/octopus-database/index.md)

See also: [SQL Server Database](/docs/installation/sql-server-database.md), which explains the editions and versions of SQL Server that Octopus supports and explains the requirements for how the database must be configured.

### Shared storage

Octopus stores several files that are not suitable to store in the database. These include:

- NuGet packages used by the [built-in NuGet repository inside Octopus](/docs/packaging-applications/package-repositories/index.md). These packages can often be substantial.
- [Artifacts](docs/projects/deployment-process/artifacts.md) collected during a deployment. Teams using Octopus sometimes use this feature to collect large log files and other files from machines during a deployment.
- Task logs are text files that store all of the log output from deployments and other tasks.

As with the database, from the Octopus perspective, you'll tell the Octopus Servers where to store them as a file path within your operating system. Octopus doesn't care what technology you use to present the shared storage; it could be a mapped network drive or a UNC path to a file share. Each of these three types of data can be stored in a different place.

Whichever way you provide the shared storage, a few considerations to keep in mind:

- To Octopus, it needs to appear as a mapped network drive (e.g., `D:\`) or a UNC path to a file share (e.g., `\\server\path`)
- The service account that Octopus runs needs **full control** over the directory
- Drives are mapped per-user, so you should map the drive using the same service account that Octopus is running under

If your Octopus Server is running in Microsoft Azure, you can use [Azure File Storage](https://docs.microsoft.com/en-us/azure/storage/files/storage-files-introduction) - it just presents a file share over SMB 3.0.

#### Azure Files

If your Octopus Server is running in Microsoft Azure, there is only one solution unless you have a [DFS Replica](https://docs.microsoft.com/en-us/windows-server/storage/dfs-replication/dfsr-overview) in Azure. That solution is [Azure File Storage](https://docs.microsoft.com/en-us/azure/storage/files/storage-files-introduction) presents a file share over SMB 3.0 that will is shared across all of your Octopus servers.

Once you have created your File Share, I find the best option is to add the Azure File Share as a [symbolic link](https://en.wikipedia.org/wiki/Symbolic_link) and then adding this to `C:\Octopus\` for the Artifacts, Packages, and TaskLogs which need to be available to all nodes.

Run the below before installing Octopus.

````PowerShell
# Add the Authentication for the symbolic links. You can get this from the Azure Portal.

cmdkey /add:octostorage.file.core.windows.net /user:Azure\octostorage /pass:XXXXXXXXXXXXXX

# Add Octopus folder to add symbolic links

New-Item -ItemType directory -Path C:\Octopus
New-Item -ItemType directory -Path C:\Octopus\Artifacts
New-Item -ItemType directory -Path C:\Octopus\Packages
New-Item -ItemType directory -Path C:\Octopus\TaskLogs

# Add the Symbolic Links. Do this before installing Octopus.

mklink /D C:\Octopus\TaskLogs \\octostorage.file.core.windows.net\octoha\TaskLogs
mklink /D C:\Octopus\Artifacts \\octostorage.file.core.windows.net\octoha\Artifacts
mklink /D C:\Octopus\Packages \\octostorage.file.core.windows.net\octoha\Packages
````

[Install Octopus](https://octopus.com/docs/installation) and then run the below.

````powershell
# Set the path
& 'C:\Program Files\Octopus Deploy\Octopus\Octopus.Server.exe' path --artifacts "C:\Octopus\Artifacts"
& 'C:\Program Files\Octopus Deploy\Octopus\Octopus.Server.exe' path --taskLogs "C:\Octopus\TaskLogs"
& 'C:\Program Files\Octopus Deploy\Octopus\Octopus.Server.exe' path --nugetRepository "C:\Octopus\Packages"
````

### Load Balancing in Azure

To distribute HTTP load among Octopus Server nodes with a single point of access, it is recommended to use an HTTP load balancer. We typically recommend using a round-robin (or similar) approach for sharing traffic between the nodes in your cluster.

Octopus Server provides a health check endpoint for your load balancer to ping: `/api/octopusservernodes/ping`

![](images/load-balance-ping.png "width=500")

Making a standard `HTTP GET` request to this URL on your Octopus Server nodes will return:

- HTTP Status Code `200 OK` as long as the Octopus Server node is online and not in [drain mode](#ManagingHighAvailabilityNodes-Drain).
- HTTP Status Code `418 I'm a teapot` when the Octopus Server node is online, but it is currently in [drain mode](#ManagingHighAvailabilityNodes-Drain) preparing for maintenance.
- Anything else indicates the Octopus Server node is offline, or something has gone wrong with this node.

:::hint
The Octopus Server node configuration is also returned as JSON in the HTTP response body.
:::

Any of the below Load Balancers support Octopus in a Highly-Available configuration.

- [Azure Traffic Manager](https://docs.microsoft.com/en-us/azure/traffic-manager/traffic-manager-overview)
- [Azure Application Gateway](https://docs.microsoft.com/en-us/azure/application-gateway/overview)
- [Azure Load Balancer](https://docs.microsoft.com/en-us/azure/load-balancer/load-balancer-overview)
- [Kemp LoadMaster](https://kemptechnologies.com/uk/solutions/microsoft-load-balancing/loadmaster-azure/)
- [F5 Big-IP Virtual Edition](https://www.f5.com/partners/technology-alliances/microsoft-azure)

### Authentication Provider(s)

We  recommend [Active Directory](https://en.wikipedia.org/wiki/Active_Directory) for most installations but for this to work in Azure you would need a domain controller setup locally in Azure for this to work. Please check [Authentication Providers](/docs/security/authentication/auth-provider-compatibility.md) for a full list of supported Authentication Providers.

If you are hosting in Azure with domain controllers, it would be a similar setup for [On-Premises](/docs/administration/high-availability/configuring-octopus-for-high-availability-on-premises.md)
