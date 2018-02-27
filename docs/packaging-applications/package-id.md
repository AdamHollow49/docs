---
title: Package ID
description: Choose an ID for your package
position: 2
---

The combination of package ID and [version number](versioning-in-octopus-deploy.md) uniquely identify a package.

Package identifiers in Octopus follow similar rules to [NuGet](https://docs.microsoft.com/en-us/nuget/create-packages/creating-a-package#choosing-a-unique-package-identifier-and-setting-the-version-number).

They must conform to the following specification:

- Package IDs consist of one or more segments separated by one of the following separator characters: `-` `.` `_`.

- Segments contain only alphanumeric characters.

- Must be unique within your Octopus Deploy instance.

Additionally, avoid using numbers in your package ID. This is because the package ID and version number will be combined like so:

> `<id>.<version>.<extension>`

For instance:

> `Sample.Web.2.3.tar.gz`

Including numbers in the package ID will result in the version being incorrectly parsed.

## Best Practice

We recommend naming your packages similar to namespaces in .NET. i.e. Pascal-cased and dot-separated.

Recommended examples include:

- Acme
- Acme.Billing.Web

The following examples are also valid:

- acme
- acme-billing.web
- Acme_Billing.Web

Learn about [versioning in Octopus Deploy](/docs/packaging-applications/versioning-in-octopus-deploy.md).
