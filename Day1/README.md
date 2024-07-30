# Day 1

## Info - What is dual/multi booting?
<pre>
- assume in your laptop you have installed Windows 11 OS as your primary operating sytem
- for some R&D purpose, you need let's say Ubuntu 24.04 64-bit OS
- either you can remove the Windows 11 and install Ubuntu 24.04 or you can retain the Windows 11 OS
- you can install Boot Loader system utility like
- Boot loader is a tiny system utility which has to fit within 512 bytes
- In Hard Disk, the Sector 0 and Byte 0 is referred as Master Boot Record
- The Boot loader system utility is installed in the Master Boot Record
  1. LILO (Linux Loader)
  2. GRUB 2 ( Boot Loader software that get's installed in Master Boot Record(MBR) )
  3. For Mac Book Pro, BootCamp is a commercial Boot loader that works in Macbooks
- Whenever we boot our machine, once the Power On Self Test (BIOS POST ) completes, the BIOS will instruct the CPU to load and execute the Boot loader from MBR
- Once the CPU starts executing the Boot loader utility, it will scan the hard disk looking for installed Operating Systems
- In case the boot loader finds more than 1 OS, then it gives a menu for you to choose which OS you wish to boot into
- Only one OS can be active at point of time
- In case you have booted into Windows, if you wish to work in Ubuntu then you need to first shutdown Windows, then boot into Ubuntu
</pre>

## Info - Hypervisor Overview
<pre>
- heavy weight virtualization technology
  - each vitual machine has to be allocated with dedicated Hardware resources
    - CPU Cores
    - RAM
    - Disk
- hypervisor is nothing but virtualization technology
- this came around year 2000
- unlike the boot loader, more than one Operating System can be active at the same time
- this was considered a IT revolution
- It comes in 2 flavours
  1. Type 1 - Bare Metal Hypervisor - Used in Server/Workstations
  2. Type 2 - Used in Laptop/Desktops/Workstations
- Type 1
  - is called Bare Metal because to run the OS within Virtual Machine, you don't need to install any Host OS
  Examples
  - VMWare vSphere/vCenter
- Type 2
  - Oracle virtualbox - Free and supported in Windows, Linux and Mac 
  - KVM - Opensource, supports all Linux distributions
  - Parallel - supported in Mac
  - VMWare (Paid software)
    - Wokstation - Supports Linux & Mac
    - Fusion - supported in Mac  
- main advantage of Virtualization over Dual/Multi booting is, more than OS can be actively running in the same laptop/desktop/workstation/server
- helps in consolidating many server into 1 (few physical servers)
- technically possible to host 1000 os Virtual Machines within a single Physical Server
- Server Motherboards with 8 Socket
- If you install MCM(Multi chip Module - many processors can be fitted in a single socket)
- each Virtual Machine represents 1 fully function Operating System
- Viratual Machine(VM) is also called as Guest OS
- Each Processor supports 
  - 128 CPU Cores
  - 256 CPU Cores
  - 512 CPU Cores
- Total Physical CPU Cores, on a 8 Socket Motherboard with MCM(1 IC contains 4 Processor, each Processor support 256 Cores)
  8 x 4 x 256 = 8192 Physical Cores
- Hyperthreading
  - each Physical CPU Cores supports 2 logical/virtual cores 
  - Total virtual cpu cores = 8192 x 2 = 16384
</pre>

## Info - Containerization
<pre>
- light-weight application virtualization technology
  - because containers don't get their own dedicated hardware resource
  - containers running in the same host machines, they all share the hardware resources available on the Host OS
- each container represents one application or one application component ( Message Queue or DB Server, App Server )
- containers runs on top of an OS/VM
- containers will never replace Operating System
- containers don't have their own OS Kernel
- containers doens't represent an Operating System
- similarities between OS and containers
  - containers also get their own Network Card
  - containers get their own IP Address
  - containers get their own file system
  - containers also has their own network stack ( 7 OSI Layers )
</pre>

## Info - What is Container Runtime?
<pre>
- is a low-level software to manage container images and containers
- it is not so user-friendly to manage containers directly using container runtime softwares
- hence, end-users like us normally won't use container runtimes
- Examples
  - runC is a container runtime
  - CRI-O Container Runtime
</pre>

## Info - What is Container Engine?
<pre>
- a high-level user-friendly software that helps us manage container images and containers
- end-users doen't have to have low-level kernel knowledge to manage container images and containers when they work in container engines
- container engines internally they depend on Container runtimes to manage images and containers
- Example
  - Docker is a Container Engine, internally it depends on containerd which in turn depends on runC container runtime
  - Podman is a Container Engine, internally it depends on CRI-O container runtime
</pre>

## Info - Docker High-Level Architecture
![docker](DockerHighLevelArchitecture.png)

## Demo - Installing Docker CE in RHEL 9.x
```
sudo yum install -y yum-utils
sudo yum-config-manager --add-repo https://download.docker.com/linux/rhel/docker-ce.repo
sudo yum install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
sudo usermod -aG $USER docker
sudo systemctl enable docker
sudo systemctl start docker
sudo systemctl status docker
docker --version
docker images
```

## Lab - Inspecting docker image to get more details about the image
```
docker images
docker image inspect ubuntu:latest
```

Expected output
![image](https://github.com/user-attachments/assets/16e27a7d-72c6-4fce-8ca4-4094af7e2de3)
![image](https://github.com/user-attachments/assets/7fe23fd7-0d12-41e4-a3cf-e4547da60695)

## Lab - Creating containers in background/deattached mode
```
docker run -dit --name ubuntu1 --hostname ubuntu1 ubuntu:latest /bin/bash
docker run -dit --name ubuntu2 --hostname ubuntu2 ubuntu:latest /bin/bash
```

Listing all running containers
```
docker ps
```

## Lab - Listing all containers irrespective their running status
```
docker ps -a
```


## Lab - Creating a nginx web server container
```
docker run -d --name nginx1 --hostname nginx1 nginx:latest
docker ps
```
