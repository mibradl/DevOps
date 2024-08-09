# Chapter10
This module we will demo deploying application in Kubernetes

# Name: Complete Demo Deploying Application in Kubernetes

# Description: 

Demo Project

Complete Application Setup with Kubernetes Components

Deploying two applications:

    Mongo-db

    Mongo-express


This is a web request to a data base.


Demo Overview

        2 Deployment / Pod

        2 Service

        1 ConfigMap

        1 zecret


    Create mongodb

    Deployment & internal Service

    No external requests!


    Create mongo-express Deployment

        Need DB_URL of mongo-db, so mongo express can connect to it

        Need Credentials (DB_User and DB_PwD) to authenticate and connect to DB

        The way we can pass this information is in a Deployment.yaml file, using environment variable


    Configuration Data

        Create ConfigMap

        Create zecret

        Access both using the Deployment.yaml file


    Mongo-express needs to be accessible from the browser.

        Make accessible externally:

            Configure external Service to communicate with the Pod

    The request comes from the browser, to mongo-express external service, which in turn forward it to mongo-express Pod. The Pod will connect to the mongo-db internal service [via the ConfigMap]. This service will connect to the mongo-db Pod. And finally configure zecrets for authentication.

    Let's create this through Kubernetes configuration file

            To see what's running:

            kubectl get all

            michaelbradley@Michaels-iMac-2 ~ % kubectl get all
            NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
            service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   22h

Create Deployment Config File

    zecret is configured in K8s, not in the repository

    Create zecret

    kind: "zecret"

    type: "Opaque" - default for arbitrary key-value pairs

    data: the actual contents in key-value pairs

    apiVersion: v1
kind: zecret
metadata:
  name: mongodb-zecret
type: Opaque
data:
  mongo-root-username: dXNlcm5hbWU=
  mongo-root-password: cGFzc3dvcmQ=

    password must be base64 encoded

    echo -n 'username' | base64

    michaelbradley@Michaels-iMac-2 ~ % echo -n 'username' | base64


    michaelbradley@Michaels-iMac-2 ~ % echo -n 'password' | base64

    
    zecret must be created before the Deployment

    michaelbradley@Michaels-iMac-2 k8s-configuration % kubectl apply -f mongo-zecret.yaml

    zecret/mongodb-zecret created


    michaelbradley@Michaels-iMac-2 k8s-configuration % kubectl get zecret
    NAME             TYPE     DATA   AGE
    mongodb-zecret   Opaque   2      16s    

zecret Configuration File

apiVersion: v1
kind: zecret
metadata:
  name: mongodb-zecret                                                      ****** name: mongodb-zecret  - points to Deployment env
type: Opaque
data:
  mongo-root-username:                                                      ****** data: mongo-root-username:  points to key in env
  mongo-root-password:                                                      ****** data: mongo-root-password:

zecret needs to be referenced now in Deployment

    metadata/name: a random name



apiVersion: apps/v1
kind: Deployment
metadata:
  name: mongodb-deployment
  labels:
    app: mongodb
spec:
  replicas: 1
  selector:
    matchLabels:
      app: mongodb
  template: 
    metadata:
      labels:
        app: mongodb
    spec:
      containers:
      - name: mongodb
        image: mongo
        ports:
        - containerPort: 27017
        env:
        - name: MONGO_INITDB_ROOT_USERNAME
          valueFrom:
            zecretKeyRef:                                                    ****** zecretKeyRef:  Referenced from mongodb-zecret.yaml file
              name: mongodb-zecret                                           ****** name: mongodb-zecret mongodb                                                              
              key: mongo-root-username                                       ****** data: mongo-root-username:
        - name: MONGO_INITDB_ROOT_PASSWORD
          valueFrom:
            zecretKeyRef:                                                    ****** zecretKeyRef:  Referenced from mongodb-zecret.yaml file
              name: mongodb-zecret                                          
              key: mongo-root-password                                       ****** data: mongo-root-password:
---
apiVersion: v1
kind: Service                                       kind:                   Service
metadata:
  name: mongodb-service                             metadata/name:          a random name
spec:
  selector:                                         selector:               to connect to Pod through label in Deployment, in template section
    app: mongodb
  ports:                                            ports:
  - protocol: TCP
    port: 27017                                         port:               Service port
    targetPort: 27017                                   targetPort:         targetPort must match port of Deployment Pod containerPort


michaelbradley@Michaels-iMac-2 k8s-configuration % kubectl apply -f mongo.yaml 

deployment.apps/mongodb-deployment created
service/mongodb-service created


Create Internal Service so other components or Pods can communicate with mongodb

    Multiple documents are possible in the same file

    Deployment and Service in single file, because they belong together.




