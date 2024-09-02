# Chapter10
This module we will demo project to deploy microservices applications

# Name: Deploy a Microservices Application

# Description:

Deploy Microservices App into K8s Cluster

    Developer Team will give this task to a DevOps Engineer

    Step by Step Deployment Process

        Key information you need to know

            Don't need to understand the actual code to deploy it

        What microservices are we deploying?

        Service

            frontend
            cartservice
            productcatalogservice
            currencyservice
            paymentservice
            shippingservice
            emailservice
            checkoutservice
            recommendationservice
            adservice
            loadgenerator

        How are they connected?

        Any third party services or databases?

        Need to know what these microservices depend on

        Which Service is accessible from outside the cluster?

        For example, services from the browser would connect to the frontend

        8080                9555                                            
        EmailSvc            AdSvc                   INTERNET                Load Generator

        50051               5050                    8080                    7070
        PaymentSvc          CheckoutSvc             Frontend                CartSvc -----> Cache (Redis)

        50051               7000                    3550                    8080
        ShippingSvc         CurrencySvc             ProductCatalogSvc       RecommendationSvc


        DevOps Engineers will get this information from Developers

        This is an online shop

        Frontend needs CartService and requires Redis db to persist data

        Redis is a Message Broker, in memory database

        This will serve as Storage for shopping cart information

        Load generator is optional, used to test load of application

        We have graph how applications connect to each other

        Visualizing this will give us a good understanding of requirements

        Need to know image name for each microservice

        What environment variables each microservices expects?

        Which ports each Microservice starts

        One team developing all these Microservices

        Deploy Microservice into 1 namespace


 We use a config.yaml template to configure a deployment and a service for each of the microservices   
    
    Create Deployment Config for each Microservice

        ---
apiVersion: apps/v1
kind: Deployment                    Deployment
metadata:
  name: emailservice                The name of the microservice
spec:
  selector:
    matchLabels:
      app: emailservice             The name of the microservice / Label assigns name to the Deployment
  template:
    metadata:
      labels:
        app: emailservice           Name of the Pod
    spec:
      containers:                   The container definition with the image of that microservice                
      - name: service
        image: gcr.io/google-samples/microservices-demo/emailservice:v0.8.0  Fetch Docker image from Google Container Registry
        ports:
        - containerPort: 8080       The port where the microservice starts
        env:
        - name: PORT
          value: "8080"             EmailService expects "PORT" environment variable
        livenessProbe:
          grpc:
            port: 8080
          periodSeconds: 5
        readinessProbe:
          grpc:
            port: 8080
          periodSeconds: 5
        resources:
          requests: 
            cpu: 100m
            memory: 64Mi
          limits:
            cpu: 200m
            memory: 128Mi



    Create Service Config for each Microservice 

        
---
apiVersion: v1
kind: Service                       We have the service for that microservice
metadata:
  name: emailservice                We have the corresponding name
spec:
  type: ClusterIP
  selector:
    app: emailservice               Selector used to group the name of the emailservice Pod with the Deployment and the Service
  ports:
  - protocol: TCP
    port: 5000                      The Service port
    targetPort: 8080                The Target port



    Since we will deploying eleven services for this application, the Deployment and Service definition will need to be create eleven times for each.

            The Service port can be different or the same as the container port




The next application is called the "recommendationservice"

---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: recommendationservice
spec:
  replicas: 2
  selector:
    matchLabels:
      app: recommendationservice
  template:
    metadata:
      labels:
        app: recommendationservice                  The name of the Pod
    spec:
      containers:
      - name: service
        image: gcr.io/google-samples/microservices-demo/recommendationservice:v0.8.0
        ports:
        - containerPort: 8080
        env:
        - name: PORT                                            RecommendationService expects: PORT environment variable
          value: "8080"                                         PRODUCT_CATALOG_SERVICE_ADDR env variable                                        
        - name: PRODUCT_CATALOG_SERVICE_ADDR                    Recommendation Service needs to know the endpoint of ProductCatalogService
          value: "productcatalogservice:3550"                   Endpoint = Service Name + Port  
        - name: DISABLE_PROFILER
          value: "1"
        livenessProbe:
          grpc:
            port: 8080
          periodSeconds: 5
        readinessProbe:
          grpc:
            port: 8080
          periodSeconds: 5

