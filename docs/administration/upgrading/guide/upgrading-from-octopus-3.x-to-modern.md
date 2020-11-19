---
title: Upgrading from Octopus 3.x to the latest version
description: Information on how to upgrade from Octopus Deploy 3.x to the latest version
position: 6
---

It is possible to do an in-place upgrade of 3.x to the latest version of Octopus Deploy.  With that said, the last version of 3.x, 3.17.14, was released on November 12th, 2017.  Fundamentally, Octopus is almost an entirely different product.  We did our best to maintain backward compatibility, but there is a risk a hyper-specific scenario was missed or a breaking change was introduced.  Here is an example of changes made to Octopus Deploy since 3.17.14 was released.

- The majority of endpoints in the API can accept a `Space-Id`, for example `/api/Spaces-1/projects?skip=0&take=100` whereas before it was `/api/projects?skip=0&take=100`.  If a `Space-Id` isn't supplied, the default space is used.
- Teams can be assigned to multiple roles and spaces.  Before, a team could be assigned to only one role.
- Unique internal package feed per space.  Each space has a subfolder in the `Packages` directory to keep them segregated on the file system.  Before, a package would be located at `C:\Octopus\packages\MyPackage.2020.1.1.zip`.  Now it is `C:\Octopus\packages\Spaces-1\MyPackage.2020.1.1.zip`
- Almost every table in the database had a `Space-Id` column added to it.
- Workers were introduced.
- Azure Management APIs were depreciated.
- Support for Kubernetes was introduced.
- Terraform support was introduced.
- Execution containers running on docker on workers were introduced.

There is some risk involved with doing an in-place upgrade.  This guide will walk through the steps needed to reduce the risk and keep downtime to a minimum.

## Recommended Approach - Create a cloned instance

The recommended approach is to create a cloned instance, upgrade that, and test out the new functionality with any integrations.  From there, you can migrate over to the cloned instance or do an in-place upgrade of your existing instance and use the cloned instance for testing future upgrades.  

### Overview 

Creating a clone of an existing instance involves:

1. Backup master key and license.
1. Enable maintenance mode on the main instance.
1. Backup the database of the main instance.
1. Disable maintenance mode on the main instance.
1. Restore the backup of the main instance's database as a new database on the desired SQL Server.  
1. Download the same version of Octopus Deploy as your main instance.
1. Installing that version on a new server and configure it to point to the existing database.
1. Copying all the files from the backed up folders from the source instance.
1. Optional: Disabling target.
1. Upgrade cloned instance.
1. Test cloned instance.  Verify all API scripts, CI integrations, and deployments work.
1. If migrating, then migrate over.  Otherwise, leave the test instance alone, backup the folders and database, and upgrade the main instance.

!include <upgrade-octopus-backup-master-key>
!include <upgrade-octopus-backup-database>
!include <upgrade-restore-backup>
!include <upgrade-download-same-version>
!include <upgrade-install-cloned-version>
!include <upgrade-copy-files-for-cloned-instance>
!include <upgrade-disable-targets-cloned-instance>
!include <upgrade-inplace-upgrade>
!include <upgrade-testing-upgraded-instance>
!include <upgrade-migrating-instances>
!include <upgrade-octopus-backup-folders>
!include <upgrade-main-instance-after-test-instance>

## Alternative Approach - Create a test instance

Creating a cloned instance is quite a bit of work.  You have to worry about drift and getting new compute resources allocated.  An alternative approach to the cloned instance is creating a test instance with only a handful of projects.  Test the upgrade with that test instance and then do the upgrade of your main instance.

### Overview

The steps for this are:

1. Backup master key and license
1. Download the same version of Octopus Deploy as your main instance.
1. Install Octopus Deploy on a new VM.
1. Export a subset of projects from the main instance.
1. Import that subset of projects to the test instance.
1. Download the latest version of Octopus Deploy.
1. Backup the test instance database.
1. Upgrade that test instance to the latest version of Octopus Deploy.
1. Test and verify the test instance.  
1. Enable maintenance mode on the main instance.
1. Backup the database on the main instance.
1. Backup all the folders on the main instance.
1. Do an in-place upgrade of your main instance.
1. Test upgraded main instance.
1. Disable maintenance mode.

!include <upgrade-octopus-backup-master-key>
!include <upgrade-download-same-version>
!include <upgrade-install-test-version>
!include <upgrade-export-import-test-projects>
!include <upgrade-download-latest-version>
!include <upgrade-octopus-backup-database>
!include <upgrade-inplace-upgrade>
!include <upgrade-testing-upgraded-instance>
!include <upgrade-octopus-backup-database>
!include <upgrade-octopus-backup-folders>
!include <upgrade-main-instance-after-test-instance>
!include <upgrade-high-availability>

## Rollback Failed Upgrade

While unlikely, an upgrade may fail.  It could fail on a database upgrade script, SQL Server version is no longer supported, license check validation, or plain old bad luck.  Depending on what failed, you have a decision to make.  If the cloned instance upgrade failed, it might make sense to start all over again.  Or, it might make sense to roll back to a previous version.  In either case, if you decide to roll back, the process will be:

1. Restore the database backup.
1. Restore the folders.
1. Download and install the previously installed version of Octopus Deploy.
1. Do some sanity checks.
1. If maintenance mode is enabled, disable it.

!include <upgrade-restore-backup>
!include <upgrade-rollback-folders>
!include <upgrade-find-previous-version>