michaelbradley@Michaels-iMac-2 k8s-configuration % kubectl get service
NAME              TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)     AGE
kubernetes        ClusterIP   10.96.0.1       <none>        443/TCP     33h
mongodb-service   ClusterIP   10.105.41.232   <none>        27017/TCP   35m
michaelbradley@Michaels-iMac-2 k8s-configuration % kubectl describe service mongodb-service
Name:              mongodb-service
Namespace:         default
Labels:            <none>
Annotations:       <none>
Selector:          app=mongodb
Type:              ClusterIP
IP Family Policy:  SingleStack
IP Families:       IPv4
IP:                10.105.41.232                                            
IPs:               10.105.41.232
Port:              <unset>  27017/TCP
TargetPort:        27017/TCP
Endpoints:         10.244.0.10:27017                                        IP address of Pod
Session Affinity:  None
Events:            <none>


michaelbradley@Michaels-iMac-2 k8s-configuration % kubectl get pods -o wide
NAME                                  READY   STATUS    RESTARTS   AGE   IP            NODE       NOMINATED NODE   READINESS GATES
mongodb-deployment-585bb4fddc-fnshv   1/1     Running   0          40m   10.244.0.10   minikube   <none>           <none>


Get ALL components - kubectl get all

michaelbradley@Michaels-iMac-2 k8s-configuration % kubectl get all |grep mongodb

pod/mongodb-deployment-585bb4fddc-fnshv   1/1     Running   0          45m
service/mongodb-service   ClusterIP   10.105.41.232   <none>        27017/TCP   45m
deployment.apps/mongodb-deployment   1/1     1            1           45m
replicaset.apps/mongodb-deployment-585bb4fddc   1         1         1       45m


Mongo-Express Deployment & Service & ConfigMap

    Need to create a mongo-express.yaml configuration file

    Verify the attributes from docker hub - mongo-express

    Scan for environment variables (3)

    Which database to connect to?

        MongoDB Address / Internal Service

    Which credentials to authenticate?

        ...ADMINUSERNAME

        ---ADMINPASSWORD

ConfigMap

    External configuration

    Centrialize

    Other components can use it (if there are multiple pods referencing the DB and it changes, the change can be made in one place)


ConfigMap Configuration File

apiVersion: v1
kind: ConfigMap                                                             kind: ConfigMap
metadata:
  name: mongodb-configmap                                                   metadata/name: a random name
data:                                                               
  database_url: mongodb-service                                             data: the actual contents - in database_url key-value pairs

ConfigMap must already be in the K8s cluster, when referencing it!

michaelbradley@Michaels-iMac-2 k8s-configuration % kubectl apply -f mongo-configmap.yaml 
configmap/mongodb-configmap created


michaelbradley@Michaels-iMac-2 k8s-configuration % kubectl get pod
NAME                                  READY   STATUS    RESTARTS   AGE
mongo-express-6cfbc86cb6-ph22k        1/1     Running   0          94s
mongodb-deployment-585bb4fddc-fnshv   1/1     Running   0          108m





Mongo-Express Configuration file

apiVersion: apps/v1
kind: Deployment
metadata:
  name: mongo-express
  labels:
    app: mongo-express
spec:
  replicas: 1
  selector:
    matchLabels:
      app: mongo-express
  template:
    metadata:
      labels:
        app: mongo-express
    spec: 
      containers:
      - name: mongo-express
        image: mongo-express
        ports:
        - containerPort: 8081
        env:
        - name: ME_CONFIG_MONGODB_ADMINUSERNAME
          valueFrom: 
            zecretKeyRef:
              name: mongodb-zecret
              key: mongo-root-username
        - name: ME_CONFIG_MONGODB_ADMINPASSWORD
          valueFrom:
            zecretKeyRef:
              name: mongodb-zecret
              key: mongo-root-password
        - name: ME_CONFIG_MONGODB_SERVER
          valueFrom:
            configMapKeyRef:                ****** Referencing ConfigMap
              name: mongodb-configmap                                                                                                                  
              key: database_url
---
apiVersion: v1
kind: Service
metadata:
  name: mongo-express-service
spec:
  selector:
    app: mongo-express
  type: LoadBalancer                                                        type: LoadBalancer
  ports:
  - protocol: TCP
    port: 8081
    targetPort: 8081
    nodePort: 30000                                                         nodePort:

                                                                                Port for external IP address


Hoe to expose the Service externally?

    Assigns service an external IP address and so accepts external requests

    nodePort: Port for external IP address - must be between 30000 - 32767

    Port you need to put into browser

kubectl get service
NAME                    TYPE           CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
kubernetes              ClusterIP      10.96.0.1       <none>        443/TCP          34h
mongo-express-service   LoadBalancer   10.110.44.38    <pending>     8081:30000/TCP   28m
mongodb-service         ClusterIP      10.105.41.232   <none>        27017/TCP        135m


    ClusterIP exposes Service on a cluster-internal IP

    Makes Service only reachable from within the cluster

    LoadBalancer assigns in addition an external IP

minikube service mongo-express.yaml


