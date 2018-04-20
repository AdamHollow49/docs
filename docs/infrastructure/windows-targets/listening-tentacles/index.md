---
title: Listening Tentacles
description: Octopus Listening Tentacles wait for a connection from the Octopus server to be told what to do.
position: 30
---

<!-- all content replicated one level down -->

:::warning
**SSL Offloading is Not Supported**
The communication protocol used by Octopus and Tentacle requires intact end-to-end TLS connection for message encryption, tamper-proofing, and authentication. For this reason SSL offloading is not supported.
:::

:::warning
**Proxy Servers Supported for Tentacle Communications Since Octopus 3.4**
The communication protocol used by Octopus and Tentacle 3.4 and above supports proxies. Read more about configuring proxy servers for Tentacle communications in [proxy support](/docs/infrastructure/windows-targets/proxy-support.md).

If you are using a version of Octopus/Tentacle prior to 3.4 you will need to arrange a bypass/exception for traffic initiated from the Octopus server to the Tentacle on the configured TCP Port (port **10933** by default).
:::

## Installation {#ListeningTentacles-Installation}

:::success
**Download the Tentacle MSI**
The latest Tentacle MSI can always be [downloaded from the Octopus Deploy downloads page](https://octopus.com/downloads).
:::

This four minute video (with captions) will walk you through the process of installing a Tentacle in listening mode, and registering it with your [Octopus Deploy server](/docs/installation/index.md).
<iframe src="//fast.wistia.net/embed/iframe/qp12uky9qy" allowtransparency="true" frameborder="0" scrolling="no" class="wistia_embed" name="wistia_embed" allowfullscreen mozallowfullscreen webkitallowfullscreen oallowfullscreen msallowfullscreen width="640" height="360" style="margin: 30px"></iframe>

## Firewall Changes {#ListeningTentacles-Firewallchanges}

To allow your Octopus Deploy server to connect to the Tentacle, you'll need to allow access to TCP port **10933** on the Tentacle (or the port you selected during the installation wizard - port 10933 is just the default).

Using listening mode, you won't typically need to make any firewall changes on the Octopus Deploy server.

**Intermediary Firewalls**
Don't forget to allow access not just in Windows Firewall, but also any intermediary firewalls between the Octopus server and your Tentacle. For example, if your Tentacle server is hosted in Amazon EC2, you'll also need to modify the AWS security group firewall to tell EC2 to allow the traffic. Similarly if your Tentacle server is hosted in Microsoft Azure you'll also need to add an Endpoint to tell Azure to allow the traffic.
