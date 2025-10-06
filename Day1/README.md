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
- 
</pre>

