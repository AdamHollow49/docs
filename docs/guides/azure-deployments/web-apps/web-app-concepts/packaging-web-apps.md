---
title: Packaging Web Apps

---


In order to deploy Web Apps they must be packaged into an Octopus compatible NuGet package.

## Publish your Web App


Publish your Web App with Visual Studio to the file system:


![](/docs/images/3049436/3278570.png)

## Generate a NuGet package


Octopus requires a Web Apps to be packaged in NuGet package for use by Octopus. The easiest way generate a NuGet package is to use the [Octo.exe](/docs/home/packaging-applications/nuget-packages/using-octo.exe.md)�command line tool:

**Packaging a Cloud Service with Octo.exe**

```powershell
Octo.exe pack --id=HelloWeb --basePath=C:\PathToWebApp
```


Octo.exe will generate a NuGet package containing your Web App:


![](/docs/images/3049436/3278571.png)


Here is a sample Web App NuGet package:�[HelloWeb.1.0.0.nupkg](https://download.octopusdeploy.com/demo/HelloWeb.1.0.0.nupkg)

## Upload to a NuGet feed


In order to make the NuGet package accessible to Octopus it needs to be uploaded to a�[package repository](/docs/home/packaging-applications/package-repositories.md). The built-in Octopus package repository is accessible from Library > Packages and is a suitable place to upload your Web App NuGet package:


![](/docs/images/3049436/3278572.png)
