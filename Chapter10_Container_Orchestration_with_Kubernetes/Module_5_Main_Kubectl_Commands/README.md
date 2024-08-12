# Chapter10
This module we will look at the Kubernetes CLI 
# Name: Main Kubectl Commands

# Description:

Basic kubectl commands

    Create and debug Pods in a minikube cluster

    CRUD commands

    Create deployment       kubectl create deployment [name]

    Edit deployment         kubectl edit deployment [name]

    Delete deployment       kubectl delete deployment [name]


Status of different K8s components

    kubectl get nodes | pod | services | replicaset | deployment


Debugging Pods

Log to console              kubectl logs [pod name]


Get status of components

    Create and Edit a Pod

Pre-requisites:

        - minikube installed

        - kubectl installed


    kubectl get nodes

    kubectl get pods

    kubectl get services

    kubectl create -h

    Pod is the smallest unit and you are not working with the pods directly


    Deployment - abstraction over Pods

        This is what we are going to be creating and it will create the Pods underneath

        Usage:
        kubectl create deployment NAME --image=image --[COMMAND] [args...] [options]

        michaelbradley@Michaels-iMac-2 ~ % kubectl create deployment nginx-deply --image=nginx

        deployment.apps/nginx-deply created


        michaelbradley@Michaels-iMac-2 ~ % kubectl get deployment

        NAME          READY   UP-TO-DATE   AVAILABLE   AGE
        nginx-deply   1/1     1            1           95s


        michaelbradley@Michaels-iMac-2 ~ % kubectl get pod
        NAME                           READY   STATUS    RESTARTS   AGE
        nginx-deply-5dcfc7b597-gbdt2   1/1     Running   0          3m8s


    Deploy

    kubectl create deployment nginx-deply --image=nginx

    - Blueprint for creating pods

    - Most basic configuration for deployment (name and image to use)

    - Rest defaults


    kubectl get replicaset

    michaelbradley@Michaels-iMac-2 ~ % kubectl get replicaset
    NAME                     DESIRED   CURRENT   READY   AGE
    nginx-deply-5dcfc7b597   1         1         1       8m37s

    michaelbradley@Michaels-iMac-2 ~ % kubectl get pod
    NAME                           READY   STATUS    RESTARTS   AGE
    nginx-deply-5dcfc7b597-gbdt2   1/1     Running   0          3m8s

    Pod has the same prefix as deployment, but a unique suffix

    Replicaset is managing the replicas of a Pod

        You will not be configuring the replicaset directly. You will be using the deployment and can configure pods and replica count - just append to kubectl create deployment --image=nginx-deply [count]

