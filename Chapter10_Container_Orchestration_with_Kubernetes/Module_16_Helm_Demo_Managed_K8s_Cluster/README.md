# Chapter10
This module we will demo installing a stateful application on K8s using Helm

# Name: Helm Demo - Managed K8s cluster

# Description: 

Deploying a managed K8s cluster on Linode Kubernetes Engine 

                                HELM


        replica         replica

        pv              pv              ing             ws              browser

        db              db



    Overview

        Deploy mongodb using Helm

        3 replica using StatefulSet

        Configure data persistence with Linode's cloud storage

    Overview of what we will build/deploy

        Deploy UI client mongo-express

        Configure nginx-ingress


    You will need this setup almost always for your K8s cluster

Create K8s cluster on LKE

    login https://cloud.linode.com/linodes

    Create Kubernetes

        test

        region

        version - latest

        add node pools

            Control Plane nodes are managed for you

            Here you create the Worker nodes

        shared CPU

            4 GB

            2 Worker Nodes

            enable HA = no

        create cluster


            Node Pools

 
            lke214516-309725-09d151740000       Running         172.104.210.60

            lke214516-309725-225a9ee30000       Running         104.237.151.211

            Pool ID 309725


apiVersion: v1
kind: Config
preferences: {}

clusters:
- cluster:
    certificate-authority-data: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSURCVENDQWUyZ0F3SUJBZ0lJWGlLUEZQUXNONUF3RFFZSktvWklodmNOQVFFTEJRQXdGVEVUTUJFR0ExVUUKQXhNS2EzVmlaWEp1WlhSbGN6QWVGdzB5TkRBNE1UZ3dNekl4TkRGYUZ3MHpOREE0TVRZd016STJOREZhTUJVeApFekFSQmdOVkJBTVRDbXQxWW1WeWJtVjBaWE13Z2dFaU1BMEdDU3FHU0liM0RRRUJBUVVBQTRJQkR3QXdnZ0VLCkFvSUJBUURFbjJobDlmWWtLczdwaWNVeUxxdEkySm9nOGFmZUpJcllRQ0pSbW1LS0oxV3pHZXE5cG1PS0x2TGUKSmh2N01ES2xxYUV3blVodldZb0oraEpEUTFtOU9HdFZyalVpRFFMK01iMkJGbktaMWJya3drUGhUbi81UWZCMApwQ0VDbHB4aWdhbEplSFlXUDdKZHlzeVJwNE9ZbC9vS3dPOHhiYjVHTWhMWWZOQ2ZudzlCVFRaN1l4S2Q3RDRaCktOc3pVYTl5M1IvcWh3NUs1UThTZzVSU3BwZGRNeUU1b2dmMVZyd0NyaFhUMXZZTE9QNzZyN05uM2VtZFI3NHEKaTV1ZERsbVBXeEhyR3RDTFZhMDBwandYVTI1dWxSdmQ2d1RGWE96Y0FRVTN4RTVGdndPZmNFUXRVQmpPaHMyYQo3RjAwaWZqSG4veDhINXBUWVNWWmxHdEhRcUpaQWdNQkFBR2pXVEJYTUE0R0ExVWREd0VCL3dRRUF3SUNwREFQCkJnTlZIUk1CQWY4RUJUQURBUUgvTUIwR0ExVWREZ1FXQkJRSHltSmptQ0hIakVINjB0c3FXYmM3MHA1YXJqQVYKQmdOVkhSRUVEakFNZ2dwcmRXSmxjbTVsZEdWek1BMEdDU3FHU0liM0RRRUJDd1VBQTRJQkFRREVqQXNBZHBxYQptbjhQYW03TUZHNnVRY1BYdEtIb1g4ajV2WWxYclpISitHR0xvWEtRNFNPd2EwWTNSazRFUjY0WS9vRWJrK3BJCndJUHpKc2VPUXQzcUJWQ3lFZWc3enMyZXpwZzFKbzh2UzFFNjdmL2cxL05XbjhjZmNtTTJxV1VzNmVkK1ZWZGgKcGtDSVJoL0dsbzc1aUdweVBRNHV3dGZ6dEpYYU1GZzNLQXV4RmRYK3N6a0NSSnpVeE9VQ1R4OUtyZTBtZE9aLwpUbnk5UnlGMldVWVBjUkVoQ0s2QnFTODJOQkkvM3VrVysvSlg3amQwRGpzRktyOHhuUkppU0xvYjdXSC80L0ZZCjNNK1ZYc05KYTk2UEJ2bXVzYVdteVpnNzF4L2htY2h4OVIxRWpMRms3dVFkR0JVY2RjZ2JkY0NEbHNXeWhTS2cKdHl4bktNcU5jTkRUCi0tLS0tRU5EIENFUlRJRklDQVRFLS0tLS0K
    server: https://74b75e0c-bea4-4652-8b27-f2788b253271.us-east-2.linodelke.net:443
  name: lke214516