---
apiVersion: v1
kind: Service
metadata:
  name: recommendationservice
spec:
  type: ClusterIP
  selector:
    app: recommendationservice
  ports:
  - protocol: TCP
    port: 8080
    targetPort: 8080




The next application is called the Product Catalog Service

---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: productcatalogservice
spec:
  replicas: 2
  selector:
    matchLabels:
      app: productcatalogservice
  template:
    metadata:
      labels:
        app: productcatalogservice
    spec:
      containers:
      - name: service
        image: gcr.io/google-samples/microservices-demo/productcatalogservice:v0.8.0
        ports:
        - containerPort: 3550
        env:
        - name: PORT                    ProductCatalogService expects "PORT" environment variable
          value: "3550"
        - name: DISABLE_PROFILER
          value: "1"
        livenessProbe:
          grpc:
            port: 3550
          periodSeconds: 5
        readinessProbe:
          grpc:
            port: 3550
          periodSeconds: 5

---
apiVersion: v1
kind: Service
metadata:
  name: productcatalogservice
spec:
  type: ClusterIP
  selector:
    app: productcatalogservice
  ports:
  - protocol: TCP
    port: 3550                          Service Port
    targetPort: 3550


The next microservice is called the paymentservice

---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: paymentservice
spec:
  replicas: 2
  selector:
    matchLabels:
      app: paymentservice
  template:
    metadata:
      labels:
        app: paymentservice
    spec:
      containers:
      - name: service
        image: gcr.io/google-samples/microservices-demo/paymentservice:v0.8.0
        ports:
        - containerPort: 50051
        env:
        - name: PORT                                PaymentService expects "PORT" environment variable
          value: "50051"
        - name: DISABLE_PROFILER
          value: "1"
        livenessProbe:
          grpc:
            port: 50051
          periodSeconds: 5
        readinessProbe:
          grpc:
            port: 50051
          periodSeconds: 5
---
apiVersion: v1
kind: Service
metadata:
  name: paymentservice
spec:
  type: ClusterIP
  selector:
    app: paymentservice
  ports:
  - protocol: TCP
    port: 50051
    targetPort: 50051


The next microservice is the currencyservice

---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: currencyservice
spec:
  replicas: 2
  selector:
    matchLabels:
      app: currencyservice
  template:
    metadata:
      labels:
        app: currencyservice
    spec:
      containers:
      - name: service
        image: gcr.io/google-samples/microservices-demo/currencyservice:v0.8.0
        ports:
        - containerPort: 7000
        env:
        - name: PORT
          value: "7000"                             currencyservice expects "PORT" environment variable
        - name: DISABLE_PROFILER
          value: "1"
        livenessProbe:
          grpc:
            port: 7000
          periodSeconds: 5
        readinessProbe:
          grpc:
            port: 7000                              
          periodSeconds: 5

---
apiVersion: v1
kind: Service
metadata:
  name: currencyservice
spec:
  type: ClusterIP
  selector:
    app: currencyservice
  ports:
  - protocol: TCP
    port: 7000                                  Service Port
    targetPort: 7000                            



The next microservice is called shippingservice

---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: shippingservice
spec:
  replicas: 2
  selector:
    matchLabels:
      app: shippingservice
  template:
    metadata:
      labels:
        app: shippingservice
    spec:
      containers:
      - name: service
        image: gcr.io/google-samples/microservices-demo/shippingservice:v0.8.0
        ports:
        - containerPort: 50051         Container Port matches target Port in Service                 
        env:
        - name: PORT
          value: "50051"
        livenessProbe:
          grpc:
            port: 50051
          periodSeconds: 5
        readinessProbe:
          grpc:
            port: 50051
          periodSeconds: 5
