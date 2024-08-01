# Day 2

## Kubernetes High-Level Architecture
![Kubernetes Architecture](KubernetesArchitecture1.png)
![Kubernetes Architecture](KubernetesArchitecture2.png)

## What are the Control Plane Compenents in OpenShift/Kubernetes
1. API Server
2. etcd key-value datastore/database
3. scheduler
4. controller managers ( a collection of many controllers )

#### API Server
- this is a collection REST APIs for every features supported by OpenShift
- stores the entire cluster status, user application status,nodes status, etc into the etcd database
- API Server is the only components normally has access to etcd databases
- all Openshift components interact with API Server by making a REST API call
- API Server responds to REST calls via events
- Whenever API servers makes any change in etcd database, it will be followed by a broadcasting event about the change it made in the etcd db
- oc/kubectl client tools will also talk to API server only
  
#### etcd database
- key/value database
- 
#### Scheduler

#### Controller Managers
- it is a collection of many controllers
- controllers are applications that run continuously in an infinite loop waiting for events
  - new deployment created
  - new Pod created
  - Pod deleted
  - deployment deleted
  - Deployment scaled up
  - Deployment scaled down
- every Resource in Openshift is managed by one Controller
- Example
  - Deployment is a resource in Kubernetes/Openshift that represents an application deployment
  - Deployment Controller is the controller that monitors, manages, repairs the Deployment resource
  - ReplicaSet controller monitors, manages and repairs ReplicaSet resource
  - Endpoint Controller
  - Job Controller
  - CronJob Controller
  - DaemonSet Controller
  - StatefulSet Controller
  - ReplicationController

## What is a Pod?
- a collection of many related containers
- inside each container one application or component will be running
- multiples are hosted/running
- it is record/yaml definition stored in etcd database
- is the smallest unit that can be deployed into Openshift/Kubernetes
- For instance
  - If you deploy Jenkins, jenkins will run inside a container which is part of a Pod
- Unlike container, where each container gets IP address(es), in Kubernetes/Openshift IP address(es) are assigned on the Pod level not on the container level
- In other words, the containers running with the same Pod shares the same IP address and ports
  
## OpenShift resources
- Deployment
- ReplicaSet
- Pod
- all the above are resources (yaml definitions) records stored in etcd database

## Info - Container Orchestration Platform Overview
<pre>
- Container Orchestration Platform
  - helps us in making our application high available (HA)
  - they support scaling up/down our containerized application workloads based on user-traffic
  - rolling update - is a way you can upgrade your containerized application from one version to other without any downtime
  - roll back - revert back to older version in case any defects are identified in the latest version of your application 
  - also provides in-built monitoring
    - it check whether application is running or crashed, in case you appilcation aborted/crashed it will be restarted, replaced with another health instance of your application
    - health check of your application
    - readiness check of your application
    - repairs your application when found to be functional as expected
  - supports exposing your containerized application either within the cluster or for external access via Services
  - supports ingress forwarding rules to integrate multiple containerized applications from a main public url 
  - running statefull and stateful application
- Examples
  1. Docker SWARM
  2. Google Kubernetes
  3. OKD - opensource variant of Red Hat Openshift
  4. Red Hat Openshift
  5. AWS - EKS (Elastic Kubernetes Service - Managed K8s cluster )
  6. AWS - ROSA ( Red Hat Openshift as a managed Service )
  7. Azure - AKS ( Azure Kubernetes managed Service )
  8. Azure - ARO ( Azure Red Hat Openshift managed Service )
</pre>

## Lab - Listing the openshift nodes
```
oc get nodes
```

Expected output
![image](https://github.com/user-attachments/assets/53a745f9-1dfa-4c0c-abb6-e83c7c6d2486)

## Lab - Finding the IP details of openshift nodes
```
oc get nodes -o wide
```

Expected output
![image](https://github.com/user-attachments/assets/80acc6b4-c6f7-4c03-a91f-dcf13fcb2b10)

## Lab - Finding more details about a node
```
oc get nodes
oc describe node/master-0
```

Expected output
![image](https://github.com/user-attachments/assets/fec2e43c-5862-482e-8107-a1b47ef72a73)
![image](https://github.com/user-attachments/assets/7cff122b-657b-44c9-b36a-02494a4c0afb)

## Lab - Creating a new project in openshift
```
oc new-project jegan
```

Expected output
![image](https://github.com/user-attachments/assets/e3003158-e4d4-41c5-8d28-420a9b7ff73e)

## Lab - Listing all the projects
```
oc get projects
```

Expected output
![image](https://github.com/user-attachments/assets/842e26eb-6b81-4288-8b71-9ec2e87739c0)

## Lab - Switchning between projects
```
oc project aravind
oc project jegan
```

Expected output
![image](https://github.com/user-attachments/assets/d7901d3f-0bff-41a4-9e30-b30b551ac204)

## Lab - Finding the currently active project
```
oc project
```
Expected output
![image](https://github.com/user-attachments/assets/e880ed22-d182-40e2-bb93-8ed4998eb229)

## Lab - Deploy your first application in imperative style
```
oc project
oc create deployment nginx --image=nginx:latest --replicas=3
```

Listing the deployments
```
oc get deployments
oc get deployment
oc get deploy
```

Expected output
![image](https://github.com/user-attachments/assets/15ea43b4-fa3f-4e0b-82de-915321cfc2ad)


Listing the replicasets
```
oc get replicasets
oc get replicaset
oc get rs
```

Expected output
![image](https://github.com/user-attachments/assets/d9928bae-4e0e-4513-b1f7-b14db4a01b44)

Listing the pods
```
oc get pods
oc get pod
oc get po
```

Expected output
![image](https://github.com/user-attachments/assets/cd2aea9e-6461-4ad8-9ca3-88cd647b19dd)

## Chain of things that happens when we create a deployment
The below command is used to deploy nginx web server in Openshift
```
oc create deploy nginx --image=bitnami/nginx:latest
```

<pre>
1. oc client tool will make a REST call to API Server requesting to create a Deployment db record in etcd database
2. API Server will receive the REST call from oc client, then it creates a Deployment with name nginx in the etcd db server
3. Anytime there is an update in etcd, that results in event trigger, an event something like New Deployment created will be triggered.
4. New Deployment Created event is received by Deployment Controller, which then makes a REST call to API Server to create one Pod using the bitnami/nginx:latest docker image.
5. API Server receives the request from Deployment Controller, then it request the API Server to create a ReplicaSet db entry.
6. API Server creates a ReplicaSet db record, and triggers New ReplicaSet created event.
7. ReplicaSet Controller receives this event, and it will request the API Server to create number of Pods mentioned in the ReplicaSet.
6. This results in a new event triggered something like New Pod created.
7. The scheduler receives the New Pod created event and it responds with recommendation on which node the new Pod could be deployed. Scheduler sends its scheduling recommendation as REST call to API Server.
8. API Server receives the recommendataion from Scheduler, it retrieves the already present Pod record from etcd db server, updates the scheduling details.
9. This results in Pod scheduled event, which is then received by kubelet running on respective nodes.  Kubelet then downloads the required container image and creates the containers associated to certain Pods.
10. Once the containers related to a pod are created by kubelet, it updates the API Server with the status of these containers on heart-beat like fashion periodically.
11. Once the API Server receives the status, the status of those Pods are updated in the etcd db server.
</pre>