users:
- name: lke214516-admin
  user:
    as-user-extra: {}
    token: eyJhbGciOiJSUzI1NiIsImtpZCI6InVlRkJKZ2xWT0xfTkVSVjlrZ3BndDdpZWFRY2VNMXFTMk40U013SmhwQWsifQ.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9uYW1lc3BhY2UiOiJrdWJlLXN5c3RlbSIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VjcmV0Lm5hbWUiOiJsa2UtYWRtaW4tdG9rZW4tczZnODYiLCJrdWJlcm5ldGVzLmlvL3NlcnZpY2VhY2NvdW50L3NlcnZpY2UtYWNjb3VudC5uYW1lIjoibGtlLWFkbWluIiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9zZXJ2aWNlLWFjY291bnQudWlkIjoiYTMxYzgyZDgtNGM5Ny00NmJjLTk4NGMtYjgyOTE4Y2Q5MWRkIiwic3ViIjoic3lzdGVtOnNlcnZpY2VhY2NvdW50Omt1YmUtc3lzdGVtOmxrZS1hZG1pbiJ9.COE4LcWw0drWVCEqFa5tXNFt6ULtdglyAzVg5qCc4awxnovTWpvMCmwS9LAGsWQvrCJm8gShR9l-Yeh0WkSb64yNDPq3qMYg91ejrO06rRSCGJav_q-T1P24aHvQtQUk62cwwYzS08zGNERhRSECA-NSqoOPgLahG8KgYwcyNZRwD5ir42FS-S6RpYkJ-Cdeij3igzotRF1fuxxXkAjeW33bXX6KfJy3lrTuGNB5_to2mdHe0o9PshavtQGXyM4SzOJNk9ugwyaYIt6kKF0gtbObhhGl0CKFFrJ5G6yy9a1vjvi-8Ghm1JVVWrAEow9weMCJG01i8hM7Y3kEYp7UAg

contexts:
- context:
    cluster: lke214516
    namespace: default
    user: lke214516-admin
  name: lke214516-ctx

current-context: lke214516-ctx

        Download the file - test-kubeconfig.yaml       

        Kubeconfig necessary to access cluster from  your local machine

        set test-kubeconfig.yaml file as an environment variable

