---
title: Deploying ASP.NET Core Web Applications
position: 1
---


ASP.NET Core is the future of ASP.NET, and it contains many changes to how applications are built, and how they are run.

## Publishing and Packing the Website


Once you have a project up and running (see the [getting started guide](https://docs.asp.net/en/latest/getting-started.html)), it need to be published and packed

```powershell
# Publish the application to a folder
dotnet publish source/MyApp.Web --output published-app --configuration Release

# Package the folder into a ZIP
octo pack --id MyApp.Web --version 1.0.0 --basePath published-app 
```


If you are using the built-in repository, you can create a�[zip file](/docs/home/packaging-applications/creating-zip-packages.md)�instead.�The generated nupkg or zip file should then be then be [pushed to a repository](/docs/home/packaging-applications/package-repositories.md).


If you are using TeamCity, you can use the�[new TeamCity plugin for dotnet commands](https://github.com/JetBrains/teamcity-dnx-plugin).

## Deployment


ASP.NET Core applications can either run as a command line program with Kestrel, or under IIS ([which also uses Kestrel - check out the book for details](https://leanpub.com/aspnetdeployment)).

:::hint
See the [ASP.NET Core IIS documentation](https://docs.asp.net/en/latest/publishing/iis.html#install-the-http-platform-handler) for instructions on setting up IIS for ASP.NET Core
:::


When running under IIS,�ensure the Relative home directory is�specified�as "wwwroot" for compatibility with IIS.


![](/docs/images/3702900/3964976.png)




## Configuration


ASP.NET Core introduces a new JSON-based configuration file format. Octopus 3.3 introduced a new convention which can be used to update these JSON configuration files with Octopus variables


![](/docs/images/3702900/5275655.png)


![](/docs/images/3702900/5275656.png)


For more information, see the section on the�[JSON Configuration Variables Feature](/docs/home/guides/deploying-asp.net-core-web-applications/json-configuration-variables-feature.md).
