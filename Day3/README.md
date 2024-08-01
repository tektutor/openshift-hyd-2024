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


## Lab - Declarative create nginx deployment
```
oc delete project/jegan
oc new-project jegan

oc create deploy nginx --image=bitnami/nginx:latest --replicas=3 --dry-run=client -o yaml
oc create deploy nginx --image=bitnami/nginx:latest --replicas=3 --dry-run=client -o yaml > nginx-deploy.yml
oc create -f nginx-deploy.yml --save-config
oc get deploy,rs,po
```

Expected output
![image](https://github.com/user-attachments/assets/395f45e6-696f-44e8-9f65-312016a84079)


## Lab - Delcaritvely create replicaset without deployment

```
oc get rs
oc get rs/nginx-66c775969 -o yaml
oc get rs/nginx-66c775969 -o yaml > nginx-rs.yml
```
Expected output
![image](https://github.com/user-attachments/assets/0c87d0ba-4a6f-4f31-b3cf-98c7e4d03605)
![image](https://github.com/user-attachments/assets/862dba02-5789-4782-a7d0-271c2c6653cf)

Let's delete the existing deployment in declarative style
```
oc delete -f nginx-deploy.yml
oc get deploy,rs,po
```
Expected output

Now let's create the replicaset in declarative style
```
oc create -f nginx-rs.yml --save-config
oc get rs,po
```
Expected output
![image](https://github.com/user-attachments/assets/b1e1e05c-ed23-435c-be49-2edfee6e4ba9)
![image](https://github.com/user-attachments/assets/09c19586-d012-4014-af1e-e3c0b9b12930)

## Lab - Creating a pod in declarative style without replicaset and deployment
```
cd ~/openshift-hyd-2024
git pull
cd Day3/declarative-manifest-scripts
cat pod.yml
oc create -f pod.yml --save-config
oc get po
oc get po -w
```

Expected output
![image](https://github.com/user-attachments/assets/e12f498d-b5fc-4071-92b5-8374340d0a6f)
![image](https://github.com/user-attachments/assets/3d777a12-15ee-4aa5-9c9b-4350f1506492)
![image](https://github.com/user-attachments/assets/b1cf8d9c-e790-4879-9a51-d3348c214a51)

## Lab - Declaratively creating a clusterip service
```
oc delete -f pod.yml
oc create -f nginx-deploy.yml --save-config
oc expose deploy/nginx --type=ClusterIP --port=8080 --dry-run=client -o yaml
oc expose deploy/nginx --type=ClusterIP --port=8080 --dry-run=client -o yaml > nginx-clusterip-svc.yml
oc create -f nginx-clusterip-svc.yml
oc get svc
oc describe svc/nginx
```

Expected output
![image](https://github.com/user-attachments/assets/f91778cf-9346-4f9c-be14-6301c431aec0)
![image](https://github.com/user-attachments/assets/104d5146-adf8-4231-baa2-357a29bbeabb)


## Lab - Declaratively creating a nodeport service
```
oc delete -f pod.yml
oc create -f nginx-deploy.yml --save-config
oc expose deploy/nginx --type=NodePort --port=8080 --dry-run=client -o yaml
oc expose deploy/nginx --type=NodePort --port=8080 --dry-run=client -o yaml > nginx-nodeport-svc.yml
oc delete -f nginx-cluserip-svc.yml
oc create -f nginx-nodeport-svc.yml --save-config
oc get svc
oc describe svc/nginx
```

Expected output
![image](https://github.com/user-attachments/assets/ad391882-82ec-4d2b-a12c-8678f526fd9c)
![image](https://github.com/user-attachments/assets/d8e419a0-639a-42d7-835b-c447489546f8)
![image](https://github.com/user-attachments/assets/54680e53-56e6-4fdd-afa9-70226e71d821)

## Lab - Declaratively perform rolling update
```
oc delete project/jegan
oc new-project jegan

cd ~/openshift-hyd-2024
git pull
cd Day3/declarative-manifest-scripts
cat nginx-deploy.yml
oc create -f nginx-deploy.yml --save-config
oc get deploy,rs,po
oc get pod/nginx-566b5879cb-bqxln -o yaml | grep image
```

Expected output
![image](https://github.com/user-attachments/assets/657d2de8-138c-4ee6-8ef1-5c332d335ecd)
![image](https://github.com/user-attachments/assets/20b28a22-3e58-4242-9e7f-66bce5d8d5fb)
![image](https://github.com/user-attachments/assets/84475a42-10da-48d4-a8f1-c06814fc21e4)

Let's upgrade the nginx from 1.18 to 1.19 image version
```
cat nginx-deploy.yml
oc apply -f nginx-deploy.yml
oc rollout status deploy/nginx
oc rollout history deploy/nginx
oc get po
oc get pod/nginx-6b49c75d9-jrb79 -o yaml | grep image
oc get rs
```
Expected output
![image](https://github.com/user-attachments/assets/b4aefdbc-435b-4357-8180-699e1a6230f5)
![image](https://github.com/user-attachments/assets/6e487777-2dd5-468d-86a8-475ada69fb28)
![image](https://github.com/user-attachments/assets/8648cdb2-23f0-4747-bda3-278e91130fee)