Deploy MongoDB StatefulSet

    Two ways to deploy StatefueSet

        1) Create all the configuration files yourself

            StatefulSet Config

            Services

            Other configurations

        2) Use bundle of those configuration files (Helm Chart)

            brew install helm

            https://kubeapps.dev/docs/latest/tutorials/getting-started/

            helm repo add bitnami https://charts.bitnami.com/bitnami

            helm search repo bitinami

            helm search repo bitnami/mongodb - search for chart to install in the cluster using mongodb

            NAME                   	CHART VERSION	APP VERSION	DESCRIPTION                                       
            bitnami/mongodb        	15.6.19      	7.0.12     	MongoDB(R) is a relational open source NoSQL da...
            bitnami/mongodb-sharded	8.3.4        	7.0.12     	MongoDB(R) is an open source NoSQL database tha...


            https://github.com/bitnami/charts/tree/main/bitnami/mongodb - view Mongodb parameters

                Choose replicset for multiple replicas

                add auth.rootPassword

                Helm chart should use the StorageClass of Linode's Cloud Storage

                helm-mongodb.yaml

                architecture: replicaset
                replicaCount: 3
                persistence:
                storageClass: "linode-block-storage"
                auth:
                rootPassword: secret-root-pwd


                    connect to Linode

                    create physical storage

                    attach to your Pods

                    override root password

                helm install [our name] --values [values file name] [chart name]

                helm install mongodb --values helm-mongodb.yaml bitnami/mongodb

                Three replicas

                mongodb-0.mongodb-headless.default.svc.cluster.local:27017
                mongodb-1.mongodb-headless.default.svc.cluster.local:27017
                mongodb-2.mongodb-headless.default.svc.cluster.local:27017


                Three mongdb pods running and another that manages the replicas between those
                kubectl get pod 
                NAME                READY   STATUS    RESTARTS        AGE
                mongodb-0           1/1     Running   0               3m16s
                mongodb-1           1/1     Running   0               2m38s
                mongodb-2           1/1     Running   0               2m4s
                mongodb-arbiter-0   1/1     Running   1 (2m16s ago)   3m17s


    Deploy MongeExpress

        Get password from Secret component

        Internal Service - only accessible within the cluster


        kubectl apply -f helm-mongo-express.yaml

        deployment.apps/mongo-express created
        service/mongo-express-service created

        kubectl get pod

        NAME                            READY   STATUS    RESTARTS     AGE
        mongo-express-b6d76f85c-zdh92   1/1     Running   0            13s
        mongodb-0                       1/1     Running   0            8h
        mongodb-1                       1/1     Running   0            8h
        mongodb-2                       1/1     Running   0            8h
        mongodb-arbiter-0               1/1     Running   1 (8h ago)   8h


        kubectl logs mongo-express-b6d76f85c-zdh92

        Welcome to mongo-express 1.0.2
        ------------------------


        Mongo Express server listening at http://0.0.0.0:8081
        Server is open to allow connections from anyone (0.0.0.0)
        basicAuth credentials are "admin:pass", it is recommended you change this in your config.js!


    Ingress

        1) Deploy Ingress Controller

            Use Helm Chart for Ingress Controller

            nginx-ingress controller

                helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx

                 helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
                "ingress-nginx" has been added to your repositories

            Install helm chart for nginx-ingress

                helm install nginx-ingress ingress-nginx/ingress-nginx --set controller.publishService.enable=true

                helm install nginx-ingress ingress-nginx/ingress-nginx --set controller.publishService.enable=true

                NAME: nginx-ingress
                LAST DEPLOYED: Sun Aug 18 19:44:27 2024
                NAMESPACE: default
                STATUS: deployed
                REVISION: 1
                TEST SUITE: None
                NOTES:
                The ingress-nginx controller has been installed.
                It may take a few minutes for the load balancer IP to be available.
                You can watch the status by running 'kubectl get service --namespace default nginx-ingress-ingress-nginx-controller --output wide --watch'

                kubectl get pod

                NAME                                                      READY   STATUS    RESTARTS     AGE
                mongo-express-b6d76f85c-zdh92                             1/1     Running   0            28m
                mongodb-0                                                 1/1     Running   0            9h
                mongodb-1                                                 1/1     Running   0            9h
                mongodb-2                                                 1/1     Running   0            9h
                mongodb-arbiter-0                                         1/1     Running   1 (9h ago)   9h
                nginx-ingress-ingress-nginx-controller-7bc7c7776d-z4ck7   1/1     Running   0            3m9s

            NodeBalancer = entrypoint to the K8s cluster

                External (public) IP address from Linode's NodeBalancer (Ports 80, 443)

            
            LoadBalancer = accessible externally

            ClusterIP = internal service

                 kubectl get svc

            NAME                                               TYPE           CLUSTER-IP       EXTERNAL-IP     PORT(S)                      AGE
            kubernetes                                         ClusterIP      10.128.0.1       <none>          443/TCP                      20h
            mongo-express-service                              ClusterIP      10.128.48.102    <none>          8081/TCP                     38m
            mongodb-arbiter-headless                           ClusterIP      None             <none>          27017/TCP                    9h
            mongodb-headless                                   ClusterIP      None             <none>          27017/TCP                    9h
            nginx-ingress-ingress-nginx-controller             LoadBalancer   10.128.123.22    45.79.247.135   80:31082/TCP,443:30350/TCP   12m
            nginx-ingress-ingress-nginx-controller-admission   ClusterIP      10.128.244.176   <none>          443/TCP                      12m



        2) Create Ingress Rule

            Use Ingress config file

            Set rules: hosts to Linode DNS for NodeBalancer

            In production this would be your application domain, my-app.com

            The domain must point to IP address of NodeBalancer


        kubectl apply -f helm-ingress.yaml

        ingress.networking.k8s.io/mongo-express configured


        kubectl get ingress

        NAME            CLASS    HOSTS                                    ADDRESS         PORTS   AGE
        mongo-express   <none>   45-79-247-135.ip.linodeusercontent.com   45.79.247.135   80      7m20s

        1) Browser: hostname configured in ingress rule

        2) Hostnane gets resolved to external IP of nodeBalancer

        3) Ingress Controller resolved the rule and forwarded request to internal MongoExpress Service


        kubectl scale --replicas=0 statefulset/mongodb
        statefulset.apps/mongodb scaled

        kubectl get pod

        NAME                                                      READY   STATUS     RESTARTS      AGE
        mongo-express-b6d76f85c-kq7vd                             1/1     Running    0             102m
        mongodb-0                                                 1/1     Running    0             52s
        mongodb-1                                                 1/1     Running    0             25s
        mongodb-2                                                 0/1     Init:0/1   0             2s
        mongodb-arbiter-0                                         1/1     Running    1 (86m ago)   87m
        nginx-ingress-ingress-nginx-controller-7bc7c7776d-z4ck7   1/1     Running    0             173m

        Volumes are re-attached to Pods

