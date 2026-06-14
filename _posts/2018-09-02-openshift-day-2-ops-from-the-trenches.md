---
layout: post
title: "OpenShift Day 2 Ops: From the trenches"
date: 2018-09-02 16:57:00 +0000
image: /assets/images/from-the-trenches.jpg
tags: [Openshift, cloud, RedHat]
excerpt: "Once your OpenShift POC is successful and it is time to scale up and deliver services on production you need to focus on day 2 operations. In this blog you will find some important lessons we learnt on the job running production clusters."
---

Most implementations of OpenShift start with a proof-of-concept. Sooner or later this POC is successful and it is time to scale up and deliver services on production. In the [official OpenShift documentation](https://docs.openshift.com/container-platform/latest/day_two_guide/index.html) are great tips for day 2 operations. However, in this blog you will find some additional but important lessons we learnt on the job running production clusters.

### Observability

Robust observability, if implemented, will provide the first tangible benefits of the new platform. By observability we mean the 3 pillars - logging, metrics and tracing. OpenShift does come with it’s own built-in logging and metrics infrastructure but with a little bit of plumbing we can get a best of breed monitoring solution. Some examples of this plumbing work are: forwarding the logging via fluentd to an enterprise-wide platform like Splunk, implementing Prometheus-based metrics monitoring, interpreting audit logs and tying up with alerting systems, implementing service-mesh and tracing solutions. The ability to monitor the system deeply and pinpoint issues provides a shot in the arm for any platform or application team.

### Security

OpenShift comes security-hardened out of the factory, with plenty of built-in constructs like secrets, service accounts, security context constraints, RBAC and identity provider integrations. A whole range of additional security measures may be implemented, from scanning of images, to audit and security analysis, to running to chaos tests, not the least adhering to best practices for managing such infrastructure. Attack surface area can be diminished by addressing vulnerabilities at host, platform, and container levels. When it comes to security, more is always better. Maintaining a principle of least access, ie, granting privileges only when necessary, together with a shared DevSecOps model will minimise attack vectors.

### Backup, restore, upgrades

Taking regular backups are table-stakes while managing any serious infrastructure platform. In the case of a multi-tenant platform like OpenShift this may be a shared responsibility of the tenants and the platform teams. The entire cluster definition maybe backed up by performing an etcd backup, while the data-backup (of registry and similar infra components) maybe implemented via the backup procedures of the underlying persistent storage. Do not be afraid to regularly test the etcd backup and restore - fail sooner and learn fast! Another moment of test of stability is during upgrades. With each release the upgrade process is getting simpler, but there could always be breaking changes, so the release notes are an important accompaniment. As a general practise it is recommended to keep up with the OpenShift release cadence without too long a delay.

### Resource utilization

OpenShift allows administrators to allot and ration compute, memory and network resources to each tenant at a granular level. Enforcing resource limits at namespace and cluster levels ensures that no runaway application can hog cluster resources. Cloudforms can be used for calculating infrastructure costs, and report to development teams on their resource consumption, with the possibility of implementing chargeback.

### Persistent storage

At an early stage during the implementation of the platform it becomes clear that we need persistent storage in order to retain the state of the cluster, the registry, and applications. While OpenShift offers a robust persistent storage mechanism in conjunction with many types of backends (filesystem-based and block-based), it is usual practise to just start using NFS as storage backend simply because it is easy and known to most system administrators. Sooner than later, it becomes imperative to look at dynamically provisioned storage backends such as GlusterFS, AWS Elastic Block Store, GCE Persistent disk, etc. Dynamic storage provides great flexibility to administrators, and allows users to request for storage without having any knowledge of underlying infrastructure.

### Human Ops

For all the technical measures listed above, a key ingredient is the human element. How is the platform Ops team organised? How will the team deliver its services to tenants of the platform? How will the team keep up with the new features and ever changing technology landscape? In my opinion, the team should focus on automating as much as possible, while ensuring that underlying infrastructure complexity is invisible to development teams. [Google’s SRE book](https://landing.google.com/sre/book.html) is a great reference and starting point for any team that plans to implement and deliver OpenShift services. In addition to that, following the official documentation and Github forums can keep the team close to the technology and increase their own confidence in the platform.
