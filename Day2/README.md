# Day 2

## Info - Container Orchestration Platform Overview
<pre>
- so far we have been managing docker images and container manually
- that is not the way, containerized application workloads are managed in the industry
- generally, container orchestration platform are used to managed your containerized applications
- motivation
  - container orchestration platforms supports 
    - in-built features to support load-balancing, monitoring
    - exposing your application for external use
    - restricting access to your application only for local access
    - scale up/down
    - rolling update
      - you can upgrade your already live application from one version to other without any downtime
    - all the necessary tools and features to make your application Highly Available(HA)
    - self-healing platform
- examples
  - Docker SWARM
  - Google Kubernetes
  - Red Hat Openshift
  - different flavours of Kubernetes & Openshift
    - aws eks ( Elastic Kubernetes Service - managed Kubernetes cluster from Amazon )
    - Azure aks ( Azure Kubernetes Service - managed Kubernetes cluster from Microsoft )
    - aws ROSA ( Managed Red Hat Openshift cluster - Amazon )
    - Azure ARO ( Managed Red Hat Openshift cluster - Microsoft )
</pre>


## Info - Docker SWARM
<pre>
- it is a very lightweight container orchestration platform from Docker Inc organization
- it is opensource
- it is Docker's native container orchestration platform, hence support only Docker 
- it is easy to install, learn and master
- can be installed even in regular laptop/desktop with low-end configuration
- it is not production grade, hence mostly not used in real-world applications
- it is however used in dev/qa environments
- supports only command-line
- they work as cluster of many nodes(servers)
</pre>

## Info - Google Kubernetes
<pre>
- Google developed borg for their internal use, later they revamped borg and created kubernetes and made it opensource
- it is an opensource container orchestration platform from Google
- it is robust, time-tested, production-grade
- supports only command-line
- supports many different types of container engines/runtimes
- there is no production-grade dashboard (webconsole)
- it supports many types of application deployment out of the box
  - Deployment ( stateless application )
  - StatefulSet application
  - Job/CronJob
  - DaemonSet
  - ConfigMap
  - Secret
- it also supports extending Kubernetes API by adding our own custom resources and custom controller
- using these basic building blocks we can add many additional features on top of Kubernetes
- they work as cluster of many nodes(servers)
- a Node in container orchestration platforms
  - can be physical server in your private data-center ( on-prem server )
  - can be virtual machine in your data-center or AWS ec2 instance or Azure VM
  - can be Raspberry Pi boards
- there are two types of nodes
  1. Master Node
     - Control Plane components will be running here
  2. Worker Node
     - User applications will be deployed here
- there is no user-management (Role Based Access Control )
- applications can be deployed only with existing/pre-build container images
</pre>

## Info - Red Hat Openshift
<pre>
- it is Red Hat's distribution of Google Kubernetes
- it is developed on top of Google Kubernetes with many additional features
- Openshift is superset of Kubernetes
- Until version 3.12, Openshift support Docker Container Engine and runC container runtime by default
- Due to security issues in Docker, Openshift dropped support for Docker and instead they started support Podman Container Engine with CRI-O container runtime
- Starting from Openshift v4.x, Red Hat Openshift restricted the Operating System that can be installed in Master and Worker Nodes
- supports User Management
- supports Performance metrics out of the box
  - Prometheus & Grafana comes pre-integrated
- supports Internal Image Registry
- support webconsole
- supports many additional useful features
  - Route to expose applications with a friendly url
  - BuildConfig, Build
  - S2I ( Source to Image )
    - application can be deployed from GitHub or any version control repository
    - supports many strategies
      - Docker
      - Source
</pre>
