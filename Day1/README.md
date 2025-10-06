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
</pre>
