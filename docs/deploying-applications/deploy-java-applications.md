---
title: Java Applications
description: Deploy to WildFly, Red Hat JBoss EAP and Tomcat using Octopus Deploy
position: 6
---

Octopus Deploy comes with a number of steps that allow you to deploy and modify the state of Java applications for popular Java application servers.

In addition, Java packages like `jar`, `war`, `ear` and `rar` files can be managed as part of the Octopus Deploy library.

## Supported Application Servers

The following application servers are supported by Octopus Deploy:

* Tomcat 7
* Tomcat 8
* Tomcat 9
* Red Hat JBoss EAP 6
* Red Hat JBoss EAP 7
* WildFly 10
* WildFly 11

:::hint
The `Deploy Java Archive` step deploys a Java package to a location on the target machine's filesystem. This means that any Java application server that can deploy applications with a file copy can make use of Octopus Deploy.

The application servers listed above are those that are tested with Octopus Deploy, and have custom steps for deploying and managing the state of applications.
:::

## Deploying Java Applications via Octopus

Regardless of which application server you are deploying to, there are a number of common steps that need to be performed.

### 1. Push a Package to Octopus

For a package to be made available to the deployment steps, it first must be added to the Octopus library.

The Octopus library accepts files with the `jar`, `war`, `ear` and `rar` file extensions.

