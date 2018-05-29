---
title: How to Run Steps on the Octopus Server
description: How to run steps directly on the Octopus Server in scenarios where they don't need to be run on a deployment target.
position: 21
---

Sometimes you will want to run a script on your Octopus Server itself. Perhaps you want to configure your Octopus Server in some way. Maybe you want to deploy a package onto your Octopus Server. Maybe you just want to run a script somewhere without the bother of setting up another machine. All of these are possible, with one caveat: be careful about the security implications!

## Running custom scripts on your Octopus Server

A default installation of Octopus Server comes with a built-in worker allowing you to [run custom scripts](/docs/deployment-examples/custom-scripts/index.md) on the Octopus Server. You can configure your worker to run under a different user account, disable the built-in worker altogether, or delegate to an external worker.

Learn more about [workers](/docs/administration/workers/index.md).

## Deploying packages to your Octopus Server

If you want to deploy a package on your Octopus Server, you should [install a Tentacle on your Octopus Server](#install-tentacle) and treat it just like any other deployment target.

Tentacle lets you run tasks in a flexible way. You can configure your Tentacle service to run under a different user account, for example. In fact, you could have one [Tentacle instance](/docs/administration/managing-multiple-instances.md) for your pre-production steps, and another for production steps, running under different user, all on the same machine.

An analogy is to think about the way build agents in TeamCity or TFS work. You can't make the TeamCity server or TFS server arbitrarily run scripts during the build. But you can install the build agent service on the same server as your TeamCity/TFS server, and it has the same effect, but with more flexibility.

### Install Tentacle on the Octopus Server{#install-tentacle}

1. Follow the steps to download and [install Tentacles](/docs/infrastructure/windows-targets/index.md) on the Octopus Server.
2. Configure the Tentacle in [listening mode](docs/infrastructure/windows-targets/index.md#configure-a-listening-tentacle-recommended).
3. Register the Tentacle so that it appears in your [Environments](/docs/infrastructure/environments/index.md) tab.
4. Assign the machine to all of your applicable environments, and give it a role like `octopus-server`.
5. When configuring your step, you can now choose the `octopus-server` role as your target role.