Uninstall

        helm uninstall mongodb
        release "mongodb" uninstalled

        pvcdcecd9d077ab49e0
        active
        Newark, NJ	10 GB	
        Unattached

        pvc691fd1681a944b6a
        active
        Newark, NJ	10 GB	
        Unattached

        pvc68b5b052265b4944
        active
        Newark, NJ	10 GB	
        Unattached




    Configuration

        mongodb     mongodb     mongodb             UI

        pv          pv          pv                  mongo-express               ingress             browser
        
                Linode Cloud Storage

    

# Usage

export KUBECONFIG=test-kubeconfig.yaml

kubectl get node 
NAME                            STATUS   ROLES    AGE   VERSION
lke214516-309725-09d151740000   Ready    <none>   10h   v1.30.3
lke214516-309725-225a9ee30000   Ready    <none>   10h   v1.30.3


% helm repo add bitnami https://charts.bitnami.com/bitnami
"bitnami" has been added to your repositories

helm search repo bitnami
NAME                                        	CHART VERSION	APP VERSION  	DESCRIPTION                                       
bitnami/airflow                             	19.0.1       	2.10.0       	Apache Airflow is a tool to express and execute...
bitnami/apache                              	11.2.14      	2.4.62       	Apache HTTP Server is an open-source HTTP serve...
bitnami/apisix                              	3.3.9        	3.9.1        	Apache APISIX is high-performance, real-time AP...
bitnami/appsmith                            	4.0.0        	1.34.0       	Appsmith is an open source platform for buildin...
bitnami/argo-cd                             	7.0.1        	2.12.1       	Argo CD is a continuous delivery tool for Kuber...
bitnami/argo-workflows                      	9.1.13       	3.5.10       	Argo Workflows is meant to orchestrate Kubernet...
bitnami/aspnet-core                         	6.2.11       	8.0.8        	ASP.NET Core is an open-source framework for we...
bitnami/cassandra                           	11.3.12      	4.1.5        	Apache Cassandra is an open source distributed ...
bitnami/cert-manager                        	1.3.17       	1.15.3       	cert-manager is a Kubernetes add-on to automate...
bitnami/chainloop                           	0.1.6        	0.95.7       	Chainloop is an open-source Software Supply Cha...
bitnami/cilium                              	1.0.17       	1.16.1       	Cilium is an eBPF-based networking, observabili...
bitnami/clickhouse                          	6.2.17       	24.7.3       	ClickHouse is an open-source column-oriented OL...
bitnami/common                              	2.22.0       	2.22.0       	A Library Helm Chart for grouping common logic ...
bitnami/concourse                           	4.2.11       	7.11.2       	Concourse is an automation system written in Go...
bitnami/consul                              	11.3.12      	1.19.1       	HashiCorp Consul is a tool for discovering and ...
bitnami/contour       
 ... 


 helm install mongodb --values helm-mongodb.yaml bitnami/mongodb
