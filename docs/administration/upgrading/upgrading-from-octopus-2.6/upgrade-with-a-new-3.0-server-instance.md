---
title: Upgrade with a new 3.0 server instance
description: Information on how to upgrade from Octopus 2.6 to a new Octopus 3.0 instance.
position: 2
---

This is the recommended way of performing an upgrade for larger installations. It gives you the opportunity to verify that your Tentacles have been successfully upgraded, and allows you to more easily roll back if you have any issues.

Be sure to read the [Upgrading from Octopus 2.6](/docs/administration/upgrading/upgrading-from-octopus-2.6/index.md) documentation page. You must have a working 2.6 Octopus installation for the data migration.

## Summary {#Upgradewithanew3.0serverinstance-Summary}

!toc

## Step by step {#Upgradewithanew3.0serverinstance-Stepbystep}

To upgrade with a new Octopus 3.x server, follow these steps:

### 1. Back up your Octopus 2.6 database and master key {#Upgradewithanew3.0serverinstance-1.BackupyourOctopus2.6databaseandmasterkey}

See the [Backup and restore](/docs/administration/upgrading/upgrading-from-octopus-2.6/backup-2.6.md) page for instructions on backing up your database.

### 2. Install Octopus 3.x on a new virtual or physical server {#Upgradewithanew3.0serverinstance-2.InstallOctopus3.xonanewvirtualorphysicalserver}

:::success
**Upgrade to the latest version**
When upgrading to Octopus 3.x please use the latest version available. We have been constantly improving the 2.6 to 3.x data migration process whilst adding new features and fixing bugs.
:::

See the [Installing Octopus 3.x](/docs/installation/installing-octopus/index.md) page for instructions on installing a new Octopus 3.x instance.

### 3. Migrate your data from 2.6 to 3.x {#Upgradewithanew3.0serverinstance-3.Migrateyourdatafrom2.6to3.x}

See the [Migrating data from Octopus 2.6 to 3.x](/docs/administration/upgrading/upgrading-from-octopus-2.6/migrating-data-from-octopus-2.6-to-3.x.md) page for instructions on importing your Octopus 2.6 database backup into Octopus 3.x.

:::hint
**Migration taking a long time?**
By default we migrate everything from your backup including historical data. You can use the `maxage=` argument when executing the migrator to limit the number of days to keep. For example: `maxage=90` will keep 90 days of historical data ignoring anything older.

To see the command syntax click the **Show script** link in the wizard
:::

:::hint
**Using the built-in Octopus NuGet repository?**
If you use the built-in [Octopus NuGet repository](/docs/packaging-applications/package-repositories/index.md) you will need to move the files from your 2.6 server to your 3.x server. They are not part of the backup.
In a standard 2.6 install the files can be found under `C:\Octopus\OctopusServer\Repository\Packages`
You will need to transfer them to the new server to `C:\Octopus\Packages`Once the files have been copied, you will need to restart the Octopus Server service to re-index the files - The index runs in the background, so if you have a lot of packages it could take a while (5-20 mins) to show in the UI or be usable for deployments.
:::

### 4. Use Hydra to automatically upgrade your Tentacles {#Upgradewithanew3.0serverinstance-4.UseHydratoautomaticallyupgradeyourTentacles}

Hydra is a tool we've built that will help you update your Tentacles to the latest version. It is particularly useful migrating from 2.6 to 3.x as the communication methods have changed.

:::problem
This is the point of no return. When your Tentacles are upgraded to 3.x your 2.6 server will not be able to communicate with them.

We strongly recommend testing Hydra against a small subset of "canary" machines before upgrading the rest of your machines. The best way to do this is:

1. Create a new "canary" machine role and assign it to a few machines.
2. Set the Update Octopus Tentacle step to run on machines with the "canary" role.
3. Once you are confident the Tentacle upgrade works as expected, you can use Hydra to upgrade all of the remaining machines.
:::

#### How does Hydra work?

Hydra consists of two parts:

1. A package that contains the latest Tentacle MSI installers
2. An Octopus 2.6 step template that does the upgrade to your environments

To account for issues with communicating with a Tentacle that has been 'cut off' from its Octopus Server, the Hydra process connects to the Tentacle and creates a scheduled task on the Tentacle Machine. If it is able to schedule the task it considers that install a success. The task runs one minute later.

The scheduled task does the following:

1. Find Tentacle services
2. Stop all Tentacles (if they’re running)
3. Run MSI
4. Update configs for any polling Tentacles
5. Starts any Tentacles that were running when we started

