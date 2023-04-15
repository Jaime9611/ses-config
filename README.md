# Traveler Service Config

## Introduction

<span dir="">We initialized </span>this _Git_<span dir=""> repository with </span>the purpose of saving all our configuration files in one place<span dir=""> and populate them with the properties that every micro service required.</span>

Currently we have available the default and production configuration profiles.

## Properties in Config Server

```
server.port=8888
spring.application.name=configservice
spring.cloud.config.enabled=true
spring.cloud.config.server.git.uri=${GIT_URI:https://gitlab.jalasoft.local/Paolo.Moscoso/traveler-service-config.git}
spring.cloud.config.server.git.username=${GIT_USER}
spring.cloud.config.server.git.password=${GIT_PASSWORD}
spring.cloud.config.server.git.default-label=main
spring.cloud.config.server.git.skip-ssl-validation=true
```

* <span dir="">The first we need to configure is the server </span>_port_<span dir=""> on which our server is listening and </span>its name, then we have to enable the cloud config service.
* We have to set the _Git_<span dir="">-URL, which provides our version-controlled configuration content.</span> (You can create your repository and changes this URL). Optionally we can enter our username and password if required.
* If we do not want to create another repository we could also create a new branch and point our local environment to the new one. I suggest using this approach when you have a task that requires to add new properties in the config file, after your task was approved and merged you can add your changes with an extra MR to the main branch in this repository.

```
spring.cloud.config.server.git.default-label=MyOwnBranchName
```

## Create your own configuration profile

When you want to add a configuration profile for a new working environment, the following must be taken into account, <span dir="">the name of the configuration file is composed like a normal Spring </span>_application.properties_<span dir="">, but instead of the word ‘application,' a configured name, such as the value of the property </span>_‘**spring.application.name**',_<span dir=""> of the client is used, followed by a dash and the active profile. For example:</span>

```
config-client-development.properties
config-client-production.properties
```

As a mentioned make sure in the local properties of the client application you are using the same name and profile. Example:

```
spring.application.name=config-client
spring.profiles.active=development
```

## Important Properties

Currently there are different micro services each one with its own functionality and properties, but we have two properties that are repeated in all of them.

```
server.port=8090 #Default Profile
server.port=${ATTACHMENTS_SERVER_PORT} #Production Profile
```

<span dir="">We use the </span>**server.port**<span dir=""> property to overwrite the default property</span>. (By default 8080). When using the production profile be sure to get this value from an environment variable.

```
eureka.client.service-url.defaultZone=http://localhost:8095/eureka
eureka.client.service-url.defaultZone=${EUREKA_SERVER}
```

This is <span dir="">a fallback value that provides the Eureka service URL for our application client that does not express a preference</span>.
