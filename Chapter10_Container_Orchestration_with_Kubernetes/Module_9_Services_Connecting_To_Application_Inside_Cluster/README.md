# Chapter10
This module we will explore Services Connecting to Application inside Cluster

# Name: Services Connecting to Application inside Cluster

# Description: 


Kubernetes Services

    What is a Kubernetes Service and when we need it?

    Different Service types explainee

        ClusterIP Service

        Headless Service

        NodePort Service

        LoadBalancer Services

    Differences between them and then to use which one

    Each Pod gets its own IP address

        Pods are ephemeral  - are destroyed frequently

        When a Pod fails and another gets started in its place, it's assigned a new IP

    With a Service

        You have a stable IP aaddress that stays even when the Pod dies

        This represents a stable persistent IP address

        Also provides load balancing

        Good abstraction for loose coupling

        Within & outside cluster


        ClusterIP Services

            Default type

            When you don't specify you will be assigned this type



        Microservice app deployed

            Pod with microservice container on port 3000    

            Side-car container that collects microservice logs on port 9000

            Pod gets an IP address from Node's range

                Node 10.2.2.25

            If you have three Nodes as part of your cluster, each Node will get a range of IP addresses

                Node 1 - 10.2.1.x

                Node 2 - 10.2.2.x

                Node 3 - 10.2.3 x

                If you want to see a list of worker nodes IP addresses

                    kubectl get pods -o wide

                
            If you have replicas set to 2

                Node 2 - 10.2.2.5
                                                        Internal Service
                    containerPort: 3000
                                                        ClusterIP
                    containerPort: 9000                
                                                        Service <-----------  Ingress <---------- Browser
                Node 1 - 10.2.1.4                       10.128.8.64:3200

                    containerPort: 3000

                    containerPort: 9000

                apiVersion: networking.k8s.io/v1beta
                kind: Ingress
                metadata:
                  name: ms-one-ingress
                  annotations:
                    kubernetes.io/ingress.class: "nginx"
                spec:
                  rules:    
                    - hosts: microservice-one.com
                      http:
                        paths:
                          - path: /
                            backend:
                              serviceName: microservice-one-service
                              servicePort: 3200


            A Service is not a Pod. It's an abstraction layer that represents an IP address

        Ingress

            Defines Ingress rules that forward the request based on request address to certain services

            The service is defined by its name



            DNS resolution then maps that name to an IP address that the Service was assigned

        Service

            Once the Service gets the request it is forwarded to one those pods registered as service endpoints 

            How does the Service know which Pods to forward the request to, or what port to forward to?

            This is defined by selector. Pods are identified via selectors

            We specify the selector attribute that has a key-value pair as a list

            labels of Pods, to match that selector

            random label names


            apiVersion: v1
                kind: Service
                metadata:
                  name: microservice-one-service
                  labels:
                    app: microservice-one
                spec:
                  selector:
                    app: microservice-one



        Pods

            In the Pod configuration file (Deployment) 

            Assign the Pod certain labels in the metadata section

            In the Service yaml file we define a selector to match any Pod that has all of these labels


            apiVersion: apps/v1
                kind: Deployment
                metadata:
                  name: microservice-one
                  labels: 
                spec:
                  replicas: 2
                  selector: 
                  template:
                    metadata:
                      labels:
                        app: microservice-one   


            If you have a Deployment that creates 3 replicas of Pods 

                                    selector:
                                       app: my-app
                      Service          type: microservice 


                      labels:
                        app: my-app
                        type: microservice

                Pod             Pod          Pod
                10.2.1.4        10.2.2.5     10.2.2.6
                Node 1          Node 2       Node 2


                Service matches 3 replicas

                Register all three Pods as its endpoints

                Must match ALL the selectors

                This is how the Service knows what Pods belong to it



            Pods with multiple ports

            Which port to forward request to?

                This is defined in the targetPort attribute

            apiVersion: v1
                kind: Service
                metadata:
                  name: microservice-one-service
                  labels:
                    app: microservice-one
                spec:
                  selector:
                    app: microservice-one
                  ports:
                    - protocol: TCP
                      port: 3200
                      targetPort: 3000

            When we create the Service
             
                1) It will find all the Pods that match the Selector. These Pods will become endpoints of the Service. The Service is a LoadBalancer and will select a Pod.

                2) The Service will send the request to the targetPort

                Service
                10.128.8.64
            
            selector:
              app: microservice-one

                Pod
                10.2.2.5
                Node 2

            labels:
              app: microservice-one

                Pod
                10.2.1.4
                Node1

            labels:
              app: microservice-one


        Service Endpoints

            K8s creates Endpoint object

            kubectl get endpoints

            Has same name as Service

            Keeps track of which Pods are the members/endpoints of the Service

        Service Communication: port vs targetPort

            Service port is arbitrary

            targetPort must match the port the container is listening at (3000)


        Service Communication: Example

            Let's suppose the microservice receives a request, thru the browser, ingress and internal clusterIP

            Now it needs to communicate with the database, to handle that request

            Assume the microservice communicates with a mongodb

            We have two replicas of the mongodb in the cluster, which also have their own endpoints

            Mongodb is also a clusterIP, and has its own IP address

            The microservice IP can now talk to the Mongodb service endpoint


            Service
            10.128.19.160:27017
            ClusterIP

            Pod
            10.2.3.3:27017
            Node 3

            Pod
            10.2.1.5:27017
            Node will communicate with one of the Pods that gets the request from the Service and sends the request to the Service of the Mongodb


            apiVersion: v1
                kind: Service
                metadata:
                  name: microservice-one-service
                spec:
                  selector:
                    app: mongodb
                  ports:
                    - protocol: TCP
                      port: 27017
                      targetPort: 27017


            apiVersion: apps/v1
                kind: Deployment
                metadata:
                  name: mongodb
                  labels: 
                spec:
                  replicas: 1
                  selector: 
                  template:
                    metadata:
                      labels:
                        app: mongodb  


            The Service of the Mongodb forwards the request to the Pod with the targetPort: 27017 


        Multi-port Services


                Service (ClusterIP)
                10.128.19.16 0
                *:27017
                *9216

            Second container running for monitoring metrics

                Pod
                10.2.1.5
                mongo-db application
                mongo-db exporter:9216
                Node 1

                Pod
                10.2.3.3
                mongo-db application
                mongo-db exporter:9216
                Node 3
                


            In the cluster we have the Prometheus application that scrapes the metrics endpoint from this mongodb-exporter container, from the *:9216 endpoint.

            Thus, Service has to handle two different endpoint requests, which means Service has two of its own ports open for handling these requests 

            This is the example of multi-port Service

            When you have multiple ports you have to name those ports

                apiVersion: v1
                kind: Service
                metadata:
                  name: microservice-one-service
                spec:
                  selector:
                    app: mongodb
                  ports:
                    - name: mongodb
                      protocol: TCP
                      port: 27017
                      targetPort: 27017
                    - name: mongodb-exporter
                      protocol: TCP
                      port: 9216
                      targetPort: 9216

            This is an example of ClusterIP Service Type


        Headless Services

            Each request to the Service is forwarded to the replicas that are registered as Service endpoints

            Imagine if Client wants to communicate with one of specific Pod directly

            Pods need to communicate directly with a specific Pod, without going thru a Service

            Use case:

                Stateful applications like databases (mysql, mongodb, elasticsearch)

                In such cases the Pods are not identical

                Only main instance is allowed to write to DB

                The replica must connect to the main to syncronize data

                When a second replica is created it must connect to the existing replica Pod to clone the data from it and get up to date with data state.

            Client needs to figure out IP addresses of each Pod

                Option 1 - API call to K8s API Server

                    Makes app to tied to K8s API

                    Inefficient

                Option 2 - DNS Lookup

                    DNS Lookup for Service - returns single IP address of the Service (ClusterIP)

                    Set ClusterIP to "None" - returns Pod IP address instead



                Main                                        Replica

                mysql-0                                     mysql-1
            writing    reading                                      reading

                database            data replica            database


                apiVersion: v1
                kind: Service
                metadata:
                  name: mongodb-service-headless
                spec:
                  selector:
                    clusterIP: None
                    app: mongodb
                  ports:
                    - name: mongodb
                      protocol: TCP
                      port: 27017
                      targetPort: 27017

            No cluster IP address is assigned!

                kubectl get service


                Service (ClusterIP)
                10.128.19.16 0
                *:27017
                *9216

                Headless
                Service

            Second container running for monitoring metrics

                Pod
                10.2.1.5
                mongo-db application
                mongo-db exporter:9216
                Node 1

                Pod
                10.2.3.3
                mongo-db application
                mongo-db export

            For use cases where Client nneds to communicate with Pods directly or Pods to communicate directly with each other usig the Headless Service



        NodePort Service

            Three Service type attributes

                ClusterIP

                apiVersion: v1
                kind: Service
                metadata:
                  name: my-service
                spec:
                  type: ClusterIP

                Default type not needed!

                Internal service


                NodePort

                apiVersion: v1
                kind: Service
                metadata:
                  name: ms-service-nodeport
                spec:
                  type: NodePort
                  selector:
                    app: microservice-one
                  ports:
                    - protocol: TCP
                      port: 3200
                      targetPort: 3000
                      nodePort:30008


                Creates a service that is accessible on a static port on each worker node in the cluster

                In the previous example the ClusterIP is only accessible within the cluster

                NodePort makes external traffic accessible to fixed port on each Worker Node

                Instead of Ingress, the browser request will come directly to the Worker Node port that the Service specification defines

                The nodePort has a predefine range from 30000 - 32767

                The nodePort service is accessible for the external traffic like browser requests, ip-address of the Worker Node, and the nodePort defined here.

                When we create the nodePort the ClusterIP port from which the NodePort Service will route is automatically created

                    ms-service-nodeport         NodePort 10.128.202.9       <none>     3200:30008/TCP

                                                                cluster-ip:3200     node-ip:30008

                NodePort Service spans all Worker Nodes, so it can handle all request and forward it to one of those Pod replicas

                NodePort is not secure, the clients have direct access to the Worker Nodes





                LoadBalancer

                Becomes accessible externally through cloud providers LoadBalancer

                    Google

                    AWS

                    Azure

                    Linode

                    Open Stack

                Whenever we create a LoadBalancer Service NodePort and ClusterIP Service are created automatically, for which the cloud provider will route the traffic to

                The entrypont becomes the LoadBalancer first and then direct the traffic to NodePort on the Worker Node and the ClusterIP on the internal service

                The LoadBalancer service is an extension of the NodePort Service

                NodePort service is an extension of ClusterIP Service



apiVersion: v1
                kind: Service
                metadata:
                  name: ms-service-loadbalancer
                spec:
                  type: LoadBalancer
                  selector:
                    app: microservice-one
                  ports:
                    - protocol: TCP
                      port: 3200
                      targetPort: 3000
                      nodePort:30010

Wrap up

        NodePort Service NOT for external connection or Production

        Configure Ingress or LoadBalancer for Production environments

        Ingress will route to ClusterIP internal Service

        Or you would use LoadBalancer that uses Cloud Providers


                  




















# Usage


    