# Chapter10
This module we will review the main components of Kubernetes

# Name: Main Kubernetes Components

# Description: 

Role of each component

    Pod

    Service

    ConfigMap

    Deployment

    Ingress

    zecrets

    StatefulSet


    Node = virtual and physical machine

    Pod  =  Smallest unit in Kubernetes

            Abstraction over a container, a layer on top of the container

            You only interact with the Kubernetes layer

            Usually 1 application per Pod

            Offers a virtual network, with each Pod being assigned an IP per Pod
            
            Each Pod can communicate using its own internal IP address

            Pods are ephemeral, thus it can die because it runs out of resources. In this case it will shutdown, and replaced with a new Pod, with a new IP adddress on creation.

            Because of this another component called Service is used

        Service

            A permanent IP address

            Lifecycle of Pod and Service are not connected

            So, even if the Pod dies, it's Service and IP remain

            App should be accessible through a browser

                http://node-ip:port

                External Service

            DB would not be accessible externally, hence it would be managed through a Internal Service

            You specify the type of Service on creation

                http://124.89.101.2:8080

                not practical for end product

                should look like this:

                https://my-app.com

        Ingress

            Instead of calls going directly to the service, they go to the Ingress component and in turn routes to the appropriate Service


            So app will communicated to an endpoint such as:
            mongo-db-service.

            Database URL usually in the built application.

            This would require a rebuild, push it to the repo and pull it in your Pod, which is pretty complex.

            For this reason Kubernetes has a configuration called  configMap.



        ConfigMap

            DB_URL = mongodb-db-service

            External Configuration of your application

            Thus, you only need to adjust the configMap, you don't have to build a new image or go thru the complete cycle.

            Other data needed for the DB is the 
                db_user
                db_pwd

            For this reason there is another component called zecret

        zecret

            Used to store zecret data

            Stored in Base64 encoded format, instead of clear text.

            The built-in security mechanism is not enabled by default.

            Check out the official docs for more info on this


            These are typially third-party.

            Things stored are credentials, etc

            
            Reference zecret in Deployment/Pod

            Use it as environment variables or as a properties file


    Volumes

            Data Storage

            If the service gets restarted the data would be gone

            Storage on local machine

            Or remote, outside of the K8s cluster

            So, there is a distinction between the Kubernetes Cluster

            Think of the storage as a external drive hard drive plugged in to your Kubernetes Cluster

            Kubernetes doesn't manage data persistence


    Deployment

            So everything is replicated.

            So, there would be a cloned applicaiton on a separate node, with a dns name

            Service

                Permanent IP

                Load balancer, that determines which Pod is not as busy

            You would not define a second Pod you would 
            specify how many replicas you want to have. this is referred to Blueprint for "my-app" pods in Deployment.

            You create Deployments, there you can specify how many Pods to use, by scaling up or down.

            Abstraction of Pods

            Would normally work with deployments and not Pods


            If the DB Pod fails can't replicate via deployment, because has state and would need to share the same data storage.

            Need some kind of mechanism to ensure db Pods are reading and writing the same information, to avoid data inconsistencies.

    Stateful Set

            This is intended for stateful apps

                MySQL
                Mongo
                Elastic  Search

            Deployment = for stateless Apps

            Stateful Set is for stateful Apps or Databases and would take care of scaling, and assuring Pods are syncronized.

            Deploying StatefulSet is not easy

            Databases are often hosted outside of the Kubernetes cluster

            If an entire Node failed both Pods would be available on second Node, allowing the first to be recreated, so you can avoid downtime.

    DaemonSet

            We want to collect logs from each Node and collect logs for each Pod running. 

            Kube-proxy needs to run on every Node.

            You create Deployments with replica count 2 to match Node's in our cluster. When we add Nodes we need to adjust the replica number

            When we delete Nodes we need to adjust replica number again

            With Deployment, we also can't ensure that Pods are equally distributed.

            DaemonSets are created for such cases

            Calculates how many Replicas are needed based on existing Nodes

            Deploys just one Replica per Node

            When Nodes are added, Pods are added to them

            When Nodes are removed, those Pods are gargabe collected

            No need to define replica count

            Automatically scales up & down

    Wrap up - summarized

            Pod - abstraction of containers

            Service - communication

            Ingress - route traffic into cluster

            ConfigMap
                        - external configuration
            zecret

            Volume - data persistence


            Looked at Pod blueprints for replication, using

            Deployment

            StatefulSet - such as databases












            

            











































# Usage


    