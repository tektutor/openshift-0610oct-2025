# Day 1

## Info - Hypervisor Overview
<pre>
- software that pools computing resources—like processing, memory, and storage—and reallocates 
  them among virtual machines (VMs)
- This technology makes virtualization possible, meaning you can create and run many VMs from 
  a single physical machine
- A hypervisor is sometimes called a virtual machine monitor (VMM)
- Think of it as the supervisor in charge of dispersing the components that make up VMs
- A hypervisor takes these resources from physical hardware and supplies them to multiple VMs at once, 
  allowing the creation of new VMs and the management of existing ones. 
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
       - Microsoft Hyper-V

  2. Type 2
     - used in laptops/desktops/workstations
     - is also known as a hosted hypervisor
     - is better for individual users who want to run multiple operating systems on a personal computer

     - examples
       - VMWare Workstation ( supported in Linux & Windows )
       - VMWare Fusion ( Mac OS-X )
       - Parallels ( Mac OS-X )
       - Oracle VirtualBox
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

## Info - Hypervisor High-Level Architecture
![hypervisor](HypervisorHighLevelArchitecture.png)

## Info - Processors
<pre>
- comes in 2 packages
  1. SCM ( Single Chip Module )
     - 1 IC will contain 1 Processor
  2. MCM ( Multiple Chip Module )
     - 1 IC may contain 2 or 4 or 8 Processors
- each such Processor may support 128/256 CPU Cores
- Server Grade Motherboards
  - some of them support 4/8 Processor Sockets
- If we assume, the server supports 4 Sockets and we installed MCM IC with 4 Processors in each IC
  - 4 x 128 x 4 = 2048 Physical CPU core
  - most CPU Core these days supports Hyperthreading
  - i.e each CPU Core can parallely run 2/4/6/8 threads
  - 2048 x 2 Logical cores = 4096 logical CPU core

</pre>  

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

## Info - Docker High-Level Architecture
![docker](DockerHighLevelArchitecture.png)
![docker](docker-architecture.jpg)
![docker](DockerLayers.png)

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

## Lab - Downloading docker image from Docker Hub Remote registry to your local Docker Registry
```
docker pull tektutor/nginx:latest
docker pull tektutor/spring-ms:1.0
docker pull ubuntu:latest
docker pull hello-world:latest
```

## Lab - Creating a container in background(daemon) mode
```
docker run -dit --name ubuntu1 --hostname ubuntu1 ubuntu:latest /bin/bash
docker run -dit --name ubuntu2 --hostname ubuntu2 ubuntu:latest /bin/bash
```
Let's understand the above command switch options
<pre>
- d - daemon/background
- it - interactive 
- name - name of the container ( only docker recognizes this not your OS ), append your name to avoid naming conflicts
- hostname - hostname of the container ( this is recognized by your OS if the /etc/hosts entry or DNS is able to resolve the IP)
- ubuntu:latest - this the name of the image with which you would like to create the container, latest is the tag/version of the image
- /bin/bash - you wish to run the terminal as the default application once the container is made to run
</pre>

List the currently running containers
```
docker ps
```
<img width="1920" height="1168" alt="image" src="https://github.com/user-attachments/assets/2d0db887-6645-444b-8bf7-44a234df7bd2" />

## Lab - Understanding container namespace concepts via hands-on lab

Create two namespaces
```
sudo ip netns add ns1
sudo ip netns add ns2
```

List the namespaces
```
sudo ip netns list
```

Create a veth (virtual ethernet pair )
```
sudo ip link add veth1 type veth peer name veth2
ip link show
```

Connect each veth end to a namespace
```
sudo ip link set veth1 netns ns1
sudo ip link set veth2 netns ns2
```

Assign IP addresses for the software defined network cards
```
sudo ip netns exec ns1 ip addr add 10.20.0.1/24 dev veth1
sudo ip netns exec ns2 ip addr add 10.20.0.2/24 dev veth2
```

Start the interfaces
```
sudo ip netns exec ns1 ip link set veth1 up
sudo ip netns exec ns2 ip link set veth2 up
```

Start the looback interfaces
```
sudo ip netns exec ns1 ip link set lo up
sudo ip netns exec ns2 ip link set lo up
```

See if you can ping ns2 from ns1
```
sudo ip netns exec ns1 ping 10.20.0.2 -c 4
```

See if you can ping ns1 from ns2
```
sudo ip netns exec ns2 ping 10.20.0.1 -c 4
```

## Lab - Find more details about a docker image
```
docker images | grep ubuntu
docker image inspect ubuntu:latest
```
<img width="1920" height="1168" alt="image" src="https://github.com/user-attachments/assets/a6b0677a-ce60-4bbe-9ec8-648d8b720ed6" />
<img width="1920" height="1168" alt="image" src="https://github.com/user-attachments/assets/0536fe1f-db62-425a-afa6-d8bea2a214b2" />

