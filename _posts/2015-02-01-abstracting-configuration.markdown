---
layout: post
title:  "Abstracting configuration from applications"
date:   2015-02-01
tags: nowtv, docker, deployment, 12factor,
---
Dockerizing our applications at NOW TV forced us to rethink application configuration, currently each RPM had every environment configuration bundled within, an environment variable is used to tell the application which configuration it should read.

Updating the configuration without building a new RPM was possible, we drop the updated config file (non version controlled) to a specific location and then reload the application, it would read this external configuration and override.

However over the years we've run into issues with this approach. Simply put overriding configurations is error-some, doing a new deploy we often forget to delete or re-write the external configuration. Deleting the external configuration means we lost role back capability.

Ultimately an easy fix is to always read external, always version control and tie this into the deployment process. This is exactly what we're doing and how we're achieving it with Docker.

#### Configurations with docker

Once a container is built we can't change the bundled configuration, without rebuilding; an unnecessary task. We set ourselves the challenge, how can we better manage the configuration and abstract it from the application?

DevOps jumped at the chance to remind us about [12 factor applications](http://12factor.net), looking specifically at the configuration.

Whilst we manage some applications via environment variables, those applications not following this principle, it was a leap too far on our upgrade path, we opted for an incremental approach.

As developers we're used to using a VCS; we specifically use git. Using git allows us to version control our configuration and create configurations per environment, all maintained by the relevant development teams.

#### Our set up with docker using git

Each team has a repository per application and each branch maps to an environment. Using master to put the initial configuration, often this is the development configuration.

Other branches pull changes from master, ensuring the format is maintained through all environments, thus only the value needs to be changed.

Each docker application exposes a volume `/var/app/config`, the application in the container looks within that location for configuration. The application fails to start if no configuration is found.

#### Next steps

Automating the deployment of configurations is our next step, deploying the application will keep the git tagged configuration inline with the container tag. Applications would expose the configuration on restricted admin endpoint, after successful application start up the git configuration  can be compared, verifying the application has started with the correct configuration.

Finally aiming to tie this process into a build job, will enable our teams to achieve continous deployment.

#### TLDR;

Abstract your configuration away as a first step, if you can move straight to using environment variables in an automated way great. But don't be fearful of small steps by not bundling your configuration with your application as a first step. Finally automate the testing of the application after deployment and as things don't always go to plan, automate the roll back process.