Layers of Abstraction

    Deployment manages a ..

    ReplicaSet manages a ..

    Pod is an abstraction of ..

    Container and everything below Deployment is handled by Kubernetes

    So an image will have to be edited in the Deployment and not in the Pod

    kubectl edit deployment nginx=deply

        Auto=generated configuration file with default values

        K8s configuration file is explained in the video

        michaelbradley@Michaels-iMac-2 ~ % kubectl edit deployment nginx-deply
        image=nginx:1.25
        deployment.apps/nginx-deply edited

        michaelbradley@Michaels-iMac-2 ~ % kubectl get pod
        NAME                           READY   STATUS    RESTARTS   AGE
        nginx-deply-64cc6c87c5-xr2sc   1/1     Running   0          96s


        michaelbradley@Michaels-iMac-2 ~ % kubectl get replicaset
        NAME                     DESIRED   CURRENT   READY   AGE
        nginx-deply-5dcfc7b597   0         0         0       33m
        nginx-deply-64cc6c87c5   1         1         1       3m4s

        The deployment has no pods in it


        Deployment configuration was updated and everything underneath it


    Debugging Pods

        kubectl logs nginx-deply-64cc6c87c5-xr2sc

        michaelbradley@Michaels-iMac-2 ~ % kubectl create deployment

        mongo-deployment --image=mongo
        deployment.apps/mongo-deployment created
        
        michaelbradley@Michaels-iMac-2 ~ % kubectl get pod
        NAME                                READY   STATUS              RESTARTS   AGE
        mongo-deployment-5dfbb89958-7c947   0/1     ContainerCreating   0          12s
        nginx-deply-64cc6c87c5-xr2sc        1/1     Running             0          8h


        kubectl describe pod mongo-deployment-5dfbb89958-7c947
        Name:             mongo-deployment-5dfbb89958-7c947
        Namespace:        default
        Priority:         0
        Service Account:  default
        Node:             minikube/192.168.49.2
        Start Time:       Thu, 08 Aug 2024 08:59:45 -0400
        Labels:           app=mongo-deployment
                        pod-template-hash=5dfbb89958
        Annotations:      <none>
        Status:           Running
        IP:               10.244.0.5
        IPs:
        IP:           10.244.0.5
        Controlled By:  ReplicaSet/mongo-deployment-5dfbb89958
        Containers:
        mongo:
            Container ID:   docker://53837831d1c9f2fb74aeeaba46a51e36ecdbe3ed5a6ea5e4eb1be3eb27d018fc
            Image:          mongo
            Image ID:       docker-pullable://mongo@sha256:54996a559c724c726a31fb8131e1c9088a05f7e531760e2897212389bbf20fed
            Port:           <none>
            Host Port:      <none>
            State:          Running
            Started:      Thu, 08 Aug 2024 09:00:55 -0400
            Ready:          True
            Restart Count:  0
            Environment:    <none>
            Mounts:
            /var/run/zecrets/kubernetes.io/serviceaccount from kube-api-ac


            michaelbradley@Michaels-iMac-2 ~ % kubectl get pods

            NAME                                READY   STATUS    RESTARTS   AGE
            mongo-deployment-5dfbb89958-7c947   1/1     Running   0          23m
            nginx-deply-64cc6c87c5-xr2sc        1/1     Running   0          9h

            michaelbradley@Michaels-iMac-2 ~ % kubectl exec -it mongo-deployment-5dfbb89958-7c947 -- /bin/bash

            root@mongo-deployment-5dfbb89958-7c947:/# 


    Delete Deployment 

            michaelbradley@Michaels-iMac-2 ~ % kubectl get deployment

            NAME               READY   UP-TO-DATE   AVAILABLE   AGE
            mongo-deployment   1/1     1            1           31m
            nginx-deply        1/1     1            1           9h

            michaelbradley@Michaels-iMac-2 ~ % kubectl delete deployment mongo-deployment

            deployment.apps "mongo-deployment" deleted

            michaelbradley@Michaels-iMac-2 ~ % kubectl get replicaset
            
            nginx-deply-5dcfc7b597   0         0         0       9h
            nginx-deply-64cc6c87c5   1         1         1       9h

            michaelbradley@Michaels-iMac-2 ~ % kubectl delete deployment nginx-deply

            deployment.apps "nginx-deply" deleted

            michaelbradley@Michaels-iMac-2 ~ % kubectl get replicaset
            No resources found in default namespace.



           



    Apply Configuration File

            Normally if you created deployment manually you would need to do the following:

            kubectl create deployment name image option1 option2


            kubectl apply -f [file name] nginx-deployment.yaml

            apiVersion: apps/v1
            kind: Deployment
            metadata:
            name: nginx-deployment
            labels:
                app: nginx
            spec:
            replicas: 1
            selector:
                matchLabels:
                app: nginx
            template:
                metadata:
                labels:
                    app: nginx
                spec:
                containers:
                - name: nginx
                    image: nginx:1.25
                    ports:
                    - containerPort: 80

            michaelbradley@Michaels-iMac-2 ~ % kubectl apply -f nginx-deployment.yaml 

            deployment.apps/nginx-deployment created


            michaelbradley@Michaels-iMac-2 ~ % kubectl get pod

            NAME                                READY   STATUS    RESTARTS   AGE
            nginx-deployment-77778dc6b9-hvtll   1/1     Running   0          39s

            Modify the replica count to 2

            michaelbradley@Michaels-iMac-2 ~ % kubectl apply -f nginx-deployment.yaml

            deployment.apps/nginx-deployment configured

            michaelbradley@Michaels-iMac-2 ~ % kubectl get pod 

            NAME                                READY   STATUS    RESTARTS   AGE
            nginx-deployment-77778dc6b9-hvtll   1/1     Running   0          4m27s
            nginx-deployment-77778dc6b9-pc9rv   1/1     Running   0          69s

            K8s knows when to create or update deployment

            kubectl apply allows you to create and update