---
apiVersion: v1
kind: Service
metadata:
  name: shippingservice
spec:
  type: ClusterIP
  selector:
    app: shippingservice
  ports:
  - protocol: TCP
    port: 50051
    targetPort: 50051


The next microservice is called adservice

---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: adservice
spec:
  replicas: 2
  selector:
    matchLabels:
      app: adservice
  template:
    metadata:
      labels:
        app: adservice
    spec:
      containers:
      - name: service
        image: gcr.io/google-samples/microservices-demo/shippingservice:v0.8.0
        ports:
        - containerPort: 9555
        env:
        - name: PORT                            Environment Variable with the PORT value
          value: "9555"
        livenessProbe:
          grpc:
            port: 9555
          periodSeconds: 5
        readinessProbe:
          grpc:
            port: 9555
          periodSeconds: 5
        resources:
          requests: 
            cpu: 200m
            memory: 180Mi
          limits:
            cpu: 300m
            memory: 300Mi

---
apiVersion: v1
kind: Service
metadata:
  name: adservice
spec:
  type: ClusterIP
  selector:
    app: adservice
  ports:
  - protocol: TCP
    port: 9555                          This is the service port
    targetPort: 9555                    Same as container port



The next microservice is called the cartservice

---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: cartservice
spec:
  replicas: 2
  selector:
    matchLabels:
      app: cartservice
  template:
    metadata:
      labels:
        app: cartservice
    spec:
      containers:
      - name: service
        image: gcr.io/google-samples/microservices-demo/cartservice:v0.8.0
        ports:
        - containerPort: 7070
        env:
        - name: PORT                        Expects "PORT" environment variable, with value of 7070
          value: "7070"
        - name: REDIS_ADDR                  To add persistence we point to a REDIS_ADDR env variable
          value: "redis-cart:6379"          Here we define the service endpoint, so it knows how to find it
        - name: DISABLE_PROFILER
          value: "1"
        livenessProbe:
          grpc:
            port: 7070
          periodSeconds: 5
        readinessProbe:
          grpc:
            port: 7070
          periodSeconds: 5

---
apiVersion: v1
kind: Service
metadata:
  name: cartservice
spec:
  type: ClusterIP
  selector:
    app: cartservice
  ports:
  - protocol: TCP
    port: 7070
    targetPort: 7070


Next we create the redis-cart microservice

---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: redis-cart
spec:
  replicas: 2
  selector:
    matchLabels:
      app: redis-cart
  template:
    metadata:
      labels:
        app: redis-cart
    spec:
      containers:
      - name: redis
        image: redis:alpine                        Pulled from DockerHub
        ports:
        - containerPort: 6379                      Uses this port
        livenessProbe:
          initialDelaySeconds: 5
          tcpSocket:
            port: 6379
          periodSeconds: 5
        readinessProbe:
          initialDelaySeconds: 5
          tcpSocket:
            port: 6379
          periodSeconds: 5
        resources:
          requests: 
            cpu: 70m
            memory: 200Mi
          limits:
            cpu: 125m
            memory: 300Mi
        volumeMounts:                       Volume needs to be mounted inside the container
        - name: redis-data
          mountPath: /data                  Path inside the container where Redis will store the data
      volumes:                              Define the type of volume
      - name: redis-data                    Name of the volume
        emptyDir: {}                        Type of the Volume
---
apiVersion: v1
kind: Service
metadata:
  name: redis-cart
spec:
  type: ClusterIP
  selector:
    app: redis-cart                 Since these will be deployed in the same namespace all we need is the name
  ports:
  - protocol: TCP
    port: 6379                              Service Port
    targetPort: 6379                        Target Port


    Need to provide a volume for our redis application

    Redis will persist its data in memory

Different Volume Types available

    Persistent Volume Types exist beyond lifetime of a Pod

    Ephemeral Volume Types; K8s destroys volume when pod cease to exist

        emptyDir
        
        Is initially empty

        Fist created when a Pod is assigned to a Node

        Exists as long as a Pod is running

        Container crashing does NOT remove a Pod from a Node

        Therefore, data safe across container crashes!


        We going to use this type to temporarily store data in Redis



