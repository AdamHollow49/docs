﻿---
title: Node on *Nix deployments
position: 3
---


:::hint
This guide depends on features shipped with **Octopus 3.3** which makes packaging and deploying Node.js apps much simpler.
:::


As Octopus Deploy expands its capabilities beyond the standard Microsoft world, its easier than ever to deploy non .Net projects to non Windows platforms. From Octopus 3.3 onwards you are able to package up your projects using [other formats in addition to the current NuGet style](/docs/home/packaging-applications/supported-packages.md).


In this guide we will go through the process of packaging up a Node.js project into a tarball and deploying to a Linux based target over a SSH connection. Any part of this guide can be attempted on its own in conjunction with other project or target types, however this aims to provide an end-to-end example of one particular set up. Please note that these pages are not intended as an "ultimate guide to Linux" and are only an introductory guide to show how you can quickly get started today with a simple deployment scenario.


The guide broken down into several bite-sized portions:


- [Configuring Target Machine](/docs/home/guides/node-on-nix-deployments/configuring-target-machine.md)
- [Create & Push Node.js Project](/docs/home/guides/node-on-nix-deployments/create-&-push-node.js-project.md)
- [Configure Octopus Deploy Project](/docs/home/guides/node-on-nix-deployments/configure-octopus-deploy-project.md)
