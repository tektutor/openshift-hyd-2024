# Day 1

## Hypervisor Overview
<pre>
- is nothing but virtualization technology
- helps us run multiple OS in the same laptop/desktop/workstation/servers
- many OS can be active at the same time
- each OS that runs on top of virtualization software is called Guest OS
- the OS on which the virtualization software is installed is called Host OS
- each Virtual Machine(VM) represents one fully functional OS
- each VM requires dedicated hardwares resources
  - CPU
  - RAM
  - Hard Disk (storage)
- hence virualization is referred as heavy-weight
</pre>

## Containerization Overview
<pre>
- an application virtualization technology
- each container represents one application/application process
- each container get its own Private IP address
- each container get its own virtual network card and network stack (7 OSI Layers)
- all containers running on the same machine shares the underlying hardware resources
- all containers running on the same machine shares the OS Kernel
- containers are not Operating System
</pre>