The next microservice is checkoutservice

---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: checkoutservice
spec:
  replicas: 2
  selector:
    matchLabels:
      app: checkoutservice
  template:
    metadata:
      labels:
        app: checkoutservice
    spec:
      containers:
      - name: service
        image: gcr.io/google-samples/microservices-demo/checkoutservice:v0.8.0
        ports:
        - containerPort: 5050
        env:
        - name: PORT
          value: "5050"
        - name: PRODUCT_CATALOG_SERVICE_ADDR
          value: "productcatalogservice:3550"
        - name: SHIPPING_SERVICE_ADDR
          value: "shippingservice:50051"
        - name: PAYMENT_SERVICE_ADDR
          value: "paymentservice:50051"
        - name: EMAIL_SERVICE_ADDR
          value: "emailservice:5000"
        - name: CURRENCY_SERVICE_ADDR
          value: "currencyservice:7000"
        - name: CART_SERVICE_ADDR
          value: "cartservice:7070"
        livenessProbe:
          grpc:
            port: 5050
          periodSeconds: 5
        readinessProbe:
          grpc:
            port: 5050
          periodSeconds: 5
      
---
apiVersion: v1
kind: Service
metadata:
  name: checkoutservice
spec:
  type: ClusterIP
  selector:
    app: checkoutservice
  ports:
  - protocol: TCP
    port: 5050
    targetPort: 5050


    CheckoutService needs to know:
        
        Endpoint: Service Name + Service Port for each Microservice it connects to



The next microservice is the frontend

---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: frontend
spec:
  replicas: 1
  selector:
    matchLabels:
      app: frontend
  template:
    metadata:
      labels:
        app: frontend
    spec:
      containers:
      - name: service
        image: gcr.io/google-samples/microservices-demo/frontend:v0.8.0
        ports:
        - containerPort: 8080
        env:
        - name: PORT
          value: "8080"
        - name: PRODUCT_CATALOG_SERVICE_ADDR
          value: "productcatalogservice:3550"
        - name: CURRENCY_SERVICE_ADDR
          value: "currencyservice:7000"
        - name: CART_SERVICE_ADDR
          value: "cartservice:7070"
        - name: RECOMMENDATION_SERVICE_ADDR
          value: "recommendationservice:8080"
        - name: SHIPPING_SERVICE_ADDR
          value: "shippingservice:50051"
        - name: CHECKOUT_SERVICE_ADDR
          value: "checkoutservice:5050"
        - name: AD_SERVICE_ADDR
          value: "adservice:9555"
        livenessProbe:
          httpGet:
            path: "/_healthz"
            port: 8080
          periodSeconds: 5
        readinessProbe:
          httpGet:
            path: "/_healthz"
            port: 8080
          periodSeconds: 5

---
apiVersion: v1
kind: Service
metadata:
  name: frontend
spec:
  type: NodePort
  selector:
    app: frontend
  ports:
  - protocol: TCP
    port: 80
    targetPort: 8080
    nodePort: 30007


        Frontend Service needs the Service Endpoints

        ClusterIP is an internal service type

        Frontend needs external access


Deploy Microservices into K8s cluster

    Linode

        Kubernetes

            Cluster Label: online-shop-microservice

            Region: US, Atlanta, GA

            Kubernetes version: default (1.30)

            HA Control Plane: No

            Shared CPU

            Add Linode 2 GB

            Create Cluster


            Node Pools
            Linode 2 GB

            lke222686-321965-00d235c70000
            Running
            23.239.18.155

            lke222686-321965-11b1436f0000
            Running
            173.230.134.178	

            lke222686-321965-51a3780a0000
            Running
            192.155.92.143	


            Download online-shop-microservices-kubeconfig.yaml

            Need to set permission to strict policy because it contains secrets. Need limited permissions

            chmod 400 online-shop-microservices-kubeconfig.yaml

            % export KUBECONFIG=/Users/michaelbradley/k8s-configuration/online-shop-microservices-kubeconfig.yaml

            kubectl get node - shows we are connected to the cluster

            Instead of deploying to the default name space we will create microservices

                kubectl create ns microservices

                % kubectl apply -f config.yaml -n microservices

                % kubectl get pod -n microservices

                % kubectl get svc -n microservices

            Performed get on browser: 23.239.18.155:30007 and was able to hit ONLINEBOUTIQUE

            