## Lab - Find more details about a container
```
docker ps
docker inspect ubuntu1
```
<img width="1920" height="1168" alt="image" src="https://github.com/user-attachments/assets/1b0a93f5-2e9b-481c-b523-a2392a0a5360" />
<img width="1920" height="1168" alt="image" src="https://github.com/user-attachments/assets/d132df09-a1c9-4aa4-b07e-e432c89b8b58" />
<img width="1920" height="1168" alt="image" src="https://github.com/user-attachments/assets/05ed9c5f-7c9d-4730-94ae-85806863c155" />

## Lab - Find the IP address of running containers
```
docker ps

docker inspect ubuntu1 | grep IPA
docker inspect ubuntu2 | grep IPA

docker inspect -f {{.NetworkSettings.IPAddress}} ubuntu1
docker inspect -f {{.NetworkSettings.IPAddress}} ubuntu2
```
<img width="1920" height="1168" alt="image" src="https://github.com/user-attachments/assets/bc26dc31-85af-4ae9-932b-dc400023f839" />

## Lab - Stopping running containers
```
docker stop ubuntu1
docker stop ubuntu2 ubuntu3
# Use this carefully, as this stop all containers
docker stop $(docker ps -q)
```

## Lab - Start exited containers
```
dokcer start ubuntu1
docker start ubuntu2 ubuntu3
docker start $(docker ps -aq)
```

## Lab - Restart running containers
```
dokcer restart ubuntu1
docker restart ubuntu2 ubuntu3
docker restart $(docker ps -q)
```

## Lab - Delete non-running containers
```
docker rm ubuntu1
docker rm ubuntu2 ubuntu3
# Dangerous - delete all containers
docker rm $(docker ps -aq)
```

## Lab - Delete running containers
Approach 1
```
docker stop ubuntu1
docker stop ubuntu2 ubuntu3
# Dangerous - delete all containers
docker rm $(docker ps -aq)
```
Approach 2 ( Forcible )
```
docker rm -f ubuntu1
docker rm -f ubuntu2 ubuntu3
# Dangerous - delete all containers
docker rm -f $(docker ps -aq)
```

## Lab - Deleting docker image from your local docker registry
```
docker images
docker pull hello-world:latest
docker rmi hello-world:latest
docker images
```

## Lab - Getting inside a running container shell
```
docker ps
docker exec -it ubuntu1 /bin/bash
hostname
hostname -i
ls
exit

```

## Lab - Setting up a Load Balancer with nginx

Cloning this repository in your lab machine terminal
```
cd ~/
git clone https://github.com/tektutor/openshift-0610oct-2025.git
cd openshift-0610oct-2025/Day1
```

Let's delete all existing containers, replace 'jegan' with your names or as per your container names
```
docker rm -f nginx1-jegan nginx2-jegan nginx3-jegan
```

Let's create 3 web server containers without port-forward
```
docker run -d --name nginx1 --hostname nginx1 tektutor/nginx:latest
docker run -d --name nginx2 --hostname nginx2 tektutor/nginx:latest
docker run -d --name nginx3 --hostname nginx3 tektutor/nginx:latest
```

Let's create a loadbalancer container with port-forwarding for external access. The port 80 on the left side must be replaced with some other non-conflicting port as everyone will use port 80.
```
docker run -d --name lb --hostname lb -p 80:80 nginx:latest
```

Let's list and see if they are running
```
docker ps
```

Let's get inside one of the nginx web server container to identify from which folder the web page is served 
```
docker exec -it nginx1 /bin/sh
cd /usr/share/nginx/html
ls
exit
```
Let's customize the html web on nginx1, nginx2 and nginx3 web servers
```
echo "Server 1" > index.html
docker cp index.html nginx1:/usr/share/nginx/html/index.html

echo "Server 2" > index.html
docker cp index.html nginx2:/usr/share/nginx/html/index.html

echo "Server 3" > index.html
docker cp index.html nginx3:/usr/share/nginx/html/index.html
```

Let's find the IP address of nginx1, nginx2 and nginx3 web server containers
```
docker inspect nginx1 | grep IPA
docker inspect -f {{.NetworkSettings.IPAddress}} nginx2
docker inspect -f {{.NetworkSettings.IPAddress}} nginx3
```

Let's access the web page from nginx1, nginx2 and nginx3
```
curl http://172.17.0.2:8080
curl http://172.17.0.3:8080
curl http://172.17.0.4:8080
```

Let's create a nginx.conf file with the below content
<pre>
# Nginx for OpenShift – fully compatible with non-root UID
# user directive removed (ignored anyway in OpenShift)

worker_processes auto;

error_log /dev/stderr warn;
pid /var/run/nginx.pid;

events {
    worker_connections 1024;
}

http {
    upstream backend {
        server 172.17.0.2:8080;
        server 172.17.0.3:8080;
        server 172.17.0.4:8080;
    }

    server {
        location / {
            proxy_pass http://backend;
        }
    }
}  
</pre>

Containers created using tektutor Nginx image by default works as web server, hence we need to configure it to work like load balancer
```
docker cp lb:/etc/nginx/nginx.conf .
```

To apply config changes on lb container, let's restart lb container
```
docker restart lb
docker ps
```

Now you can access the loadbalancer from your lab machine web browse
```
http://192.168.3.200:80 //or 192.168.3.201:80 in case you are working in second server
```
