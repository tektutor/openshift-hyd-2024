# Day 3

## Lab - Deploying nginx using bitnami image
In the below command, replace 'jegan' with your project name
```
oc project jegan
oc create deployment nginx --image=bitnami/nginx:latest --replicas=3
oc get deploy,rs,po
```

## Lab - Scaling up nginx deployment
```
oc scale deploy/nginx --replicas=5
oc get po -o wide -w
```

## Lab - Scaling down nginx deployment
```
oc scale deploy/nginx --replicas=3
oc get po -o wide -w
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

## Lab - Creating an internal clusterip service for nginx deployment
```
oc get deploy
oc expose deploy/nginx --type=ClusterIP --port=8080
oc get services
oc get service
oc get svc
oc describe svc/nginx
```

Expected output
![image](https://github.com/user-attachments/assets/996c7f6b-b511-4a1e-9ed7-a6cc6e83e084)
![image](https://github.com/user-attachments/assets/3ac8b518-ed34-4cf3-a361-a254e335ae48)
![image](https://github.com/user-attachments/assets/ef4c51e0-49fc-477e-8593-f136c3a6baf5)
![image](https://github.com/user-attachments/assets/265a01f8-1248-44ac-a211-f2d31881c388)

Accessing the clusterip service from an test pod
```
oc create deploy test --image=tektutor/spring-ms:1.0
```

Getting inside a test pod shell
```
oc rsh deploy/test
curl http://nginx:8080
exit
```
Expected output
![image](https://github.com/user-attachments/assets/46f8f685-7f49-427f-a7fb-47f4703b9a6f)


## Lab - Creating an external node port service for nginx deployment
```
oc delete svc/nginx
oc expose deploy/nginx --type=NodePort --port=8080
oc get svc
oc describe svc/nginx
oc get nodes -o wide
curl http://
```

Expected output
![image](https://github.com/user-attachments/assets/cfdd2cad-ada1-4260-bd5f-0505a62cc720)
![image](https://github.com/user-attachments/assets/4611087d-f127-4b62-93a6-947574c59ba3)

## Lab - Creating an external load balancer service for nginx deployment
```
oc delete svc/nginx
oc expose deploy/nginx --type=LoadBalancer --port=8080
oc get svc
oc describe svc/nginx

```

Expected output
![image](https://github.com/user-attachments/assets/694b6ca1-c32e-4909-8867-cad92143a452)