## Additional items to note

Earlier versions of 3.x, including 3.1, 3.4, and 3.5, also carry some additional caveats to make a note of.  Before upgrading to a modern version of Octopus Deploy, please keep these in mind.

###  Upgrading to Octopus 3.1 or greater {#UpgradingfromOctopus3.x-UpgradingTo31UpgradingtoOctopus3.1orgreater}

Summary: Tentacle was upgraded from .NET 4.0 to .NET 4.5 to enable TLS 1.2.

:::success
**You can upgrade to Octopus Server 3.1 without upgrading any Tentacles and get all of the new 3.1 deployment features because Calamari will continue to work on both Tentacle 3.0 and 3.1.**
:::

This is the first modern version of Octopus Server where there has been a Tentacle upgrade, and it has caused some confusion. This section aims to answer some of the most commonly asked questions about upgrading to Octopus 3.1 and the impact on Tentacles.

**Am I required to upgrade to Tentacle 3.1?**
No, you aren't required to upgrade to Tentacle 3.1. Tentacle 3.0 will still work and benefit from Calamari's latest version and all of the deployment features we shipped in **Octopus 3.1**.

**What changed with Tentacle 3.1?**
The Octopus-Tentacle communication protocol in 3.1 can use TLS 1.2, which requires .NET 4.5 to be installed on the server.

**When should I upgrade to Tentacle 3.1?**
We recommend upgrading to Tentacle 3.1 as soon as you are able. Upgrading Tentacles in **Octopus 3.1** is automated and can be done through the Environments page. The main benefit you'll get is the Octopus-Tentacle communication protocol can use TLS 1.2.

**What would stop me from upgrading to Tentacle 3.1?**
[Your server needs to support .NET 4.5](https://msdn.microsoft.com/en-us/library/8z6watww%28v=vs.110%29.aspx). Tentacle 3.1 requires .NET 4.5 to be installed on the server, which is what enables TLS 1.2 support, and .NET 4.5 is supported on Windows Server 2008 SP2 or newer. This means Windows Server 2003 and Windows Server 2008 SP1 are not supported for Octopus Server or Tentacle 3.1.

**How can I make Octopus/Tentacle use TLS 1.2 instead of TLS 1.0?**
Octopus Server and Tentacle to 3.1 will use TLS 1.2 by default. **Tentacle 3.0** will still work with **Octopus 3.1**, but the communication protocol will fall back to the lowest-common-denominator of TLS 1.0.

**Can I have a mixture of Tentacle 3.0 and 3.1? I'm not ready to upgrade some of my application servers.**
Yes, you can have a mixture of **Tentacle 3.0** and **3.1** working happily with **Octopus 3.1**. We have committed to maintaining compatibility with the communication protocol.

**If I keep running Tentacle 3.0 does that mean I won't get any of the new Octopus 3.1 deployment features?**
The deployment features are handled by Calamari and Octopus Server makes sure all Tentacles have the latest Calamari. This means servers hosting **Tentacle 3.0** or **3.1** will get all of the new deployment features we shipped with **Octopus 3.1** by means of the latest Calamari.

**Will you continue to support Windows Server 2003 or Windows Server 2008 SP1?**
No, from **Octopus 3.1** onward, we are dropping official support for Octopus Server and Tentacle hosted on Windows Server 2003 or Windows Server 2008 SP1.

:::hint
**Tentacle communications protocol**
Read more about the [Octopus - Tentacle communication](/docs/security/octopus-Tentacle-communication/index.md) protocol and [Troubleshooting Schannel and TLS](/docs/security/octopus-Tentacle-communication/troubleshooting-schannel-and-tls.md).
:::

### Upgrading to Octopus 3.4 or greater {#UpgradingfromOctopus3.x-UpgradingtoOctopus3.4orgreater}

See the [Release Notes](https://octopus.com/downloads/compare?from=3.3.27&amp;to=3.4.0) for breaking changes and more information.

**Using TeamCity NuGet feeds?** You will need to upgrade your TeamCity server to v9.0 or newer and [enable the NuGet v2 API](https://teamcity-support.jetbrains.com/hc/en-us/community/posts/206817105-How-to-enable-NuGet-feed-v2). **Octopus 3.4**+ no longer supports the custom NuGet v1 feeds from TeamCity 7.x-8.x. We recommend upgrading to the latest TeamCity version available due to continual improvements in their NuGet feed - or switch to using the [Octopus built-in repository](/docs/packaging-applications/package-repositories/index.md).

**Want to use SemVer 2 for packages or releases?** You will need to upgrade OctoPack and/or Octopus CLI to 3.4 or newer.

### Upgrading to Octopus 3.5 or greater {#UpgradingfromOctopus3.x-UpgradingtoOctopus3.5orgreater}

Some server configuration values are moved from the config file into the database in 3.5+.

If you are upgrading to a 3.5+ version, please backup your server config file prior to upgrading. If you need to downgrade, then replace the config with the original file after the downgrade and restart the Octopus Server.

## Troubleshooting {#UpgradingfromOctopus3.x-Troubleshooting}

In a few cases, a bug in a 3rd party component causes the installer to display a "Installation directory must be on a local hard drive" error. If this occurs, running the install again from an elevated command prompt using the following command (replacing Octopus.3.3.4-x64.msi with the name of the installer you are using):

`msiexec /i Octopus.3.3.4-x64.msi WIXUI_DONTVALIDATEPATH="1"`