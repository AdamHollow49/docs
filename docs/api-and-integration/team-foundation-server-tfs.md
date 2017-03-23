---
title: Team Foundation Server (TFS)
description: Octopus Deploy integrates with Team Foundation Server to provide for a full automated build and deployment pipeline.
position: 2
---

Octopus Deploy integrates with Team Foundation Server to provide for a full automated build and deployment pipeline. This section provides information on integrating Octopus Deploy and TFS. The procedures on this page have been verified against Visual Studio 2013 but should also work with previous versions of TFS.

## Using the extension with Team Foundation Server

### Team Foundation Server (TFS) 2017 or above?

We have built a [comprehensive extension for Visual Studio Team Services (VSTS)](/docs/api-and-integration/visual-studio-team-services-vsts.md) (formerly Visual Studio Online or VSO) which also works with TFS 2017. We highly recommend using this extension for VSTS/VSO and TFS 2017 or newer.

### Team Foundation Server (TFS) 2015 Update 2 or 3

The marketplace extension for VSTS (above) is not compatible with TFS 2015. You can [download a version of the extension for TFS 2015 here](https://download.octopusdeploy.com/tfs-2015-extension/octopusdeploy.octopus-deploy-build-release-tasks-2.0.39.vsix).

### Upgrading Team Foundation Server and the Extension

You can safely upgrade from TFS 2015 to TFS 2017 with version 1.2.x of the extension without needing to modify your builds. The 1.2.x version of the extension will continue to work with TFS 2017, and you can upgrade individual tasks in build definitions when you're ready.

Upgrading from 1.2.x to version 2.x of the extension requires additional permissions to support the dashboard widget.

![](/docs/images/3048175/extension-upgrade.png)

![](/docs/images/3048175/extension-upgrade-2.png)

After approving the upgrade, builds can continue using the 1.2.x version of the extension, and you can upgrade the tasks in your build definitions whenever you're ready. Just choose version 2.x in the dropdown for the appropriate tasks.

![](/docs/images/3048175/extension-version-choice.png)

## Packaging applications when building with Team Build (XAML-based builds) {#TeamFoundationServer(TFS)-PackagingapplicationswhenbuildingwithTeamBuild}

When Team Build builds your solution, you will need to package your applications ready to be deployed. This can be done by [installing OctoPack](/docs/packaging-applications/nuget-packages/using-octopack/index.md) on the projects that you plan to deploy.

When defining your build definition, you can expand the **Advanced** properties to specify custom arguments for MSBuild. At a minimum, you'll need to pass:

```
/p:RunOctoPack=true
```

![](/docs/images/3048175/3278177.png)

:::success
**OctoPack**
There are plenty of other properties that you can pass here. For example, you can tell OctoPack to publish the resulting packages to a file share or another NuGet repository. See the "Publishing a new package to Octopus" section below, or [Learn more in the section on OctoPack](/docs/packaging-applications/nuget-packages/using-octopack/index.md).
:::

Publishing a new package to Octopus

In most cases, you'll want to push your newly-created package to the built-in Octopus Deploy NuGet repository. To do this, you can use two additional MSBuild arguments to tell OctoPack to publish the package(s).

```
/p:OctoPackPublishPackageToHttp=http://my-octopus-server/nuget/packages
```

```
/p:OctoPackPublishApiKey=API-ABCDEFGMYAPIKEY
```

![](/docs/images/3048175/3278173.png)

:::success
**Built-in Nuget and API keys**
To find your Octopus NuGet repository URL, see the [Package repositories](/docs/packaging-applications/package-repositories/index.md) section.

To create an API key, see the [How to create an API key](/docs/how-to/how-to-create-an-api-key.md) section.
:::

## Deploying automatically after a build {#TeamFoundationServer(TFS)-Deployingautomaticallyafterabuild}

Version 2.6 of Octopus Deploy introduced [Lifecycles ](/docs/key-concepts/lifecycles.md)and [Automatic Release Creation](/docs/deploying-applications/automatic-release-creation.md). You can use these two features to automatically deploy to one or more environments when a new package is pushed to the built-in NuGet repository.

First, turn on Automatic Release Creation to create a new release when your package is pushed. Then, using the project Lifecycle configure one or more environments in your first phase to deploy automatically when a new release is available.

By adding these features to the steps above, you can set up complete end-to-end continuous integration:

1. OctoPack will create new NuGet packages for your projects
2. OctoPack will push those packages to the built-in NuGet repository
3. Automatic Release Creation will create a new Release
4. Your Lifecycle will trigger a deployment when the release is created

## Walkthrough {#TeamFoundationServer(TFS)-Walkthrough}

This 6 minute video walks through the above steps to create an end-to-end continuous deployment process for Team Foundation Server.

<iframe src="//fast.wistia.net/embed/iframe/jmnuxifuyo" allowtransparency="true" frameborder="0" scrolling="no" class="wistia_embed" name="wistia_embed" allowfullscreen mozallowfullscreen webkitallowfullscreen oallowfullscreen msallowfullscreen width="640" height="360" style="margin: 30px"></iframe>

## See also {#TeamFoundationServer(TFS)-Seealso}

The following posts have more details on integrating Octopus Deploy with TFS:

- [Automated deployment with TFS Preview](https://octopus.com/blog/automated-deployment-with-tfspreview-octopack-myget), a complete end-to-end walkthrough
- [Automated deployment with TFS Team Build](https://octopus.com/blog/using-octopus-and-tfs-builds)
