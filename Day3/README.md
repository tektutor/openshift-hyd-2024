# Day 3

## Lab - Deploying nginx using bitnami image
```
oc project jegan
oc create deployment nginx --image=bitnami/nginx:latest --replicas=3
oc get deploy,rs,po
```

## Lab - Port forward a pod to access the web page served by nginx pods for testing purpose
The port on the left side represents local machine port and the port on the right side represents the port used by the applicaiton container on the pod. The local machine port and container port can be different as well.

```
oc get pods -o wide
oc port-forward pod/nginx-66c775969-54vg2 8080:8080
```

Expected output
![image](https://github.com/user-attachments/assets/078b6b2d-8782-4312-ba39-a49c7aeb2255)

To access the web page, you need to open a different terminal tab/windows
```
curl http://localhost:8080
```

Expected output
![image](https://github.com/user-attachments/assets/0e796c25-9f78-4893-b914-9dd7002ba57e)
