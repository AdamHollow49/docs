---
title: Installation
position: 20
description: How to install the Octopus Server.
hideInThisSection: true
---

!include <octopus-server>

## Self-hosted Octopus Server

When installed, the self-hosted Octopus Server:

- Runs
  - As a Windows Service called **OctopusDeploy**, installed via an MSI.
  - In a container, either:
     - A [Linux](docs/installation/octopus-server-linux-container/index.md) container (recommended)
     - A [Windows](docs/installation/octopus-server-windows-container.md) container *(deprecated)*
- Stores its data in an [SQL Server Database](/docs/installation/sql-server-database.md). ([SQL Server Express](http://downloadsqlserverexpress.com/) is an easy way of getting started.)
- Includes an embedded HTTP server which serves the [Octopus REST API](/docs/octopus-rest-api/index.md) and the  **Octopus Web Portal** that you will use to manage your [infrastructure](/docs/infrastructure/index.md), [deployments](/docs/projects/deployment-process/index.md), [runbooks](/docs/runbooks/index.md), and coordinate your [releases](/docs/releases/index.md).

Before you install Octopus Deploy, review the software and hardware [requirements](/docs/installation/requirements.md), and make sure you have access to an instance of [SQL Server Database](/docs/installation/sql-server-database.md) that you can use with Octopus Deploy.

## Install Octopus as a Windows Service {#install-octopus}

1. [Download](https://Octopus.com/downloads/server) the Octopus installer.
1. Start the Octopus Installer, click **Next**, accept the **Terms in the License Agreement** and click **Next**.
1. Accept the default **Destination Folder** or choose a different location and click **Next**.
1. Click **Install**, and give the app permission to **make changes to your device**.
1. Click **Finish** to exit the installation wizard and launch the **Getting started wizard** to configure your Octopus Server.
1. Click **Get started...** and either enter your details to start a free trial of Octopus Deploy or enter your **license key** and click **Next**.
1. Accept the default **Home Directory** or enter a location of your choice and click **Next**.
1. Decide whether to use a **Local System Account** or a **Custom Domain Account**.

Learn more about the [permissions required for the Octopus Windows Service](/docs/installation/permissions-for-the-octopus-windows-service.md) or using a [Managed Service Account](/docs/installation/managed-service-account.md).

9. On the **Database** page, click the drop-down arrow in the **Server Name** field to detect the SQL Server Database. Octopus will create the database for you which is the recommended process; however, you can also [create your own database](/docs/installation/sql-server-database.md#creating-the-database).
10. Enter a name for the database, and click **Next** and **OK** to **create the database**.

  Be careful **not** to use the name of an existing database as the setup process will install Octopus into that pre-existing database.

11. Accept the default port and directory or enter your own and click **Next**.
12. If you’re using **username and passwords stored in Octopus** authentication mode, enter the username and password that will be used for the Octopus administrator. If you are using [active directory](/docs/security/authentication/active-directory/index.md), enter the active directory user details.

  You can configure additional [Authentication Providers](/docs/security/authentication/index.md) for the Octopus Server after the server has been installed.

12. Click **Install**.

When the installation has completed, click **Finish** to launch the **Octopus Manager**.

### Octopus Manager

Before you launch the **Octopus Web Portal**, it's worth taking note of the other settings such as controlling the Octopus Windows Service, importing and exporting the data Octopus stores in the SQL server, and viewing the Master Key.

You can launch the Octopus Web Portal from the Octopus Manager, by clicking **Open in Browser**.

### Save your Master Key

Under the storage section, you will see a link to **View Master Key**.

When Octopus is installed, it generates a Master Key which is a random string that is used to encrypt sensitive data in your Octopus database. You will need the Master Key if you ever need to restore Octopus.

Make a copy of the Master Key and save it in a **secure** location.

:::warning
**Warning**

If you don't have a copy of your Master Key and your hardware fails, you will not be able to recover the encrypted data from the database. Make a copy of the **Master Key** and save it in a secure location. Hopefully you will never need it, but you'll be glad you have it if you ever do. Learn about [Recovering After Losing Your Octopus Server and Master Key](/docs/administration/managing-infrastructure/lost-master-key.md).
:::

## Run Octopus Server in a Container {#run-octopus-in-container}

!include <octopus-server-in-container>

This section includes information about different options to run the Octopus Server Linux Container, and migration considerations when moving to the Octopus Server Linux Container From Windows Server or the *deprecated* Windows Container.

- [Octopus Server Linux Container](/docs/installation/octopus-server-linux-container/index.md)
- [Octopus Server Container with Docker Compose](/docs/installation/octopus-server-linux-container/docker-compose-linux.md)
- [Octopus Server Container with systemd](/docs/installation/octopus-server-linux-container/systemd-service-definition.md)
- [Octopus Server Container in Kubernetes](/docs/installation/octopus-server-linux-container/octopus-in-kubernetes.md)
- [Migrating to the Octopus Server Linux Container](/docs/installation/octopus-server-linux-container/migration/index.md)

## Launch the Octopus Web Portal

Click **Open in browser** to launch the **Octopus Web Portal** and log in using the authentication details you set up during the configuration process.

The **Octopus Web Portal**  is where you'll manage your infrastructure, projects, deployment process, access the built-in repository, and manage your deployments and releases.

## Releases of Octopus Deploy Server

!include <octopus-releases>

## Learn more

 - [Troubleshooting the Octopus installation](/docs/installation/troubleshooting.md)
 - [Configure your infrastructure](/docs/infrastructure/index.md)
 - [Upgrading guide](/docs/administration/upgrading/index.md)
 - [Automating Octopus installation](/docs/installation/automating-installation.md)

