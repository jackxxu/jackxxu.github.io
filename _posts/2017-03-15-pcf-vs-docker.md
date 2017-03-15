---
layout: post
title:  "PCF vs Docker"
date:   2017-03-15 12:50:36 -0400
categories: devops, cloud foundry, pivotal, pcf, docker
---

![chef](http://www.feeny.org/wordpress/wp-content/uploads/2015/04/featured-pcf.png "PCF")
    ***vs***
   ![docker](https://store-logos-us-east-1.s3.amazonaws.com/docker.png "Docker")


Both PCF (Pivotal Cloud Foundry or any other favor of Cloud Foundry) and Docker are very popular devops technologies. People often wonder what they do and where they fit in. There are already some discussions on StackOverflow.

To start with, the two technologies work at different levels. PCF is PaaS (Platform As A Service) and Docker is IaaS (Infrastructure As A Service), so the former works at a higher level of abstraction.

I also noticed a couple of differences between these duo:

1. PCF, as far as I know, is a deployment technology and is commonly used at the very end of the pipeline -- i.e. to send the app to the cloud. Docker can be utilized for the entire cycle. for example, I have been developing my app inside its container, so that I can avoid all the configuration of packages and know for sure that my dev environment is the same as everyone else's on the team. By the same token, docker can also be used to simplify CI environment. For example, jenkins agents no longer need to be bothered with installing the packages that are necessary for running the pipeline as it now can simply run pipelines against the Docker containers.
2. for Docker, I can **prebuild** an image and use **the same** image for all environments, which makes it faster to deploy and more importantly, gives me confidence that things likely are going to work when deploying to production. PCF builds the container at deployment time.
