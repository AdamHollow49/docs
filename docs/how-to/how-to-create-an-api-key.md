---
title: How to create an API key
position: 7
---


API keys allow you to access the Octopus Deploy [REST API](/docs/home/api-and-integration/octopus-rest-api.md) and perform tasks such as creating and deploying releases. API keys can be saved in scripts or external tools, without having to use your username and password. Each user and service account can have multiple API keys.

## Creating an API key


You can create API keys by performing the following steps:

1. From the Octopus Deploy web portal, sign in, and view your profile:
![](/docs/images/3048149/3278114.png)
2. Go to the API keys tab. This lists any previous API keys that you have created.�
![](/docs/images/3048149/3278113.png)
3. Click on **New API key**, and give the API key a name that you can use to remember what the key was for.�
![](/docs/images/3048149/3278112.png)
4. Click **Generate new**, and copy the new API key to your clipboard:
![](/docs/images/3048149/3278111.png)


:::warning
**Write your key down**
Once you generate an API key, it cannot be retrieved from the Octopus web portal again - we store only a one-way hash of the API key. If you want to use the API key again, you need to store it in a secure place such as a password manager. Read about [why we hash API keys](https://octopusdeploy.com/blog/hashing-api-keys).
:::