The package filenames have to adhere to the standard Octopus [versioning rules](https://octopus.com/docs/packaging-applications/versioning-in-octopus-deploy), which are based on the SemVer format.

:::hint
Java packages tend to be formatted with Maven versioning schemes. Maven versions are mostly compatible with SemVer, but the two versioning schemes are not identical.

See the section [Building SemVer Compatible Artifacts](#building_semver_compatible_artifacts) for tips on creating valid package filenames from builds tools like Maven and Gradle.
:::

Valid packages can then be added to the library using the [web based interface, or using the CLI tool](https://octopus.com/docs/packaging-applications/package-repositories/pushing-packages-to-the-built-in-repository).

#### <a name="building_semver_compatible_artifacts"></a>Building SemVer Compatible Artifacts
The most common incompatibility between Maven and SemVer formatting comes from the use of a dash to separate the package name from the version.

For example, by default Maven will build artifacts with names like `myapplication-1.0.0-SNAPSHOT.war`. To be Semver compatible, this filename needs to be in the format `myapplication.1.0.0-SNAPSHOT.war`.

The easiest way to generate the correct filenames in a Maven build is to set the `finalName` element to `<finalName>${project.name}.${project.version}</finalName>`.

The following xml is a snippet from a `pom.xml` file that defines the `finalName` option in a SemVer compatible format.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project
xmlns="http://maven.apache.org/POM/4.0.0"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <build>
    <finalName>${project.name}.${project.version}</finalName>
  </build>
</project>
```

Gradle builds can define the filename through the `war.archiveName` property.

The following code is a snippet of a `build.gradle` file that defines the name of the WAR file in a Semver compatible format.

```
apply plugin: 'java'
apply plugin: 'war'

group = 'com.example'
version = '0.0.1-SNAPSHOT'

war.archiveName "${project.name}.${version}.war"
```

### 2. Ensure the Target Meets the Minimum Requirements

The target machine must have Java 1.8 installed, and the `java` executable must either be on the path, or the `JAVA_HOME` environment variable must be set.

## Deploying to Tomcat via the Manager

The `Deploy to Tomcat via Manager` step takes advantage of the [Manager application](https://tomcat.apache.org/tomcat-7.0-doc/manager-howto.html) shipped with Tomcat to deploy Java applications. The following steps describe the process of deploying a web application (a WAR package) to Tomcat through Octopus Deploy.

### <a name="configure_tomcat"></a>1. Configure Tomcat

Tomcat needs to be configured with a user that Octopus can use to log into the Manager API.

Tomcat users are defined in the `tomcat-users.xml` file. The user that will deploy the applications must be added to the `manager-script` group.

Here is an example of a `tomcat-users.xml` file that defines a user called `tomcat` in the `manager-script` group.

```xml
<?xml version='1.0' encoding='utf-8'?>
<tomcat-users>
    <role rolename="manager-script"/>
    <user username="tomcat"
          password="thepassword"
          roles="manager-script"/>
</tomcat-users>

```

See the [Tomcat documentation](https://tomcat.apache.org/tomcat-7.0-doc/manager-howto.html#Configuring_Manager_Application_Access) for more details on the groups used by the manager application.

### 2. Populate the Tomcat Deployment Step

The `Deploy to Tomcat via Manager` step is used to deploy a package from the Octopus library to Tomcat. The following steps can be used to deploy an application to a Tomcat server.

* Select the `Package feed` and `Package ID` that references the Java application to be deployed.
* Define the `Context path`. This can be the root context with a path of `/`, a simple path like `myapplication`, or a nested path like `myapplication/v1`.
* If you want to take advantage of [Tomcat's parallel deployment](https://tomcat.apache.org/tomcat-7.0-doc/config/context.html#Parallel_deployment) features, you need to define the populate `Deployment Version` field. To be effective, these values are required to increase with each deployment . Common values are the Octopus release number, which is defined in the `#{Octopus.Release.Number}` variable. The package version number can also be used, and is defined in the `#{Octopus.Action.Package.NuGetPackageVersion}` variable.
* Define the path to the [Tomcat Manager](https://tomcat.apache.org/tomcat-7.0-doc/manager-howto.html). The path is relative to the target machine that is running the step, and includes the manager application context path. In most cases the target machine is also the machine hosting Tomcat, so this URL will typically point to `localhost`. Common examples of the manager URL are `http://localhost:8080/manager`.
* Supply the `Management User` and `Management Password` of the user was defined using the instructions from [Configure Tomcat](#configure_tomcat).
* The `Start/Stop the Deployment` option allows you to deploy the application in a started or stopped state. Typically you will deploy in a started state.

## Deploying to Tomcat via Manager Step Details

| Field Name | Required | Default Value | Field Description |
|-|-|-|-|
| Package feed | Yes | | The feed to use to source the Java package from. |
| Package ID | Yes | | The Java package to deploy. |
| Context Path | No | Defaults to the deployment file name e.g. `mayapplication.1.0.0`. | The context path that the application will be deployed to. Set this value to `/` to deploy to the root context. Set it to a value like `myapplication` to deploy to the `/myapplication` context. Nested contexts are also allowed, such as `myapplication/v1.0`.<br/><br/>Leading slashes are optional;  `/myapplication` and  `myapplication` will both deploy to the  `/myapplication` context path.  |
| Deployment Version | No | By default no Tomcat version is assigned to the deployment. | An optional field that defines the Tomcat version that the deployment will be assigned when deployed. Versions are most often used with [Tomcat parallel deployments](https://tomcat.apache.org/tomcat-7.0-doc/config/context.html#Parallel_deployment). The version can be set to match the version of the package from Octopus, can be set to a custom version, or the deployment can have no version.<br/><br/>It is common to use either the Octopus release number as the Tomcat version using the variable `#{Octopus.Release.Number}`, or to use the version number of the package with the variable `#{Octopus.Action.Package.NuGetPackageVersion}`.|
| Tomcat Manager URL | Yes | | This field defines the URL of the Tomcat manager application. This is relative to the target machine performing the deployment. When the Tentacle is on the same machine as the Tomcat instance, this URL will typically reference localhost i.e. `http://localhost:8080/manager`. |
| Management User | Yes | | The user that is supplied when logging into the Tomcat manager API. Note that this user is required to be part of the `manager-script` group. See the [Tomcat documentation](https://tomcat.apache.org/tomcat-7.0-doc/manager-howto.html#Configuring_Manager_Application_Access) for more details on the groups used by the manager application.|
| Management Password | Yes | | The password that is supplied when logging into the Tomcat manager API. |
| Start/Stop the Deployment | Yes |  | This field defines the state of the application after it is deployed. |

## Deploying to WildFly/JBoss EAP

The `Deploy to WildFly or Red Hat JBoss EAP` step uses the management API to deploy applications and change their state. The following steps describe the process of deploying a web application (a WAR package) to WildFly or JBoss EAP through Octopus Deploy.

### 1. Configure Authentication

You have two options for authenticating with WildFly and JBoss EAP servers.

The easiest solution is to use [silent authentication](https://access.redhat.com/documentation/en-us/red_hat_jboss_enterprise_application_platform/7.1.beta/html/how_to_configure_server_security/securing_the_server_and_its_interfaces#silent-auth). Silent authentication allows local users to log into the management API without supplying any credentials, and because Octopus typically executes a deployment from the same machine that is hosting the application server, it is treated as a local user.

To take advantage of silent authentication, the user running the Octopus deployment on the target machine (i.e the user assigned to the Tentacle service in Windows or the SSH user in Linux or MacOS) must have access to the `auth`  directory inside the directory defined by the `jboss.server.temp.dir` variable. By default this directory will be located at `$JBOSS_HOME/standalone/tmp/auth` or `$JBOSS_HOME/domain/tmp/auth`.

The second option is to use the application server `add-user` script to create a new user, and use these credentials in the Octopus deployment steps.

### 2. Populate the WildFly/JBoss EAP Deployment Step

The `Deploy to WildFly or Red Hat JBoss EAP` step is used to deploy a package from the Octopus library to a WildFly or Red Hat JBoss EAP instance.

* Select the `Package feed` and `Package ID` that references the Java application to be deployed.
* Optionally set the `Deployment Name`, which will define the name of the deployment in the WildFly deployment library. For example, deploying a file from the Octopus library with the filename `myapplication.0.0.1.war` and setting this field to `myapplication.war` will mean the deployment is listed in WildFly as `myapplication.war`. If this field is left blank, the original file name will be used by WildFly. For example, deploying a file from the Octopus library with the filename `myapplication.0.0.1.war` and leaving this field blank will mean the deployment is listed in the application server as `myapplication.0.0.1.war`.
* Set the `Management Host or IP` field to the address that the WildFly management interface is listening to. This value is relative to the target machine that is performing the deployment. Since the target machine performing the deployment is typically the same machine hosting the application server, this value will usually be `localhost`.
* Set the `Management Port` to the port bound to the WildFly management interface. For WildFly 10+ and JBoss EAP 7+, this will default to `9990`. For JBoss EAP 6, this will default to `9999`.
* The `Management Protocol` field defines the protocol to be used when interacting with the management interface. For WildFly 10+ and JBoss EAP 7+, this will default to `http-remoting` or `remote+http` (the two are equivalent). For JBoss EAP 6, this will default to `remoting`.
* If you wish to use silent authentication, and have configured the required permissions for the `$JBOSS_HOME/standalone/tmp/auth` or `$JBOSS_HOME/domain/tmp/auth` directory, then the `Management User` and `Management Password` fields can be left blank. Alternatively these fields can hold the credentials that were configured via the `add-user` script.
* If you are deploying to a standalone server, the deployment can be enabled or disabled using the `Enable/Disable the Deployment` field. This field has no effect when deploying to a domain controller.
* If you are deploying to a domain controller, the server groups that will have the deployment enabled must be specified in the `Enabled Server Groups` field. Likewise the server groups that will have the deployment disabled must be specified in the `Disabled Server Groups`. Multiple server groups can be specified separated by a comma. These fields has no effect when deploying to a standalone server.

<a name="context_path"></a>
#### Defining Context Paths

There are multiple ways that the context of an application deployed to WildFly and JBoss EAP is defined.

The `WEB-INF/jboss-web.xml` file inside a war package can define the context path. For example a war package with the following `jboss-web.xml` file will be deployed to the `/myapplication` context.

```xml
<jboss-web>
    <context-root>myapplication</context-root>
</jboss-web>
```

If not defined in the `jboss-web.xml` file, the context of a war file will default to the filename of the deployment. For example, a war file called `myapplication.war` will be deployed to the context `/myapplication`.

If the war file has the special name `ROOT.war` it will be deployed to the root context.

See [Setting the context root of a web application](https://docs.jboss.org/jbossas/guides/webguide/r2/en/html/ch06.html) for more information.

## WildFly/JBoss EAP Deployment Step Details

| Field Name |Required | Default | Field Description |
|-|-|-|-|
| Package feed | Yes | | The feed to use to source the Java package from. |
| Package ID | Yes | | The Java package to deploy. |
| Deployment Name | No | The package will retain the original file name. | The optional name of the deployment as it will appear in the application server library. Examples include `myapplication.war`, `myapplication.jar`, `myapplication.ear` or `myapplication.rar`. If this field is not set, the original filename of the deployment will be used.|
| Management Host or IP | Yes | | The location of the management interface. This is relative to the target machine performing the deployment. When the Tentacle is on the same machine as the application server instance, this URL will typically reference `localhost`. |
| Management Port | Yes | | The port that the management interface is bound to. For WildFly 10+ and JBoss EAP 7+, this defaults to `9990`. For JBoss EAP 6, this defaults to `9999`. |
| Management Protocol | Yes | | The protocol to use when interacting with the management interface. For WildFly 10+ and JBoss EAP 7+, this defaults to `http-remoting` or `remote+http` (the two are equivalent). If the management interface is protected with a certificate, the protocols are `https-remoting` or `remote+https`. For JBoss EAP 6 this defaults to `remoting`. Refer to the [JBoss Remoting](http://jbossremoting.jboss.org/remoting-3) project for more details on these protocols. |
|Management User| No | Defaults to using silent authentication. | The username to supply when connecting to the management interface. If left blank, silent authentication will be used. Silent authentication requires that the target machine performing the deployment also be the machine hosting the application server, and that the user performing the deployment (i.e. the Tentacle service user or the SSH user) have access to the `tmp/auth` directory. See the [JBoss EAP documentation](https://access.redhat.com/documentation/en-us/red_hat_jboss_enterprise_application_platform/7.1.beta/html/how_to_configure_server_security/securing_the_server_and_its_interfaces#silent-auth) for more details on silent authentication.|
|Management Password | No | Defaults to using silent authentication. | The password to supply when connecting to the management interface. This field is option is using silent authentication.|
|Enable/Disable the Deployment | Yes | | When deploying to a standalone server, this field defines if the deployment will be enabled or disabled. This field has no effect when deploying to a domain controller. |
|Enabled Server Groups | No | | When deploying to a domain controller, this field lists the server groups that will have the deployment enabled in. Multiple server groups can be supplied as a comma separated list. This field has no effect when deploying to a standalone server. |
|Disabled Server Groups | No | | When deploying to a domain controller, this field lists the server groups that will have the deployment disabled in. Multiple server groups can be supplied as a comma separated list. This field has no effect when deploying to a standalone server. |

## Starting/Stopping Tomcat Applications

The `Start/Stop App in Tomcat` step is used to start or stop applications that have already been deployed to Tomcat. The following steps can be used to start or stop an application deployed to a Tomcat server.

* Define the `Context path` of the deployed application to start or stop. This can be the root context with a path of `/`, a simple path like `myapplication`, or a nested path like `myapplication/v1`.
* Define the `Deployment Version` of the deployed application to start or stop. Leave this field blank to modify the state of an unversioned Tomcat deployment.
* Define the path to the [Tomcat Manager](https://tomcat.apache.org/tomcat-7.0-doc/manager-howto.html). The path is relative to the target machine that is running the step. In most cases the target machine is also the machine hosting Tomcat, so this URL will typically point to `localhost`. Common examples of the manager URL are `http://localhost:8080/manager`.
* Supply the `Management User` and `Management Password` of the user was defined using the instructions from [Configure Tomcat](#configure_tomcat).
* The `Start/Stop the Deployment` option allows you to start or stop the deployed application.

## Starting/Stopping Tomcat Applications Step Details

| Field Name | Required | Default | Field Description |
|-|-|-|-|
| Context Path | Yes | | The context path that the application will be deployed to. Set this value to `/` to deploy to the root context. Set it to a value like `myapplication` to deploy to the `/myapplication` context. Nested contexts are also allowed, such as `myapplication/v1.0`.<br/><br/>Leading slashes are optional;  `/myapplication` and  `myapplication` will both deploy to the  `/myapplication` context path.  |
| Deployment Version | No | Defaults to modifying the state of the unversioned Tomcat deployment. | The version of the application to start or stop. Leave the field blank to modify the state of an unversioned Tomcat deployment. |
| Tomcat Manager URL | Yes | | This field defines the URL of the Tomcat manager application. This is relative to the target machine performing the deployment. When the Tentacle is on the same machine as the Tomcat instance, this URL will typically reference localhost i.e. `http://localhost:8080/manager`. |
| Management User | Yes | | The user that is supplied when logging into the Tomcat manager API. Note that this user is required to be part of the `manager-script` group. See the [Tomcat documentation](https://tomcat.apache.org/tomcat-7.0-doc/manager-howto.html#Configuring_Manager_Application_Access) for more details on the groups used by the manager application.|
| Management Password | Yes | | The password that is supplied when logging into the Tomcat manager API. |
| Start/Stop the Deployment | Yes | |This field defines whether the application will be started or stopped. |

## Enabling/Disabling WildFly and EAP Applications

The `Enable/Disable deployment in WildFly` step is used to modify the state of a deployed application. The following steps can be used to enable or disable an application deployed to an application server.

* Define set the `Deployment Name` of the application to be enabled or disabled.
* Set the `Management Host or IP` field to the address that the WildFly management interface is listening to. This value is relative to the target machine that is performing the deployment. Since the target machine performing the deployment is typically the same machine hosting WildFly, this value will usually be `localhost`.
* Set the `Management Port` to the port bound to the WildFly management interface. For WildFly 10+ and JBoss EAP 7+, this will default to `9990`. For JBoss EAP 6, this will default to `9999`.
* The `Management Protocol` field defines the protocol to be used when interacting with the management interface. For WildFly 10+ and JBoss EAP 7+, this will default to `http-remoting` or `remote+http` (the two are equivalent). For JBoss EAP 6, this will default to `remoting`.
* If you wish to use silent authentication, and have configured the required permissions for the `$JBOSS_HOME/standalone/tmp/auth` or `$JBOSS_HOME/domain/tmp/auth` directory, then the `Management User` and `Management Password` fields can be left blank. Alternatively these fields can hold the credentials that were configured via the `add-user` script.
* If you are deploying to a standalone server, the deployment can be enabled or disabled using the `Enable/Disable the Deployment` field. This field has no effect when deploying to a domain controller.
* If you are deploying to a domain controller, the server groups that will have the deployment enabled must be specified in the `Enabled Server Groups` field. Likewise the server groups that will have the deployment disabled must be specified in the `Disabled Server Groups`. Multiple server groups can be specified separated by a comma. These fields has no effect when deploying to a standalone server.

## Enabling/Disabling WildFly and EAP Applications Step Details

| Field Name | Required | Default | Field Description |
|-|-|-|-|
| Deployment Name | Yes | | The name of the application to enable or disable. This is the name of the application as it appears in the application server management console. This name may or may not be the same as the name of the package from the Octopus library.|
| Management Host or IP | Yes | | The location of the management interface. This is relative to the target machine performing the deployment. When the Tentacle is on the same machine as the application server instance, this URL will typically reference `localhost`. |
| Management Port | Yes | | The port that the management interface is bound to. For WildFly 10+ and JBoss EAP 7+, this defaults to `9990`. For JBoss EAP 6, this defaults to `9999` |
| Management Protocol | Yes | | The protocol to use when interacting with the management interface. For WildFly 10+ and JBoss EAP 7+, this defaults to `http-remoting` or `remote+http` (the two are equivalent). If the management interface is protected with a certificate, the protocols are `https-remoting` or `remote+https`. For JBoss EAP 6 this defaults to `remoting`. Refer to the [JBoss Remoting](http://jbossremoting.jboss.org/remoting-3) project for more details on these protocols. |
|Management User| No | Defaults to using silent authentication. | The username to supply when connecting to the management interface. If left blank, silent authentication will be used. Silent authentication requires that the target machine performing the deployment also be the machine hosting the application server, and that the user performing the deployment (i.e. the Tentacle service user or the SSH user) have access to the `$JBOSS_HOME/standalone/tmp/auth` or `$JBOSS_HOME/domain/tmp/auth` directory. See the [JBoss EAP documentation](https://access.redhat.com/documentation/en-us/red_hat_jboss_enterprise_application_platform/7.1.beta/html/how_to_configure_server_security/securing_the_server_and_its_interfaces#silent-auth) for more details on silent authentication.|
|Management Password | No |  Defaults to using silent authentication. | The password to supply when connecting to the management interface. This field is option is using silent authentication.|
|Enable/Disable the Deployment | Yes | | When deploying to a standalone server, this field defines if the deployment will be enabled or disabled. This field has no effect when deploying to a domain controller. |
|Enabled Server Groups | No | | When deploying to a domain controller, this field lists the server groups that will have the deployment enabled in. Multiple server groups can be supplied as a comma separated list. This field has no effect when deploying to a standalone server. |
|Disabled Server Groups |No | | When deploying to a domain controller, this field lists the server groups that will have the deployment disabled in. Multiple server groups can be supplied as a comma separated list. This field has no effect when deploying to a standalone server. |

## Deploying via Package Copy

The `Deploy Java Archive` step is used to copy a Java archive to the target machine's filesystem. This step is not tied to any particular application server, and can be used to deploy applications to any server that will accept files copied into a deployment directory.

This step mirrors the functionality of the `Deploy a package` step, which is documented [here](https://octopus.com/docs/deploying-applications/deploying-packages).\

## Variable Substitution in Java Packages

Octopus provides the [ability to replace variables in packages during deployment](https://octopus.com/docs/deploying-applications/substitute-variables-in-files). This is done using a [specific syntax](https://octopus.com/docs/reference/variable-substitution-syntax) implemented by the [Octostash](https://github.com/OctopusDeploy/Octostache) library.

The syntax used by Octostash and Java libraries such as Spring do overlap, so care must be taken to ensure that files intended to be used as Octostash templates during deployment don't interfere with local development.

For example, you may have a `application.properties` file that defines an environment variable which reflects the environment that the application has been published to.

```properties
environment: #{Environment}
```

This file is expected to be used as an Octostash template during deployment, but when testing locally you will receive an error like `Unsatisfied dependency expressed through field 'environment';`. This is because during local development the template file has not been processed, and the common syntax between Spring and Octostash means Spring is attempting to resolve the variable `Environment`, which doesn't exist.

[Spring profiles](https://docs.spring.io/spring-boot/docs/current/reference/html/howto-properties-and-configuration.html) provide a convenient way to load valid properties files for local development, while leaving environment specific templates for deployments processed by Octopus.

For example, you may create a file called `application-dev.properties` with the following settings:

```properties
environment: Development
```

Maven can then be instructed to active the `dev` profile for testing:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <build>
    <plugins>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-surefire-plugin</artifactId>
        <configuration>
          <argLine>-Dspring.profiles.active=dev</argLine>
        </configuration>
      </plugin>
    </plugins>
  </build>
</project>
```

Now local testing is done against a valid properties file, while the main `application.properties` file is used a template during deployment to environments managed by Octopus.

## Error Messages

The Java deployment steps include a number of unique error codes that may be displayed in the output if there was an error. Below is a list of the errors, along with any additional troubleshooting steps that can be taken to rectify them.

#### WILDFLY-DEPLOY-ERROR-0001
There was an error taking a snapshot of the current configuration.

#### WILDFLY-DEPLOY-ERROR-0002
There was an error deploying the artifact.

#### WILDFLY-DEPLOY-ERROR-0003
There was an error reading the existing deployments.

####  WILDFLY-DEPLOY-ERROR-0004
There was an error adding the package to the server group.

#### WILDFLY-DEPLOY-ERROR-0005
There was an error deploying the package to the server group.

This may be due to duplicate context paths. Check that the context path is not already assigned to an existing application.

See [Defining Context Paths](#context_path) for more information on how context paths are assigned in WildFly.

#### WILDFLY-DEPLOY-ERROR-0006
There was an error undeploying the package to the server group

#### WILDFLY-DEPLOY-ERROR-0007
There was an error deploying the package to the standalone server.

This may be due to duplicate context paths. Check that the context path is not already assigned to an existing application.

See [Defining Context Paths](#context_path) for more information on how context paths are assigned in WildFly.

#### WILDFLY-DEPLOY-ERROR-0008
There was an error enabling the package in the standalone server

####  WILDFLY-DEPLOY-ERROR-0009
There was an error logging into the management API.

Make sure the credentials are correct.

####  WILDFLY-DEPLOY-ERROR-0010
There was an error logging out of the management API

####  WILDFLY-DEPLOY-ERROR-0011
There was an error terminating the CLI object

####  WILDFLY-DEPLOY-ERROR-0012
There was an error changing the deployed state of the application

#### WILDFLY-DEPLOY-ERROR-0013
The login was not completed in a reasonable amount of time.

This can happen if no credentials where supplied with the step, and silent authentication failed.

Either supply credentials to be used, or ensure that the user performing the deployment (the Tentacle service user in Windows or the SSH user in Linux and MacOS) has access to the application server `$JBOSS_HOME/standalone/tmp/auth` or `$JBOSS_HOME/domain/tmp/auth` directory.

#### WILDFLY-DEPLOY-ERROR-0014
An exception was thrown during the deployment.

####  WILDFLY-DEPLOY-ERROR-0015
Failed to deploy the package to the WildFly/EAP standalone instance

#### WILDFLY-DEPLOY-ERROR-0016
Failed to deploy the package to the WildFly/EAP domain

####  TOMCAT-DEPLOY-ERROR-0001
There was an error deploying the package to Tomcat

####  TOMCAT-DEPLOY-ERROR-0002
There was an error deploying a tagged package to Tomcat

####  TOMCAT-DEPLOY-ERROR-0003
There was an error undeploying a package from Tomcat

####  TOMCAT-DEPLOY-ERROR-0004
There was an error enabling or disabling a package in Tomcat

#### TOMCAT-DEPLOY-ERROR-0005
This is a catch all error message for unexpected errors during a Tomcat deployment. Ensure that:

* The manager URL is correct. Ensure the URL includes the context of the manager application, and that the port and hostname/IP address are correct. Also ensure that the hostname/IP address can be resolved from the target machine hosting the tentacle. A common example of a correct manager URL is `http://localhost:8080/manager`.
* The Tomcat credentials are correct.
* The firewall allows connection to the Tomcat server.
* Tomcat is started and running.

#### TOMCAT-DEPLOY-ERROR-0006
A HTTP return code indicated that the login failed due to bad credentials. Make sure the username and password are correct.

#### TOMCAT-DEPLOY-ERROR-0007
A HTTP return code indicated that the login failed due to invalid group membership. Make sure the user is part of the `manager-script` group in the `tomcat-users.xml` file.

See the [Tomcat documentation](https://tomcat.apache.org/tomcat-7.0-doc/manager-howto.html#Configuring_Manager_Application_Access) for more details on the groups used by the manager application.

#### TOMCAT-DEPLOY-ERROR-0008
The application was not successfully started or stopped.

This can happen if the application failed to initialize. Check the Tomcat logs for information on why the application could not be started.

Also confirm that the context path and version match a deployed application.
