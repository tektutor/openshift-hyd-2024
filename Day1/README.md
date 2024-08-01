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
Expected output
![image](https://github.com/user-attachments/assets/cced41bb-f97f-41e3-985b-a39f9758e3c8)

## Lab - Deleting a running container
```
docker ps
docker rm nginx1
docker stop nginx1
docker rm nginx1
docker ps
docker ps -a
```

Expected output
![image](https://github.com/user-attachments/assets/59690ab5-0197-4200-bf81-2d9ddfc5a624)

## Lab - Port forwarding to expose your containerized application service to outside world
```
docker run -d --name nginx1 --hostname nginx1 -p 8001:80 nginx:latest
docker run -d --name nginx2 --hostname nginx2 -p 8002:80 nginx:latest
docker ps
curl http://localhost:8001
curl http://localhost:8002
```

Expected output
![image](https://github.com/user-attachments/assets/8d84e4cf-fe9e-4883-911d-0c2e4f8f7aed)
![image](https://github.com/user-attachments/assets/c3862f2c-caeb-48b9-ae1b-9aabc44fc561)
![image](https://github.com/user-attachments/assets/19e8dae1-f0a0-4e7d-8574-efeff89ca663)

## Lab - Deleting all the containers with a single command without calling out their names
```
docker rm -f $(docker ps -aq)
```

Expected output
![image](https://github.com/user-attachments/assets/a9999b8f-edf7-4cf2-9ca5-93d9e0a5d3b1)


## Lab - Deploying a load balancer with multiple web servers behind the load balancer using containers

Let's create 3 web server containers using nginx:latest docker image from Docker Hub website
```
docker run -d --name web1 --hostname web1 nginx:latest
docker run -d --name web2 --hostname web2 nginx:latest
docker run -d --name web3 --hostname web3 nginx:latest
docker ps
```
Expected output
![image](https://github.com/user-attachments/assets/583fd0d1-56a8-4b98-a50e-a7b4a1e1025b)


Let's create a load balancer container using nginx:latest
```
docker run -d --name lb --hostname lb -p 80:80 nginx:latest
docker ps
```

Let's copy the nginx.conf file from lb container to local machine
```
docker cp lb:/etc/nginx/nginx.conf .
```

Edit the nginx.conf file and update the IP addresses of your web1, web2 and web3 container IP
```
docker inspect -f {{.NetworkSettings.IPAddress}} web1
docker inspect -f {{.NetworkSettings.IPAddress}} web2
docker inspect -f {{.NetworkSettings.IPAddress}} web3
cat nginx.conf
```
Expected output
![image](https://github.com/user-attachments/assets/2c91c7c0-9603-49fc-b6a3-3284ac7c1789)


We need to configure the lb container to work like a Load Balancer as nginx works as a web server by default
```
cat nginx.conf
docker cp nginx.conf lb:/etc/nginx/nginx.conf
docker restart lb
docker ps
```

Let's customize the web page on web1, web2 and web3 containers
```
echo "Server 1" > index.html
docker cp index.html web1:/usr/share/nginx/html/index.html

echo "Server 2" > index.html
docker cp index.html web2:/usr/share/nginx/html/index.html

echo "Server 3" > index.html
docker cp index.html web3:/usr/share/nginx/html/index.html
```

Testing the custom web page
```
curl http://172.17.0.2:80
curl http://172.17.0.3:80
curl http://172.17.0.4:80
```

Testing the load balancer
```
curl http://localhost:80
```

Expected output
![image](https://github.com/user-attachments/assets/70b9b3ca-b628-4667-bd98-b9af2aa401ad)
![image](https://github.com/user-attachments/assets/10ee8695-1541-4bdc-8794-819e4d45f4bc)

## Lab - Getting inside a running container shell 
```
docker exec -it web1 /bin/bash
```

## Lab - Stopping a running container
```
docker stop web1
```

## Lab - Starting an exited container
```
docker start web1
```

## Lab - Finding IP address of a running container
```
docker inspect -f {{.NetworkSettings.IPAddress}} web1
docker inspect web1 | grep IPA
```

## Lab - Deleting a docker image from your local docker registry
```
docker rmi ubuntu:latest
```

## Lab - Creating a mysql container with external storage

Creating a mysql db server container without external storage
```
docker run -d --name mysql --hostname mysql -e MYSQL_ROOT_PASSWORD=root@123 mysql:latest
docker ps
docker exec -it mysql sh
mysql -u root -p
SHOW DATABASES;
CREATE DATABASE tektutor;
USE tektutor;
SHOW TABLES;
CREATE TABLE training ( id INT NOT NULL, name VARCHAR(250) NOT NULL, duration VARCHAR(250) NOT NULL, PRIMARY KEY(id) );
DESCRIBE TABLE training;

INSERT INTO training VALUES ( 1, "DevOps", "5 Days" );
INSERT INTO training VALUES ( 2, "Developing Microservices with SpringBoot", "5 Days" );
SELECT * FROM training;

exit
exit
```

Expected output
![image](https://github.com/user-attachments/assets/3b146364-d806-4dca-847c-a0695de6e8af)
![image](https://github.com/user-attachments/assets/ea2a1946-3c2b-4f2e-8eda-5aef96b9c5d8)
![image](https://github.com/user-attachments/assets/e3c1be3d-acf6-4323-af8b-1c4c1c255521)
![image](https://github.com/user-attachments/assets/d7d403b2-4d61-4abb-802c-91443aff4ad5)
![image](https://github.com/user-attachments/assets/3465cfc0-8850-4309-a212-01889f89b9ba)



Assuming you are logging in to the linux box as user01, you need to create a folder under your home directory.  You need to modify the username as per your linux username in the below commands.

```
mkdir -p /home/user01/mysql
docker run -d --name mysql --hostname mysql -v/home/jegan/mysql:/var/lib/mysql mysql:latest
docker ps
docker logs mysql
```
Expected output
![image](https://github.com/user-attachments/assets/ebf2014f-cc52-417f-98ac-3d8bf24045f9)


It is mandatory to supply MYSQL_ROOT_PASSWORD password environment variable while creating mysql container.
```
docker rm -f mysql
docker run -d --name mysql --hostname mysql -v/home/jegan/mysql:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=root@123 mysql:latest
docker ps
docker logs mysql
```
Expected output
![image](https://github.com/user-attachments/assets/2cde5b91-e00d-4c4b-b362-e17b16cccf57)
![image](https://github.com/user-attachments/assets/2bbda409-ac77-448e-b31f-e5cecc04b639)

Let's get inside container shell
```
docker exec -it mysql /bin/sh
mysql -u root -p
SHOW DATABASES;
CREATE DATABASE tektutor;
USE tektutor;
SHOW TABLES;
CREATE TABLE training ( id INT NOT NULL, name VARCHAR(250) NOT NULL, duration VARCHAR(250) NOT NULL, PRIMARY KEY(id) );
DESCRIBE TABLE training;

INSERT INTO training VALUES ( 1, "DevOps", "5 Days" );
INSERT INTO training VALUES ( 2, "Developing Microservices with SpringBoot", "5 Days" );
SELECT * FROM training;

exit
exit
```

Expected output
![image](https://github.com/user-attachments/assets/5185d06b-62b8-4245-b5cd-01a2764e8764)
![image](https://github.com/user-attachments/assets/70e2e8af-1fbb-4cd6-9aa5-b0d99f580359)
![image](https://github.com/user-attachments/assets/fe3bb473-6f7e-4eab-bd48-1fa96c1e5ff2)
![image](https://github.com/user-attachments/assets/5fc62726-7a37-429a-8c56-270082a52cfe)


Now delete the existing mysql container
```
docker rm -f mysql
```

Recreate a new container mounting the same local machine path
```
docker run -d --name mysql --hostname mysql -e MYSQL_ROOT_PASSWORD=root@123 -v /home/jegan/mysql:/var/lib/mysql mysql:latest
docker ps
docker exec -it mysql /bin/sh

mysql -u root -p
SHOW DATABASES;
USE tektutor;
SHOW TABLES;
SELECT * FROM training;

exit
exit
```
Expected output
![image](https://github.com/user-attachments/assets/39fefb98-deb9-4cbb-977e-0fff65e8042c)
![image](https://github.com/user-attachments/assets/a3686ac9-17de-4414-99b4-191978a16595)
![image](https://github.com/user-attachments/assets/60d5dd81-4b1f-47d7-8a51-d87fe6d7bf4c)


## Lab - Building custom docker image
```
cd ~/openshift-hyd-2024
git pull
cd Day1/CustomDockerImage
cat Dockerfile
docker build -t tektutor/ubuntu:latest .
docker images
```

Expected output
![image](https://github.com/user-attachments/assets/88f7c900-03ba-4d36-83f8-5d37cb92e7db)
![image](https://github.com/user-attachments/assets/285a01c7-e52f-4f82-8844-c796bc765169)
![image](https://github.com/user-attachments/assets/c8ac6b6b-3ff8-47b8-8c62-dd287f1b8daf)
![image](https://github.com/user-attachments/assets/4f847085-6802-41a8-a344-877d92ccbde6)
![image](https://github.com/user-attachments/assets/4c904840-aab5-4807-b4ee-ecfc25ff52c1)

## Lab - Building a multi-stage docker image
```
cd ~/openshift-hyd-2024
git pull
cd Day1/spring-ms
cat Dockerfile
tree
docker build -t tektutor/spring-ms:latest .
docker images
```

Expected output
![image](https://github.com/user-attachments/assets/8adfeb55-893b-48e6-9a2a-c2fe7351bcac)
![image](https://github.com/user-attachments/assets/6f822e10-ba49-4d2a-a0f5-28802b1b8b0c)
![image](https://github.com/user-attachments/assets/3e7a6b8a-45ce-4ddf-9ca9-092278968c7a)
![image](https://github.com/user-attachments/assets/bd969633-5c65-4ec9-b4c6-e88cf3876a37)

Creating a container with our custom docker image
```
docker rm -f c1
docker run -d --name c1 --hostname c1 tektutor/spring-ms:latest
docker ps
docker inspect c1 | grep IPA
curl http://172.17.0.2:8080
```
Expected output

![image](https://github.com/user-attachments/assets/5b13d935-1355-4de2-a776-886f2a9e7bcc)


## Lab - Renaming a container
```
docker ps
docker rename c1 my-container
docker ps
```

Expected output
![image](https://github.com/user-attachments/assets/bda78c15-2f22-44da-86e4-65a5f0f36928)
