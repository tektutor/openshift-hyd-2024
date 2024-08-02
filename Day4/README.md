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
