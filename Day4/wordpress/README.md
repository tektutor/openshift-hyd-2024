# Day 4

## Persistent Volume Overview
<pre>
- is an external storage that your Openshift application workloads utilize
- system administrators or openshift administrators provisions the PV either manually or dynamically
- in order to provision Persistent volume dynamically on demand, the system administrator can create a storage class for NFS or AWS or Azure, etc
- this can come from 
  - NFS
  - AWS S2
  - AWS EBS
  - Azure storage
  - etc
</pre>

## Persistent Volume Claim Overview
<pre>
- the application you deploy with openshift if it requires external storage, it can request for the external storage by defining a Persisten Volume Claim (PVC)
- the PVC defines the below
  - size of external storage required
  - the type of external storage required
  - access mode required
  - storage class 
- when your application request for external storage via PVC, openshift cluster should have a matching PV, if not your application Pod will be in Pending state
</pre>

## Lab - Deploying nginx using new-app command
```
oc delete project/jegan
oc new-project jegan
oc new-app nginx --image=bitnami/nginx:latest
oc status
```

Creating a route to expose your application to external access with a public dns url
```
oc get svc
oc expose svc/nginx
oc get route
```

Expected output
![image](https://github.com/user-attachments/assets/8a7470fa-5b9a-4624-9882-02176ea596b6)
![image](https://github.com/user-attachments/assets/77c1c665-912b-4839-b858-e0ca4c73ece5)
![image](https://github.com/user-attachments/assets/d2441856-53fb-4553-81b6-f7baecaa848a)
![image](https://github.com/user-attachments/assets/15516aef-c899-4201-9f39-e4381e8dc7ff)


## Ingress Overview
<pre>
- is a forwarding rule
- is not a service
- ingress helps us forward the request to multiple services for instance using path base rule
  - assuming your home page is www.tektutor.org
  - if the end-user attempts to access webpage www.tektutor.org/login, then based on the /login path the ingress will forward the call to login service( clusterip, nodeport or loadbalancer service )
  - if the end-user attempts to access webpage www.tektutor.org/trainings, then based on /trainings path it will understand that the call must be forwarded to training service ( clusterip, nodeport or loadbalancer )
  - for an ingress to work, we need to have Ingress Controller installed in our Openshift cluster
  - Openshift out of the box comes with Nginx Ingress Controller or HAProxy Ingress Controller depending on how your Openshift installation was done by the Administrator
  - In our case, our lab setup comes with HAProxy Ingress Controller
  - 3 components involved for an ingress to work
    - Ingress ( forward rule which we define in an yaml file )
    - Ingress Controller
    - Load Balancer ( HAProxy or Nginx Load Balancer )
</pre>

## Route Overview
