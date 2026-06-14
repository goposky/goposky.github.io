---
layout: post
title: "Born in the cloud"
date: 2017-02-02 11:00:00 +0000
image: /assets/images/clouds.jpg
tags: [cloud-native, container, kubernetes, PaaS, Openshift, google, docker, cloud]
excerpt: "It is 2017. Gone are the days when terms such as “cloud” and “devops” were brushed off as mere hype. Yes, in a sense these are just words, but they have come to represent real strategies that organisations now employ and benefit from. As companies sake to solve newer problems"
---

It is 2017. Gone are the days when terms such as “cloud” and “devops” were brushed off as mere hype. Yes, in a sense these are just words, but they have come to represent real strategies that organisations now employ and benefit from. As companies sake to solve newer problems in a post-cloud world, a new paradigm emerged which has come to be known as “cloud-native”. At the core of this approach is to do things small (microservices, containerization, etc), that would enable highly distributed and scalable systems. This blog looks at the cloud-native landscape and its evolution.

**The rise of Kubernetes**

It was during the Devopsdays Amsterdam event in June 2015 when I first heard of a new tech in town with a hard to pronounce name, “Kubernetes”. Containers (especially Docker) had just gotten popular around the time, and the problem of managing them at scale was very present. Docker Swarm was found lacking when it came to managing containers spread across number of hosts as in a typical datacenter situation. Google itself had been running containers for a while, and internally working on developing its own container management platform. So in 2015, when Google open-sourced this platform as Kubernetes, it was quickly lapped up by early adopters. Because hey, Google use it so let’s check this out! Kubernetes provides a set of loosely coupled and extensible components which are building blocks to deploy, scale and manage containerised applications. It was a very neat approach in typical Google style that people loved. Kubernetes quickly became one of the most active projects on Github.

**Community leverage**

In late 2015 Google teamed up with the Linux Foundation to establish the Cloud Native Computing Foundation (CNCF), with the aim to put together an open-source toolkit for cloud-native applications. CNCF focuses primarily on enabling interoperability of these tools, fostering a kind of standard level playing ground for all cloud-native tech. The first project 'adopted' by CNCF was Kubernetes. Since then, it has taken in a number of open-source cloud-native projects into its fold for every space that Kubernetes doesn’t cover (monitoring, logging, service-discovery, etc). By leveraging open-source the expectation is that a commons would emerge that everybody can make use of, and contribute to in order to ensure general welfare, and avoid lock-ins. With Kubernetes, Google has attempted to push the industry towards a certain level of standardisation around container management tech which was absent at the time. The extensibility provided by Kubernetes API makes it possible for any vendor to implement additional functional wrappers around it. This in turn led to the proliferation of containerized PaaS (platform-as-a-service) solutions.

**The container platform gold rush**

As the world began to adopt microservices and containers, infrastructure software vendors (traditional as well as new ones) came up with their own PaaS offerings built over existing container management tools like kubernetes. They adopt a more developer-centric approach trying to abstract away much of the infrastructure details. In a battle to gain developer mindshare the platforms constantly add built in support for favorite continuous delivery, language-specific build tools, etc, what they call "batteries included". In addition they need to keep up with newer versions of underlying cloud-native toolset like docker and kubernetes which release rapidly. The crowded PaaS space is currently dominated by the likes of Pivotal Cloud foundry, Red Hat Openshift, Rancher, etc.

**What's ahead**

Into 2017 we are seeing Kubernetes evolve as the defacto standard in the container orchestration space, having grown over 30%, much at the expense of Pivotal Cloud Foundry and Docker Swarm. Almost every major cloud provider (with the notable exception of Amazon Web Services, for now) and most container-based PaaS solutions now run (or support) a Kubernetes based container service. Container platforms which are based on Kubernetes like Red Hat Openshift have the momentum at the moment. More organizations will take leap into cloud-native making use of the plethora of options available. In the meantime they should, look out at shiny new toys like Serverless and CaaS (Container-as-a-Service) that could possibly impact (or disrupt) the landscape.
