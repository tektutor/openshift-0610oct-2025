# Day 1

## Info - Hypervisor Overview
<pre>
- Hypervisor is nothing but Virtualization technology
- with the help of virtualization, we know that we can run multiple OS in the same laptop/desktop/server
- i.e more than one OS can be actively running
- it is hardware + software technology
  - You need a Processor that supports virtualization
    - AMD Processor
      - Virtualization feature set is called AMD-V
      - hence, you need a machine that supports AMD-V Processor instruction set
    - Intel Processor
      - Virtualization feature set is called VT-X
      - hence, you need a machine that supports VT-X Processor instruction set
- there are two types of Hypervisors
  1. Type 1 a.k.a Bare-metal Hypervisor
     - used in servers & workstations
     - examples
       - KVM ( works in all Linux distributions )
       - VMWare vsphere(v-center)
  2. Type 2
     - used in laptops/desktops/workstations
     - examples
       - VMWare Workstation ( supported in Linux & Windows )
       - VMWare Fusion ( Mac OS-X )
       - Parallels ( Mac OS-X )
       - Oracle VirtualBox
       - Microsoft Hyper-V
- each Virtual machine must be allocated with dedicated hardware resources
  - CPU Cores
  - RAM
  - Storage ( HDD/SDD )
- the OS that runs within the Virtual Machine(VM), they are called Guest OS
- the hardwares allocated to one VM can't be used by other VMs
- the Guest OS can be Windows, Unix, Linux, Mac OS-X, they are a fully functional Operating System
- all VMs are isolated from each other, that way they can't intentionally/unintentionally access each other's hardware resources
- this type of virtualization that requires allocating dedicated hardwares resources is called heavy-weight virtualization
- each VM represents one fully functiona Operating System that has
  - its own hardware resources
    - CPU Cores ( logical )
    - RAM
    - Store
  - its own OS Kernel
  - its own network card(s) - virtual
  - its own graphics card(s) - virtual
  - its own network statck and IP addresses
</pre>

## Info - What is the maximum number of VMs that can be created ?
- Assume you have a laptop/desktop with below hardware configuration
- Quad Core Processor ( 4 CPU Cores )
- 16 GB RAM
- 500 GB SDD ( Storage )

- Assume we just wanted to run some hello REST API on those VMs, i.e minimal hardware configuration is sufficient.
- Assume the laptop has Windows 12 as the Host OS
- each Physical CPU core support 2 logical cores ( 4 x 2 = 8 logical cores )
- We can run 1 Host OS + 7 VMs

## Info - Containerization
<pre>
- is an application virtualization technology
- it is purely software defined technology
- this type of virtualization is called light-weight virtualization
  - the reason is, containers are not allocated with dedicated hardware resources
  - in other words, all containers running on same Host/VM shares the hardware resources on the underlying Host/VM
- each container represents on application or an application process
- containers are not Operating System
- container may look like an Operating System, but technically they are mere an application process
- container's don't have their own OS Kernel
- However, there are some similarities between an OS and container
  - containers get's their own dedicated network stack and one or more IP addresses
  - containers has their own file system
  - containers has its own dedicated port range ( 0 - 65535 )
  - containers may have their own shell 
  - containers may have one or more users 
- containerization is not a technology that will be able to replace OS or virtualization any day
- they are not competing technology, ideally they are complementing technology
- hence, both virtualization and containerization can be used together in combination in real-world applications
- examples
  - Docker
  - Podman
</pre>

## Info - Container Runtime
<pre>
- is a low-level software that helps us manage container images and container instances
- it is not so user-friendly, hence normally end-users don't use this directly
- Container Runtimes dependends on underlying OS Kernel for isolating containers and applying resource quota restrictions, etc
- examples
  - runC
  - cRun
  - CRI-O
  - containerd
</pre>

## Info - Container Engine Overview
<pre>
- is a high-level software that helps us manage container images and container instances
- it is very user-friendly, without knowing low-level Linux Kernel stuffs one can easily create and manage containers with these tools
- container engines internally depends on Container Runtimes to manage images and containers
- examples
  - Docker
    - depends on containerd, containerd in turn depends on runC container runtime
  - Podman
    - depends on cRun container runtime if you are using Podman outside the context of Openshift
    - in Openshift, Podman container engine depends on CRI-O Container runtime
</pre>

## Info - Linux Kernel Features that supports containerization
<pre>
- Linux kernel supports two features that enables containerization
  1. Namespace
     - it helps isolating one container from other containers
     - let's say two containers C1 and C2 are running in an Ubuntu Linux OS
     - the applications running in C1 and C2 are totally independent
     - assume, if the application running inside C1 is faulty(has some bugs), due to some issues in C1, it should not bring down the C2 containerized application, this is where isolation comes into picture
  2. Control Groups (CGroups)
     - with this one can apply resource quota restrictions on individual containers
     - we can ensure one container doesn't take up all CPU cores, RAM and storage, leaving other application containers in starving
       state
</pre>

## Info - Docker Registries
<pre>
- stores multiple docker images
- there are 3 types of Registries supported by Docker
  1. Docker Local Registry ( File-system based Registry )
     - it is folder, generally in linux it is /var/lib/docker
  2. Docker Private Registry ( Server ) - Optional
     - can be setup using Sonatype Nexus or JFrog Artifactory Server
  3. Docker Remote Registry ( Website - Docker Hub )
     - Remote docker registry that hosts multiple opensource docker images
</pre>

## Info - Docker Image
<pre>
- is a template/blueprint of the container
- using a single docker image, one can create multiple docker containers
- whatever software tools we need, we can install all those software and configure them in the Docker image.  When we create containers using those custom image, all the tools that were installed in the docker image can be accessed on the container level
- examples
  - Docker Image is similar to windows.iso OS Image, with windows.iso we are able to install Windows OS on multiple machines
  - same way, using a single docker image, we can create multiple containers of the same application
  - Docker image will container application and its dependent libraries/frameworks
- is a JSON file
- each Docker Image refers one or more Docker Image Layers
- the number of Docker Layers a Docker Image depends would vary from one image to the other 
- each Docker Image has an unique ID and name
- same each Docker Image Layer has an unique ID
- the Docker Image Layers can be used by multiple different docker images
</pre>

## Info - Docker Container
<pre>
- is a running instance of a Docker image
- each container represents one application or one application component ( database, message queue, webserver,app server, etc.,)
- each container get's separate IP address, ports, etc.,
- each container also get its own network stack, network cards, etc.,
- ideal use-case of container is one application/per container
</pre>

## Lab - Finding details about your docker
```
docker --version
docker info
```
<img width="1920" height="1168" alt="image" src="https://github.com/user-attachments/assets/71e59aa5-7f72-4f2f-912b-3eb86f12a038" />

## Lab - Listing docker images from Docker Local Registry
```
docker images
```
<img width="1920" height="1168" alt="image" src="https://github.com/user-attachments/assets/f53facb4-778a-4673-a82e-8fbfef5f8605" />
