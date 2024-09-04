# Chapter10
This module we will review Production and Security Best Practices

# Name: Production & Security Best Practices

# Description: 

Production & Security Best Practices

    Created K8s Config Files with bad practices

    Microservices Appplication is running

    Missing best practices

We will improve our configuration files with Production & Security Best Practices

Best Practices

    1) Pinned (Tag) Version for each Container Image

        Specify a pinned version on each container image

        spec:
          containers:
            - name: app
              image: nginx:1.25.2

        When a specific tag is not specified K8s automatically pulls the latest version of that image

        Why it's not recommended?

            Makes it unpredictable

            Might break something

            Which verions are currently deployed in the cluster is not transparent

            Fixate container image for each microservice

            The DevOps Engineer should ask which image version should be deployed

            We will know when handling the CI/CD Pipeline

            Same Image Version for all microservices

    2) Configure a liveness probe on each container

        spec:
          containters:
          - name: app
            image: nginx:1.25.2
            livenessProbe:
              grpc:
                port: 8080

        spec:
          containters:
          - name: app
            image: nginx:1.25.2
            livenessProbe:
              tcpSocket:
                port: 8080


        spec:
          containters:
          - name: app
            image: nginx:1.25.2
            livenessProbe:
              httpGet:
                path: /health
                port: 8080


        Why it's important

        K8s manages its resources intelligently

        Restarts Pods when it crashes

        What about when the application inside is not in a healthy state

        How do we let K8s know what state the application inside the Pod

        So K8s automatically restarts the Application


        Perform Health Checks with Liveness Probe

            Checks if application is healthy and automatically restart the Pod

            We can add this simple script ourselves

            This is an attribute of the containers configuration

            It is called livenessProbe and is based on protocol attribute to return the health check

                HTTP
                TCP
                GRCP

            Must define the port, which runs on 8080

            Must define frequency it monitors the health

                periodSeconds: 5

    
    3) Readiness Probe for each container

        spec:
          containters:
          - name: app
            image: nginx:1.25.2
            readinessProbe:
              grpc:
                port: 8080

        spec:
          containters:
          - name: app
            image: nginx:1.25.2
            readinessProbe:
              tcpSocket:
                port: 8080


        spec:
          containters:
          - name: app
            image: nginx:1.25.2
            readinessProbe:
              httpGet:
                path: /health
                port: 8080

        Why this is important?

        K8s know the state of the Pod

        Liveness Probe for Application State

        Health checks only after container is started

        What about the starting up process?

        Pod was started sucessfully

        Application is starting

        How does K8s know the Application is fully started and available for service



        Readiness Probe Performs Health Checks

        Lets K8s know if application is ready to receive traffic

        If application needs to minutes to start, without readiness probe K8s assumes the app is ready to receive traffic as soon as the container starts

        How do we configure a readiness Probe?

        Readiness Probe checks the health during application startup

        Liveness checks while application is running

            2 TCP probes

            a) grpc

            b) tcpSocket

            Kubelet makes probe connection at the node, not the pod

            intialDelaySeconds: 5   can be used as alternative in case application takes longer than usual to start up time

            c) httpGet - used to check if endpoint is healthy

                path: "/_healthyz"
                port: 8080

    4) Resource Requests for each container

        spec:
          containters:
          - name: app
            image: nginx:1.25.2
            resources:
              requests:
                memory: "64Mi"

        spec:
          containters:
          - name: app
            image: nginx:1.25.2
            resources:
              requests:
                cpu: "250m"

        2 Types of resources: CPU and Memory

        Requests is what a container is guaranteed to get

        K8s Scheduler uses this to determine where to run the Pods

            CPU resources are defined in millicores

            Memory resources are defined in bytes


    5) Resource Limits for each container

            Why resource limits are important?

            What if appliation needs more resources?

                Too much data to load into memory

                Bug in application or infinite loop?

            Container will consume more than the requested resources

            If not limited, container could consume all the Node's resources


            Container resource limit for each container

                Make sure container never goes above a certain value

                Container is not allowed to go up to the limit

        spec:
          containters:
          - name: app
            image: nginx:1.25.2
            resources:
              requests:
                cpu: 100m
                memory: 64Mi
              limits:
                cpu: 200m
                memory: 128Mi 


            This would allow you to horizontally scale containers running on the Node, based on its capacity

            If you put values larger than your biggest node, your Pod will never be scheduled!

            Some microservice may require more resources than others

            Redis will need more Memory and less CPU, because it is a memory database



    6) Don't expose a NodePort


            Why is it a bad practice?

                This is a security risk.

                NodePort Service opens port on each Worker Node where it can accessed by external sources.

            
            The best practice is to only use internal services to the clusterIP (1 entrypoint)

                Outside the cluster on a separate server

            
        We can use a LoadBalancer Type

        apiVersion: v1
        kind: LoadBalancer
        metadata:
          name: frontend
        spec:
          type: LoadBalancer
          selector:
            app: frontend
          ports:
          - protocol: TCP
            port: 80
            targetPort: 8080
            nodePort: 30007


                browser
                   |
                   |
                   V
                LoadBalancer                    Load Balancer will receive all the traffic
                |           |
                V           V                   Cloud Platform's LoadBalancer
        Internal Service    Internal Service

        Node                Node



        Use Ingress as an alternative

                        Outside                                         Outside

                        Ingress                                         Ingress

                        ClusterIP Service                               ClusterIP Service                               
                        

                Pod                         Pod                 Pod                             Pod


        apiVersion: v1
        kind: LoadBalancer
        metadata:
          name: frontend
        spec:
          type: LoadBalancer    <-------- We will change the type to LoadBalancer to provide better security
          selector:
            app: frontend
          ports:
          - protocol: TCP
            port: 80
            targetPort: 8080
            nodePort: 30007     <-------- Remove nodePort


    7) More than one replica for Deployment

        When we do not configure the replicas it defaults to 1

        kind: Deployment
        spec:
          replicas: 2

        % kubectl get pod -n microservices
