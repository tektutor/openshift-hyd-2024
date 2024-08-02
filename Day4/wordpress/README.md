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
