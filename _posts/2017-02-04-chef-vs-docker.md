---
layout: post
title:  "Chef vs Docker"
date:   2017-02-04 12:50:36 -0400
categories: devops, chef, docker
---

![chef](https://secure.gravatar.com/avatar/658c4f32418f36d6c0c7a0cc82ea8bd4?s=128 "Chef")
    ***vs***
   ![docker](https://store-logos-us-east-1.s3.amazonaws.com/docker.png "Docker")

## What is the problem?

Chef is the first technology that embodies the idea of devops. I remember back 2012, chef brought in a fresh ideal of provisioning servers into the software development world and pioneered the "infrastructure as code" concept.

I have been mostly a Ruby developer, so Chef, which is Ruby based (at least the DSL part), is natural. Even that, I found Chef has a steep learning curve and takes some time to get used to and be productive in.

Since then, a few other implementation has come along and gained popularity, including Ansible and Puppet.

Then it came docker...

My experience with docker is very positive. It allows an application to go from nothing to being up and running in very little time. The Dockerfile syntax is relatively easy to grasp, since it is essentially shell script. Docker has very fast startup time.

There are considerable overlap between docker and chef, which begs the question: which one should I use?

And the answer a lot of times, is not "it depends".

## Why Docker?

### 1. Provisioning Servers
Docker seems to be able to do most of the things that Chef does in provisioning servers. People argue that chef and docker are two different tools by pointing out that chef is mostly a configuration management system.

It is true, however...

* in my experience I normally don't change the attributes very much. This reminds me of the Rails style of "convention over configuration". The flexibility of configurations is often overrated?...
* even when configurations do need to be changed, they are handled differently between Docker and Chef: Chef simply reruns chef client on the servers; whereas Docker simply builds a new Docker image, which gets deployed to production to decommission the servers with the older configurations.

### 2. Infrastructure provisioning

Chef has [Chef Provisioning] tool to interact with various providers' API to dynamically provision infrastructure. Docker takes a different approach, mostly because it is not server-based, but container-based. With [Docker Swarm], it creates a cloud inside the Docker infrastructure, so that the need for using AWS constructs is less.

Docker recently rolled out [Docker Data Center] which in a lot of ways, helps to reduce the reliance on the infrastructure providers.

[Chef Provisioning]: https://github.com/chef/chef-provisioning
[Docker Swarm]: https://www.docker.com/products/docker-swarm
[Docker Data Center]: https://www.docker.com/products/docker-datacenter

### 3. Maturity

It can be argued that Docker is still a little young, with a lot of its features being recently released. This is especially true in terms of storage or volume management. As a result, a lot of companies may be cautious in using it. Chef has been relatively more mature. But, this will change over time.