NAME                                     READY   STATUS    RESTARTS   AGE
adservice-7f5dc4b75f-5jc4n               1/1     Running   0          21s
cartservice-68f8747f94-hzq69             1/1     Running   0          20s
checkoutservice-86bdcfbbfc-2xwf2         1/1     Running   0          19s
currencyservice-6f7fd6989-vj776          1/1     Running   0          22s
emailservice-6df48986c8-s2hqx            0/1     Running   0          24s
frontend-55cf69d447-gddk4                1/1     Running   0          18s
paymentservice-c5776df6f-njkzl           1/1     Running   0          23s
productcatalogservice-8d4f48b75-smp8p    1/1     Running   0          23s
recommendationservice-69459db9c9-vlltj   1/1     Running   0          24s
redis-cart-8c5bbbccf-gnwb4               1/1     Running   0          20s
shippingservice-5cc877bc4c-htc4n         1/1     Running   0          22s

        1/1 replicas

        If 1 Pod crashes your application is not accessible until new Pod restarts

        With more replicas the application is always available. No downtime for users

        Always deploy more than 1 replica of each application/microservice


    8) Use More than 1 Worker Node in your cluster

            Single Point of Failure with just 1 Node

            If something happens to that Node all Pods are lost

            You want to replicate your Nodes as well

            Typically you need each replica to run on a different Node, so you should configure 3 replicas

        Reasons for Server Unavailability

            Server crashes

            Server reboots because of an update

            Server maintenance

            Server broken

    9) Using Labels

        Use labels for all resources

        Labels are Key Value Pairs

        Attach to K8 resources

        Why is the important?

            Custom indentifier for your components

            Should be meaningful and relevant to users

        Technical Labels

        apiVersion: v1
        kind: Deployment
        metadata:
          labels:
            application: my-app
            version: "v31"
            release: "r42"
            stage: production

        Business Labels

        apiVersion: v1
        kind: Deployment
        metadata:
          name: Deployment
          labels:
            owner: payment-team
            project: fraud-detection
            business-unit: "80432"
      
        With these labels they can be referenced

        1. Group Pods with labels
        apiVersion: apps/v1
        kind: Deployment
        metadata:
          name: emailservice
        spec:
          selector:
            matchLabels:
              app: emailservice
          template:
            metadata:
                labels: 
                app: emailservice
            spec:


        2. Referenced in Service Component

        apiVersion: v1
        kind: Service
        metadata:
          name: emailservice
        spec:
          type:
          selector:
            app: emailservice
          ports:
          - protocol: TCP
            port: 5000
            targetPort: 8080
       

    10) Using Namespaces

        Use Namespaces to isolate resources

        Why is this important?

            Organize K8s resources in Namespace

            Make it easier to manage and is considerably more organized

            As opposed to having everything in 1 Namespace

        Define Access Rights based on Namespaces

            Teams can be on the same cluster but in a separate Namespace and therefore security managed separately.

        Namespace Permissions

            Role

            RoleBinding

        Created own namespace microservcie-onlineshop, instead of default one

            One Namespace for whole Microservice - microservice-onlineshop

            One Namespace per microservice - emailservice, cartservice, paymentservice


        3 Security Best Practices

            1) Ensure Images are free of vulnerabilities

                Happens with Third Party Libraries with vulnerabilities - Public Registries like DockerHub

                Base images with vulnerabilities

                This can be avoided by:

                    Manual Vulnerability Scans

                    Automated Scans in Build Pipeline - CI/CD

            2) No Root Access for containers

                spec:
                  containers:
                    - name: app
                      image: nginx: 1.25.2
                      securityContext:
                        priviledged: true

                Why is this important?

                With root access you have access to host-level resources

                If container is hacked, much more damage can be done!

                Configure Containers to use unprivileged users will minimize these security risks.

                Usually official images do not use root user

                Good practice to check whether container is running as root


            3) Update Kubernetes to the latest version

                Why is this important?

                Important Security Fixes

                General Business Fixes 

                Will need to update K8s Version Node by Node to avoid application downtime

                Which is another reason to have multiple nodes

                And Pod replicas on different Nodes
































        










# Usage


    