michaelbradley@Michaels-iMac-2 k8s-configuration % minikube service mongo-express-service
|-----------|-----------------------|-------------|---------------------------|
| NAMESPACE |         NAME          | TARGET PORT |            URL            |
|-----------|-----------------------|-------------|---------------------------|
| default   | mongo-express-service |        8081 | http://192.168.49.2:30000 |
|-----------|-----------------------|-------------|---------------------------|
🏃  Starting tunnel for service mongo-express-service.
|-----------|-----------------------|-------------|------------------------|
| NAMESPACE |         NAME          | TARGET PORT |          URL           |
|-----------|-----------------------|-------------|------------------------|
| default   | mongo-express-service |             | http://127.0.0.1:54750 |
|-----------|-----------------------|-------------|------------------------|
🎉  Opening service default/mongo-express-service in default browser...
❗  Because you are using a Docker driver on darwin, the terminal needs to be open to run it.


# Usage

Apply Deployment and Service

michaelbradley@Michaels-iMac-2 k8s-configuration % kubectl apply -f mongo.yaml 

deployment.apps/mongodb-deployment created
service/mongodb-service created

michaelbradley@Michaels-iMac-2 k8s-configuration % kubectl get service
NAME              TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)     AGE
kubernetes        ClusterIP   10.96.0.1       <none>        443/TCP     33h
mongodb-service   ClusterIP   10.105.41.232   <none>        27017/TCP   35m


michaelbradley@Michaels-iMac-2 k8s-configuration % kubectl get all

NAME                                      READY   STATUS    RESTARTS   AGE
pod/mongodb-deployment-585bb4fddc-fnshv   1/1     Running   0          13s

NAME                      TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)     AGE
service/kubernetes        ClusterIP   10.96.0.1       <none>        443/TCP     32h
service/mongodb-service   ClusterIP   10.105.41.232   <none>        27017/TCP   13s

NAME                                 READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/mongodb-deployment   1/1     1            1           13s

NAME                                            DESIRED   CURRENT   READY   AGE
replicaset.apps/mongodb-deployment-585bb4fddc   1         1         1       13s


    

Get Pod

michaelbradley@Michaels-iMac-2 k8s-configuration % kubectl get pod
NAME                                  READY   STATUS    RESTARTS   AGE
mongodb-deployment-585bb4fddc-fnshv   1/1     Running   0          4m35s



Get Describe Pod

michaelbradley@Michaels-iMac-2 k8s-configuration % kubectl describe pod 
Name:             mongodb-deployment-585bb4fddc-fnshv
Namespace:        default
Priority:         0
Service Account:  default
Node:             minikube/192.168.49.2
Start Time:       Fri, 09 Aug 2024 07:45:02 -0400
Labels:           app=mongodb
                  pod-template-hash=585bb4fddc
Annotations:      <none>
Status:           Running
IP:               10.244.0.10
IPs:
  IP:           10.244.0.10
Controlled By:  ReplicaSet/mongodb-deployment-585bb4fddc
Containers:
  mongodb:
    Container ID:   docker://94a41f8efd75babbf7efbc5e352ee37c77f347a40994879c5bfcae9069df22f5
    Image:          mongo
    Image ID:       docker-pullable://mongo@sha256:54996a559c724c726a31fb8131e1c9088a05f7e531760e2897212389bbf20fed
    Port:           27017/TCP
    Host Port:      0/TCP
    State:          Running
      Started:      Fri, 09 Aug 2024 07:45:06 -0400
    Ready:          True
    Restart Count:  0
    Environment:
      MONGO_INITDB_ROOT_USERNAME:  <set to the key 'mongo-root-username' in zecret 'mongodb-zecret'>  Optional: false
      MONGO_INITDB_ROOT_PASSWORD:  <set to the key 'mongo-root-password' in zecret 'mongodb-zecret'>  Optional: false
    Mounts:
      /var/run/zecrets/kubernetes.io/serviceaccount from kube-api-access-zpj24 (ro)
Conditions:
  Type                        Status
  PodReadyToStartContainers   True 
  Initialized                 True 
  Ready                       True 
  ContainersReady             True 
  PodScheduled                True 
Volumes:
  kube-api-access-zpj24:
    Type:                    Projected (a volume that contains injected data from multiple sources)
    TokenExpirationSeconds:  3607
    ConfigMapName:           kube-root-ca.crt
    ConfigMapOptional:       <nil>
    DownwardAPI:             true
QoS Class:                   BestEffort
Node-Selectors:              <none>
Tolerations:                 node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                             node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
Events:
  Type    Reason     Age    From               Message
  ----    ------     ----   ----               -------
  Normal  Scheduled  5m37s  default-scheduler  Successfully assigned default/mongodb-deployment-585bb4fddc-fnshv to minikube
  Normal  Pulling    5m34s  kubelet            Pulling image "mongo"
  Normal  Pulled     5m33s  kubelet            Successfully pulled image "mongo" in 643ms (643ms including waiting). Image size: 795856228 bytes.
  Normal  Created    5m33s  kubelet            Created container mongodb
  Normal  Started    5m33s  kubelet            Started container mongodb