With just one Tentacle service this should be a very quick process, but we cannot estimate how long it may take with many Tentacle services running on the one machine.

#### Common problems using Hydra

The scheduled task is set to run as `SYSTEM` to ensure the MSI installation will succeed. If your Tentacles are running with restricted permissions, they may not be able to create this scheduled task. **The only option is to upgrade your Tentacles manually.**

Hydra performs a Reinstall of each Tentacle. As part of the reinstall, the Service Account is reset to `Local System`. If you need your Tentacles to run under a different account, you will have to make the change after the upgrade completes (after you've re-established a connection from 3.x). You can do this manually, or using the following script:

```powershell
Tentacle.exe service --instance "Tentacle" --reconfigure --username=DOMAIN\ACCOUNT --password=accountpassword --start --console
```
#### Let's upgrade these Tentacles!

To use Hydra, follow these steps:

:::hint
These steps should be executed from your Octopus 2.6 server to your 2.6 Tentacles.
:::

1. Download the latest Hydra NuGet package from [https://octopus.com/downloads](https://octopus.com/downloads)
2. Use the Upload Package feature of the library to upload the OctopusDeploy. Hydra package to the built-in NuGet repository on your Octopus 2.6 server.

![](/docs/images/3048135/3278019.png "width=500")

3. Import the [Hydra step template](http://library.octopusdeploy.com/step-templates/d4fb1945-f0a8-4de4-9045-8441e14057fa/actiontemplate-hydra-update-octopus-tentacle) from the Community Library.

![](/docs/images/3048135/3278018.png "width=500")

4. Create a [new project](/docs/key-concepts/projects/index.md) with a single "Update Octopus Tentacle" step from the step template

 1. Ensure you choose or create a [Lifecycle](/docs/key-concepts/lifecycles.md)that allows you to deploy to all Tentacles.
 2. Ensure you set the Update Octopus Tentacle step to run for all appropriate Tentacles.
 3. If you are using any polling Tentacles, add the new Octopus 3.x server address (including the polling port) in the Server Mapping field.
 
:::hint
It is very important you get this value correct. An incorrect value will result in a polling Tentacle that can't be contacted by neither a 2.6 or 3.x server. Several different scenarios are supported:

1. A single Polling Tentacle instance on a machine pointing to a single Octopus Server **the most common case**:
  - Just point to the new server's polling address `https://newserver:newport` like `https://octopus3.mycompany.com:10934`
2. Multiple Polling Tentacle instances on the same machine pointing to a single Octopus Server:
  - Just point to the new server's polling address `https://newserver:newport` like `https://octopus3.mycompany.com:10934` and Hydra will automatically update all Tentacles to point to the new server's address
3. Multiple Polling Tentacle instances on the same machine pointing to different Octopus Servers **a very rare case**:
  - Use this syntax to tell Hydra the mapping from your old Octopus server to your new Octopus server: `https://oldserver:oldport=>https://newserver:newport,https://oldserver2:oldport2/=>https://newserver2:newport2` where each pair is separated by commas. This will match the first case and replace it => with the second case.
  
        Click the ![](/docs/images/3048132/3278017.png) help button for more detailed instructions.

![](/docs/images/3048132/3278014.png "width=500")

![](/docs/images/3048132/3278015.png "width=500")
:::

5. Create a release and deploy. The deployment should succeed, and one minute later the Tentacles will be upgraded.
    ![](/docs/images/3048132/3278010.png "width=500")

### 5. Verify connectivity between the 3.x server and 3.x Tentacles {#Upgradewithanew3.0serverinstance-5.Verifyconnectivitybetweenthe3.xserverand3.xTentacles}

When the Hydra task runs on a Tentacle machine, it should no longer be able to communicate with the Octopus 2.6 server. You can verify this by navigating to the Environments page and clicking **Check Health**.

![](/docs/images/3048132/3278012.png "width=500")

After successfully updating your Tentacles, you should see this check fail from your 2.6 server.

![](/docs/images/3048132/3278011.png "width=500")

Performing the Check Health on your Octopus 3.x server should now succeed.

![](/docs/images/3048132/3278009.png "width=500")

:::hint
If you have multiple Tentacles running on the same server, an update to one will result in an update to **all** of them. This is because there is only one copy of the Tentacle binaries, even with multiple instances configured.
:::

### 6. Decommission your Octopus 2.6 server {#Upgradewithanew3.0serverinstance-6.DecommissionyourOctopus2.6server}

Once you are confident your Tentacles have all been updated and work correctly, you can decommission your Octopus 2.6 Server.
