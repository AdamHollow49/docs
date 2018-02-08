---
title: Octopus Server commands - External Worker
description: Creates a new certificate that Octopus server can use to authenticate itself with its Tentacles
---

Use the external-worker command to configure an [external worker](/docs/administration/workers#external-worker).

**external-worker options**

```text
Usage: Octopus.Server external-worker [<options>]

Where [<options>] is any of:

      --instance=VALUE       Name of the instance to use
      --reset                Reset the external worker configuration to its
                               factory default, which is to execute scripts
                               using the built in worker.
      --address=VALUE        The address of the external worker. e.g.
                               'https://localhost:10933' or 'http://exampl-
                               e.com:10933'
      --thumbprint=VALUE     The thumbprint of the external worker. e.g.
                               'B8FC0BFA1726B53D66B3E89CFC830E4DD4F83752'

Or one of the common options:

      --help                 Show detailed help for this command
```