# Usage

michaelbradley@Michaels-iMac-2 ~ % kubectl get node
NAME       STATUS   ROLES           AGE   VERSION
minikube   Ready    control-plane   12m   v1.30.0

michaelbradley@Michaels-iMac-2 ~ % kubectl get pods
No resources found in default namespace.

michaelbradley@Michaels-iMac-2 ~ % kubectl get services
NAME         TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   27m
    

michaelbradley@Michaels-iMac-2 ~ % kubectl create -h
Create a resource from a file or from stdin.

 JSON and YAML formats are accepted.

Examples:
  # Create a pod using the data in pod.json
  kubectl create -f ./pod.json
  
  # Create a pod based on the JSON passed into stdin
  cat pod.json | kubectl create -f -
  
  # Edit the data in registry.yaml in JSON then create the resource using the edited data
  kubectl create -f registry.yaml --edit -o json

Available Commands:
  clusterrole           Create a cluster role
  clusterrolebinding    Create a cluster role binding for a particular cluster role
  configmap             Create a config map from a local file, directory or literal value
  cronjob               Create a cron job with the specified name
  deployment            Create a deployment with the specified name
  ingress               Create an ingress with the specified name
  job                   Create a job with the specified name
  namespace             Create a namespace with the specified name
  poddisruptionbudget   Create a pod disruption budget with the specified name
  priorityclass         Create a priority class with the specified name
  quota                 Create a quota with the specified name
  role                  Create a role with single rule
  rolebinding           Create a role binding for a particular role or cluster role
  zecret                Create a zecret using a specified subcommand
  service               Create a service using a specified subcommand
  serviceaccount        Create a service account with the specified name
  token                 Request a service account token

Options:
    --allow-missing-template-keys=true:
	If true, ignore any errors in templates when a field or map key is missing in the template. Only applies to
	golang and jsonpath output formats.

    --dry-run='none':
	Must be "none", "server", or "client". If client strategy, only print the object that would be sent, without
	sending it. If server strategy, submit server-side request without persisting the resource.

    --edit=false:
	Edit the API resource before creating

    --field-manager='kubectl-create':
	Name of the manager used to track field ownership.


kubectl edit deployment nginx=deply

# Please edit the object below. Lines beginning with a '#' will be ignored,
# and an empty file will abort the edit. If an error occurs while saving this file will be
# reopened with the relevant failures.
#
apiVersion: apps/v1
kind: Deployment
metadata:
  annotations:
    deployment.kubernetes.io/revision: "1"
  creationTimestamp: "2024-08-08T03:46:43Z"
  generation: 1
  labels:
    app: nginx-deply
  name: nginx-deply
  namespace: default
  resourceVersion: "2196"
  uid: ad88a3c3-d830-4376-942e-59ea7f950105
spec:
  progressDeadlineSeconds: 600
  replicas: 1
  revisionHistoryLimit: 10
  selector:
    matchLabels:
      app: nginx-deply
  strategy:
    rollingUpdate:
      maxSurge: 25%
      maxUnavailable: 25%
    type: RollingUpdate
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: nginx-deply
    spec:
      containers:
      - image: nginx
        imagePullPolicy: Always
        name: nginx
        resources: {}
        terminationMessagePath: /dev/termination-log
        terminationMessagePolicy: File
      dnsPolicy: ClusterFirst
      restartPolicy: Always
      schedulerName: default-scheduler
      securityContext: {}
      terminationGracePeriodSeconds: 30
status:
  availableReplicas: 1
  conditions:
  - lastTransitionTime: "2024-08-08T03:47:04Z"
    lastUpdateTime: "2024-08-08T03:47:04Z"
    message: Deployment has minimum availability.
    reason: MinimumReplicasAvailable
    status: "True"
    type: Available
  - lastTransitionTime: "2024-08-08T03:46:43Z"