# Usage

% chmod 400 online-shop-microservices-kubeconfig.yaml 
michaelbradley@Michaels-iMac-2.attlocal.net /Users/michaelbradley/k8s-configuration 
% ls -l online*
total 400
-r--------@ 1 michaelbradley  staff    2824 Sep  2 17:45 online-shop-microservices-kubeconfig.yaml
    


% kubectl get node 
NAME                            STATUS   ROLES    AGE   VERSION
lke222686-321965-00d235c70000   Ready    <none>   12m   v1.30.3
lke222686-321965-11b1436f0000   Ready    <none>   12m   v1.30.3
lke222686-321965-51a3780a0000   Ready    <none>   12m   v1.30.3


% kubectl create ns microservices
namespace/microservices created

% kubectl apply -f config.yaml -n microservices 
deployment.apps/emailservice created
service/emailservice created
deployment.apps/recommendationservice created
service/recommendationservice created
deployment.apps/productcatalogservice created
service/productcatalogservice created
deployment.apps/paymentservice created
service/paymentservice created
deployment.apps/currencyservice created
service/currencyservice created
deployment.apps/shippingservice created
service/shippingservice created
deployment.apps/adservice created
service/adservice created
deployment.apps/cartservice created
service/cartservice created
deployment.apps/redis-cart created
service/redis-cart created
deployment.apps/checkoutservice created
service/checkoutservice created
deployment.apps/frontend created
service/frontend created


% kubectl get pod -n microservices
NAME                                     READY   STATUS    RESTARTS   AGE
adservice-7f5dc4b75f-5jc4n               1/1     Running   0          21s
cartservice-68f8747f94-hzq69             1/1     Running   0          20s
checkoutservice-86bdcfbbfc-2xwf2         1/1     Running   0          19s
currencyservice-6f7fd6989-vj776          1/1     Running   0          22s
emailservice-6df48986c8-s2hqx            1/1     Running   0          24s
frontend-55cf69d447-gddk4                1/1     Running   0          18s
paymentservice-c5776df6f-njkzl           1/1     Running   0          23s
productcatalogservice-8d4f48b75-smp8p    1/1     Running   0          23s
recommendationservice-69459db9c9-vlltj   1/1     Running   0          24s
redis-cart-8c5bbbccf-gnwb4               1/1     Running   0          20s
shippingservice-5cc877bc4c-htc4n         1/1     Running   0          22s


% kubectl get svc -n microservices
NAME                    TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)        AGE
adservice               ClusterIP   10.128.163.218   <none>        9555/TCP       2m1s
cartservice             ClusterIP   10.128.31.4      <none>        7070/TCP       2m
checkoutservice         ClusterIP   10.128.44.78     <none>        5050/TCP       119s
currencyservice         ClusterIP   10.128.60.177    <none>        7000/TCP       2m2s
emailservice            ClusterIP   10.128.184.162   <none>        5000/TCP       2m4s
frontend                NodePort    10.128.222.197   <none>        80:30007/TCP   118s
paymentservice          ClusterIP   10.128.86.55     <none>        50051/TCP      2m2s
productcatalogservice   ClusterIP   10.128.162.2     <none>        3550/TCP       2m3s
recommendationservice   ClusterIP   10.128.160.104   <none>        8080/TCP       2m4s
redis-cart              ClusterIP   10.128.125.205   <none>        6379/TCP       119s
shippingservice         ClusterIP   10.128.216.127   <none>        50051/TCP      2m2s
michaelbradley@Michaels-iMac-2.attlocal.net /Users/michaelbradley/k8s-configuration 
