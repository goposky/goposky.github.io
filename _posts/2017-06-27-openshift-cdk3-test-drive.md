---
layout: post
title: "Exploring OpenShift with Red Hat CDK 3.0"
date: 2017-06-27 20:00:51 +0000
image: /assets/images/redhat.jpg
tags: [Openshift, PaaS, container, devops, RedHat, minishift]
excerpt: "Red Hat’s OpenShift container platform is an industry leading PaaS for building and running cloud-native applications. OpenShift is an enterprise-grade platform with comprehensive features, but this blog will limit to looking at it from an introductory viewpoint for the developer or CI/CD engineer. For a quick head-start on"
---

Red Hat’s OpenShift container platform is an industry leading PaaS for building and running cloud-native applications. OpenShift is an enterprise-grade platform with comprehensive features, but this blog will limit to looking at it from an introductory viewpoint for the developer or CI/CD engineer.

For a quick head-start on OpenShift fundamentals I recommend reading [this website](http://playbooks-rhtconsulting.rhcloud.com/playbooks/fundamentals/building_blocks_openshift.html). For more detail on architecture and components, the best place to go is the [Red Hat official documentation](https://docs.openshift.com/container-platform/3.5/architecture/index.html).

OpenShift is available in many flavours.

- *[OpenShift Origin](https://www.openshift.org)* - The upstream open-source version, built upon Kubernetes and other projects.
- *OpenShift Enterprise[^1] [Openshift Container Platform](https://www.redhat.com/en/technologies/cloud-computing/openshift)* - Red Hat’s flavour of Openshift platform for on-prem or cloud deployment with support.
- *[OpenShift Dedicated](https://www.openshift.com/dedicated/index.html)* - Same as Openshift enterprise, but hosted as a cloud-based service by Red Hat.
- *[OpenShift Online](https://www.openshift.com)* - A fully managed on-demand PaaS on cloud with limited memory and storage.
- *[Red Hat CDK](https://developers.redhat.com/products/cdk/overview/)* - Container Development Kit, a stripped-down version to serve as a local environment for developers.
- *[openshift.io](https://openshift.io)* (not GA as of writing) - A fully cloud-based development environment.

Many indeed! But for this blog I choose the Red Hat CDK, the easiest way to get up and running. The CDK will provide a developer-ready single node OpenShift cluster. Typical use cases of the CDK environment would be to do local development, demos, or simply to familiarize with OpenShift.

**Installation and setup**

With the release of the new CDK version 3.0 the setup is extremely simple. It will spin up a Red Hat VM on the hypervisor of choice, and then create a single node Openshift cluster on this VM. I use macOS with virtualbox (default) for this setup.

- Join the [Red Hat Developers program](https://developers.redhat.com/) and obtain a free Red Hat Developer subscription.
- Download the installer from [Red Hat Development Suite](https://developers.redhat.com/products/devsuite/download/).
- Run the installation and select Red Hat Container Development Kit components from the installer menu.

*For additional options or other OS/hypervisors refer to [Red Hat documentation](https://access.redhat.com/documentation/en/red-hat-container-development-kit/).*

The CDK comes bundled with the `minishift` and `oc` binaries that you can use to interact with Openshift. To set up the CDK cluster,

- Set and export the variables `MINISHIFT_USERNAME` and `MINISHIFT_PASSWORD` with your subscription account.
- Start minishift to create the minishift vm and provision a single-node Openshift cluster over it

```
$ minishift start
Starting local OpenShift cluster using 'virtualbox' hypervisor...
Registering machine using subscription-manager
... 
   OpenShift server started.
   The server is accessible via web console at:
       https://192.168.99.100:8443

   To login as administrator:
       oc login -u system:admin
```

- To set the `oc` binary to your path, run:  
 `eval $(minishift oc-env)`

You now have an Openshift cluster at your disposal complete with a master-cum-node host, built-in docker registry, router, internal etcd store, etc. You can check the versions you got:

```
$ oc version
oc v3.5.5.8
kubernetes v1.5.2+43a9be4
```

**Deploy an application**

- Login with the *developer* account: `oc login -u developer -p developer`
- Create a new project

```
$ oc new-project cdk-intro
Now using project "cdk-intro" on server "https://192.168.99.100:8443".
```

The project is created and you are automatically "inside" that project's namespace.

- Now let's deploy an application to this project. We will deploy a sample nodejs app from github here at [https://github.com/openshift/nodejs-ex](https://github.com/openshift/nodejs-ex). We will use OpenShift's source-to-image (s2i) feature. This is the sequence events that will occur:

- OpenShift pulls the source code from github and determines what type of application this is, ie, nodejs.
- Choses a builder image that matches nodejs
- Builds the application image and pushes it to an *imagestream*.
- This will automatically trigger a *build* followed by a *deployment*. At the end of the deployment we will have the application deployed as a service available at an endpoint.

```
$ oc new-app https://github.com/openshift/nodejs-ex -l name=my-app
--> Found image 2e621c4 (12 days old) in image stream "openshift/nodejs" under tag "4" for "nodejs"

    Node.js 4 
    --------- 
    Platform for building and running Node.js 4 applications

    Tags: builder, nodejs, nodejs4

    * The source repository appears to match: nodejs
    * A source build using source code from https://github.com/openshift/nodejs-ex will be created
      * The resulting image will be pushed to image stream "nodejs-ex:latest"
      * Use 'start-build' to trigger a new build
    * This image will be deployed in deployment config "nodejs-ex"
    * Port 8080/tcp will be load balanced by service "nodejs-ex"
      * Other containers can access this service through the hostname "nodejs-ex"

--> Creating resources with label name=my-app ...
    imagestream "nodejs-ex" created
    buildconfig "nodejs-ex" created
    deploymentconfig "nodejs-ex" created
    service "nodejs-ex" created
--> Success
    Build scheduled, use 'oc logs -f bc/nodejs-ex' to track its progress.
    Run 'oc status' to view your app.
```

- We can view the status of the app as follows:

```
$ oc status
In project cdk-intro on server https://192.168.99.100:8443

svc/nodejs-ex - 172.30.35.237:8080
  dc/nodejs-ex deploys istag/nodejs-ex:latest <-
    bc/nodejs-ex source builds https://github.com/openshift/nodejs-ex on openshift/nodejs:4 
    deployment #1 deployed 4 minutes ago - 1 pod
```

- Let's expose the service and get a route to access it.

```
$ oc expose service nodejs-ex
route "nodejs-ex" exposed

$ oc get route
NAME        HOST/PORT                                   PATH      SERVICES    PORT       TERMINATION   WILDCARD
nodejs-ex   nodejs-ex-cdk-intro.192.168.99.100.nip.io             nodejs-ex   8080-tcp                 None
```

The application is now accessible at *[http://nodejs-ex-cdk-intro.192.168.99.100.nip.io](http://nodejs-ex-cdk-intro.192.168.99.100.nip.io)*.

OpenShift integrates well with developer IDEs like Eclipse. Read [Samuel Terburg's blog](http://www.sterburg.nl/2017/05/28/openshift-local-development-options/) for a complete list of local development options.

**Continuous delivery with OpenShift**

While OpenShift comes integrated with Jenkins, it is also possible to just use the built-in continuous delivery features of OpenShift itself. A *build* can be configured with triggers based on source code change, registry update, etc which will automatically trigger off a *deployment*.

OpenShift *projects* (based on kubernetes *namespaces*) are isolated, ie, objects residing in one namespace are oblivious to other namespaces unless configured otherwise. This allows us to create logical environments that are separate from each other in the form of namespaces. Thus you could have a project representing your *dev* environment, another project for *test*, *prod*, and so on.

Usually we do not want to let our *dev* and *prod* containers running on the same host. Typically we have more powerful and secure hosts for our *prod* environments than for *dev* or *test*. This separation can be done easily using *labels* and *selectors*. You label specific nodes as *env=dev*, *env=prod*, etc. Then, you can specify a *nodeSelector* for each of your project and specify the label corresponding to the environment. Once this is done, your *dev* project's pods will land on your *dev* hosts and *prod* pods on *prod* hosts. *Labels* and *selectors* are simple yet powerful, so expect to make use of them a lot.

Organizations that are paranoid about environment isolation can deploy separate OpenShift clusters for each environment, and connect them to a common registry to do continuous delivery across environments. This combined with OpenShift's SDN (software Defined Network) provides robust isolation. Unfortunately you cannot explore a multi-node setup with the CDK environment. You can set that up using the opensource version, or the 30 day evaluation subscription of [Openshift Enterprise](https://www.openshift.com/container-platform/trial.html).

[^1]: Apparently rebranded to sound less enterprise-y and boring.