NAME: mongodb
LAST DEPLOYED: Sun Aug 18 10:45:36 2024
NAMESPACE: default
STATUS: deployed
REVISION: 1
TEST SUITE: None
NOTES:
CHART NAME: mongodb
CHART VERSION: 15.6.19
APP VERSION: 7.0.12

** Please be patient while the chart is being deployed **

MongoDB&reg; can be accessed on the following DNS name(s) and ports from within your cluster:

    mongodb-0.mongodb-headless.default.svc.cluster.local:27017
    mongodb-1.mongodb-headless.default.svc.cluster.local:27017
    mongodb-2.mongodb-headless.default.svc.cluster.local:27017

To get the root password run:

    export MONGODB_ROOT_PASSWORD=$(kubectl get secret --namespace default mongodb -o jsonpath="{.data.mongodb-root-password}" | base64 -d)

To connect to your database, create a MongoDB&reg; client container:

    kubectl run --namespace default mongodb-client --rm --tty -i --restart='Never' --env="MONGODB_ROOT_PASSWORD=$MONGODB_ROOT_PASSWORD" --image docker.io/bitnami/mongodb:7.0.12-debian-12-r6 --command -- bash

Then, run the following command:
    mongosh admin --host "mongodb-0.mongodb-headless.default.svc.cluster.local:27017,mongodb-1.mongodb-headless.default.svc.cluster.local:27017,mongodb-2.mongodb-headless.default.svc.cluster.local:27017" --authenticationDatabase admin -u $MONGODB_ROOT_USER -p $MONGODB_ROOT_PASSWORD

WARNING: There are "resources" sections in the chart not set. Using "resourcesPreset" is not recommended for production. For production installations, please set the following values according to your workload needs:
  - arbiter.resources
  - resources
+info https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/   



kubectl get all
NAME                    READY   STATUS    RESTARTS        AGE
pod/mongodb-0           1/1     Running   0               5m39s
pod/mongodb-1           1/1     Running   0               5m1s
pod/mongodb-2           1/1     Running   0               4m27s
pod/mongodb-arbiter-0   1/1     Running   1 (4m39s ago)   5m40s

NAME                               TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)     AGE
service/kubernetes                 ClusterIP   10.128.0.1   <none>        443/TCP     11h
service/mongodb-arbiter-headless   ClusterIP   None         <none>        27017/TCP   5m40s
service/mongodb-headless           ClusterIP   None         <none>        27017/TCP   5m40s

NAME                               READY   AGE
statefulset.apps/mongodb           3/3     5m41s
statefulset.apps/mongodb-arbiter   1/1     5m41s


 kubectl get secret
NAME                            TYPE                 DATA   AGE
mongodb                         Opaque               2      8m1s
sh.helm.release.v1.mongodb.v1   helm.sh/release.v1   1      8m1s


kubectl get secret mongodb -o yaml
apiVersion: v1
data:
  mongodb-replica-set-key: YmxOVlI2RmRPZA==
  mongodb-root-password: c2VjcmV0LXJvb3QtcHdk
kind: Secret
metadata:
  annotations:
    meta.helm.sh/release-name: mongodb
    meta.helm.sh/release-namespace: default
  creationTimestamp: "2024-08-18T14:45:37Z"
  labels:
    app.kubernetes.io/component: mongodb
    app.kubernetes.io/instance: mongodb
    app.kubernetes.io/managed-by: Helm
    app.kubernetes.io/name: mongodb
    app.kubernetes.io/version: 7.0.12
    helm.sh/chart: mongodb-15.6.19
  name: mongodb
  namespace: default
  resourceVersion: "28986"
  uid: db6903ae-ec83-4267-bba5-3f2b0184e799
type: Opaque


kubectl logs mongo-express-b6d76f85c-zdh92

