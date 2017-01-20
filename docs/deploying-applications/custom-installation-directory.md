---
title: Custom Installation Directory
position: 5
---


For [Package steps](http://docs.octopusdeploy.com/display/OD/Deploying+packages) & [IIS Steps](http://docs.octopusdeploy.com/display/OD/IIS+Websites+and+Application+Pools), the Custom Installation Directory feature allows you to have your package deployed to a specific location on the server. This feature helps when you are using something like a Content Management System (CMS) or some other coordinating application which requires files to reside in a certain physical location.

:::success
Only use the Custom Installation Directory feature when it is truly required. Out of the box, Octopus will usually do the right thing when deploying your package. You can read more about [how packages are deployed by convention](/docs/home/deploying-applications/deploying-packages.md), and the [order of each step in the process](/docs/home/reference/package-deployment-feature-ordering.md). The standard convention eliminates problems caused by file locks and stale files being left in the deployment folder. It also provides smoother deployments and less downtime for Windows Services and Web Applications.
:::





In your *Package Deploy* or *IIS* steps, look for the **Configure Features** link at the bottom


![](/docs/images/3048085/5865882.jpg)


Then select the feature **Custom Installation Directory**


![](/docs/images/3048085/3277679.png)


You can either specify the full path of the folder, or make use of a variable like shown below.


![](/docs/images/3048085/3277678.png)


The use of a variable means that you can scope different values to different environments.


![](/docs/images/3048085/3277677.png)


Our Packages are extracted into a new directory each time (along the lines of C:\Octopus\Applications\[Environment name]\[Package name]\[Package version]\) , and this is no different for Custom Installation Directory.


![](/docs/images/3048085/3277682.png)


We make the assumption that when you are using a Custom Installation Directory it has a working copy of an existing website or application. So to blindly extract the new package in your existing directory, without first completing any transformations, or variable substitutions could potentially break your application or website. So even when you are using the Custom Installation Directory feature, we extract the package to the above listed directory and perform all transformations and substitutions.


![](/docs/images/3048085/3277681.png)


And after substitution and transformation your files are moved.


![](/docs/images/3048085/3277680.png)


Read more about the [Ordering of Package Features](/docs/home/reference/package-deployment-feature-ordering.md).