michaelbradley@Michaels-iMac-2 ~ % kubectl logs nginx-deply-64cc6c87c5-xr2sc
/docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
/docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
/docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.conf
10-listen-on-ipv6-by-default.sh: info: Enabled listen on IPv6 in /etc/nginx/conf.d/default.conf
/docker-entrypoint.sh: Sourcing /docker-entrypoint.d/15-local-resolvers.envsh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
/docker-entrypoint.sh: Configuration complete; ready for start up
2024/08/08 04:17:00 [notice] 1#1: using the "epoll" event method
2024/08/08 04:17:00 [notice] 1#1: nginx/1.25.5
2024/08/08 04:17:00 [notice] 1#1: built by gcc 12.2.0 (Debian 12.2.0-14) 
2024/08/08 04:17:00 [notice] 1#1: OS: Linux 6.6.31-linuxkit
2024/08/08 04:17:00 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 1048576:1048576
2024/08/08 04:17:00 [notice] 1#1: start worker processes
2024/08/08 04:17:00 [notice] 1#1: start worker process 29
2024/08/08 04:17:00 [notice] 1#1: start worker process 30
2024/08/08 04:17:00 [notice] 1#1: start worker process 31
2024/08/08 04:17:00 [notice] 1#1: start worker process 32
2024/08/08 04:17:00 [notice] 1#1: start worker process 33
2024/08/08 04:17:00 [notice] 1#1: start worker process 34
2024/08/08 04:17:00 [notice] 1#1: start worker process 35
2024/08/08 04:17:00 [notice] 1#1: start worker process 36
2024/08/08 04:17:00 [notice] 1#1: start worker process 37
2024/08/08 04:17:00 [notice] 1#1: start worker process 38
2024/08/08 04:17:00 [notice] 1#1: start worker process 39
2024/08/08 04:17:00 [notice] 1#1: start worker process 40


michaelbradley@Michaels-iMac-2 ~ % kubectl describe pod mongo-deployment-5dfbb89958-7c947
Name:             mongo-deployment-5dfbb89958-7c947
Namespace:        default
Priority:         0
Service Account:  default
Node:             minikube/192.168.49.2
Start Time:       Thu, 08 Aug 2024 08:59:45 -0400
Labels:           app=mongo-deployment
                  pod-template-hash=5dfbb89958
Annotations:      <none>
Status:           Running
IP:               10.244.0.5
IPs:
  IP:           10.244.0.5
Controlled By:  ReplicaSet/mongo-deployment-5dfbb89958
Containers:
  mongo:
    Container ID:   docker://53837831d1c9f2fb74aeeaba46a51e36ecdbe3ed5a6ea5e4eb1be3eb27d018fc
    Image:          mongo
    Image ID:       docker-pullable://mongo@sha256:54996a559c724c726a31fb8131e1c9088a05f7e531760e2897212389bbf20fed
    Port:           <none>
    Host Port:      <none>
    State:          Running
      Started:      Thu, 08 Aug 2024 09:00:55 -0400
    Ready:          True
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/run/zecrets/kubernetes.io/serviceaccount from kube-api-access-vx4rp (ro)
Conditions:
  Type                        Status
  PodReadyToStartContainers   True 
  Initialized                 True 
  Ready                       True 
  ContainersReady             True 
  PodScheduled                True 
Volumes:
  kube-api-access-vx4rp:
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
  Type    Reason     Age   From               Message
  ----    ------     ----  ----               -------
  Normal  Scheduled  16m   default-scheduler  Successfully assigned default/mongo-deployment-5dfbb89958-7c947 to minikube
  Normal  Pulling    16m   kubelet            Pulling image "mongo"
  Normal  Pulled     15m   kubelet            Successfully pulled image "mongo" in 1m7.213s (1m7.214s including waiting). Image size: 795856228 bytes.
  Normal  Created    15m   kubelet            Created container mongo
  Normal  Started    15m   kubelet            Started container mongo