Waiting for mongo:27017...
/docker-entrypoint.sh: line 15: mongo: Name does not resolve
/docker-entrypoint.sh: line 15: /dev/tcp/mongo/27017: Invalid argument
Sun Aug 18 23:18:51 UTC 2024 retrying to connect to mongo:27017 (2/10)
/docker-entrypoint.sh: line 15: mongo: Name does not resolve
/docker-entrypoint.sh: line 15: /dev/tcp/mongo/27017: Invalid argument
Sun Aug 18 23:18:52 UTC 2024 retrying to connect to mongo:27017 (3/10)
/docker-entrypoint.sh: line 15: mongo: Name does not resolve
/docker-entrypoint.sh: line 15: /dev/tcp/mongo/27017: Invalid argument
Sun Aug 18 23:18:53 UTC 2024 retrying to connect to mongo:27017 (4/10)
/docker-entrypoint.sh: line 15: mongo: Name does not resolve
/docker-entrypoint.sh: line 15: /dev/tcp/mongo/27017: Invalid argument
Sun Aug 18 23:18:54 UTC 2024 retrying to connect to mongo:27017 (5/10)
/docker-entrypoint.sh: line 15: mongo: Name does not resolve
/docker-entrypoint.sh: line 15: /dev/tcp/mongo/27017: Invalid argument
Sun Aug 18 23:18:55 UTC 2024 retrying to connect to mongo:27017 (6/10)
/docker-entrypoint.sh: line 15: mongo: Name does not resolve
/docker-entrypoint.sh: line 15: /dev/tcp/mongo/27017: Invalid argument
Sun Aug 18 23:18:56 UTC 2024 retrying to connect to mongo:27017 (7/10)
/docker-entrypoint.sh: line 15: mongo: Name does not resolve
/docker-entrypoint.sh: line 15: /dev/tcp/mongo/27017: Invalid argument
Sun Aug 18 23:18:57 UTC 2024 retrying to connect to mongo:27017 (8/10)
/docker-entrypoint.sh: line 15: mongo: Name does not resolve
/docker-entrypoint.sh: line 15: /dev/tcp/mongo/27017: Invalid argument
Sun Aug 18 23:18:58 UTC 2024 retrying to connect to mongo:27017 (9/10)
/docker-entrypoint.sh: line 15: mongo: Name does not resolve
/docker-entrypoint.sh: line 15: /dev/tcp/mongo/27017: Invalid argument
Sun Aug 18 23:18:59 UTC 2024 retrying to connect to mongo:27017 (10/10)
/docker-entrypoint.sh: line 15: mongo: Name does not resolve
/docker-entrypoint.sh: line 15: /dev/tcp/mongo/27017: Invalid argument
No custom config.js found, loading config.default.js
Welcome to mongo-express 1.0.2
------------------------


Mongo Express server listening at http://0.0.0.0:8081
Server is open to allow connections from anyone (0.0.0.0)
basicAuth credentials are "admin:pass", it is recommended you change this in your config.js!
michaelbradley@Michaels-iMac-2.attlocal.net /Users/michaelbradley/k8s-configuration/helm


helm install nginx-ingress ingress-nginx/ingress-nginx --set controller.publishService.enable=true

NAME: nginx-ingress
LAST DEPLOYED: Sun Aug 18 19:44:27 2024
NAMESPACE: default
STATUS: deployed
REVISION: 1
TEST SUITE: None
NOTES:
The ingress-nginx controller has been installed.
It may take a few minutes for the load balancer IP to be available.
You can watch the status by running 'kubectl get service --namespace default nginx-ingress-ingress-nginx-controller --output wide --watch'

An example Ingress that makes use of the controller:
  apiVersion: networking.k8s.io/v1
  kind: Ingress
  metadata:
    name: example
    namespace: foo
  spec:
    ingressClassName: nginx
    rules:
      - host: www.example.com
        http:
          paths:
            - pathType: Prefix
              backend:
                service:
                  name: exampleService
                  port:
                    number: 80
              path: /
    # This section is only required if TLS is to be enabled for the Ingress
    tls:
      - hosts:
        - www.example.com
        secretName: example-tls

If TLS is enabled for the Ingress, a Secret containing the certificate and key must also be provided:

  apiVersion: v1
  kind: Secret
  metadata:
    name: example-tls
    namespace: foo
  data:
    tls.crt: <base64 encoded cert>
    tls.key: <base64 encoded key>
  type: kubernetes.io/tls