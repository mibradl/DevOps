# Chapter10
This module we will look at StatefulSet

# Name: StatefulSet - Deploying Stateful Applications

# Description:

Kubernetes StatefulSet

    What is StatefulSet?

        Used for Stateful applicatoins

        Examples of stateful applications - old databases (MySQL, ElasticSearch, Mongodb)

        Any application that stores data to keep track of its state

    Stateless Applications

        Don't keep record of state

        Each request is completely  new

    Why StatefulSet is used?

    How StatefulSet works and how it's different from Deployment?


        HTTP        When a a request comes into Nodejs doesn't depend on previous data
          |
          |         You can handle it on the payload itself  
          V

        Nodejs      passthrough for data query/update
          |
          |         Update/query
          V
        MongoDB     Update data based on previous state or query data

                    Depends on most up to data data/query


Deployment of stateful and stateless applications

    Stateless applications are deployed using Deployment

        Deployment are an abstraction of Pods

        Allows you to Replicate your app on the cluster


    Deployment of stateful applications

        Deployed using StatefulSet

        Allows you to replicate the Pods

    Both manage Pods on Container Specification

    Can configure storage and data persistence on both the same way


    So, why do we use different components?



Deployment vs StatefuSet

    Replicating stateful applications is more difficult

    Has additional requirements


        Mysql
        
        handles requests from Java app which is deployed using a deployment component


        apiVersion: apps/v1
        kind: Deployment
        metadata:
          name: my-app
          labels: 
            app: my-app
        spec:
          replicas: 3
          selector:
          ...

        scale the application to three Pods so they can handle more client requests - easy to scale


        apiVersion: apps/v1
        kind: StatefulSet
        metadata:
          name: my-db
          labels: 
            app: my-db
        spec:
          replicas: 3
          selector:

        In parallel you want to scale your mysql to handle more java request

        Will be identical and interchangeable

        Create in random order with random hashes (my-app-f5c304e)

        1 Service that load balances to any Pod

        When you delete them the are removed in random order


        mysql Pods are more difficult

        Can't be created / deleted at the same time

        Can't be randomly addressed

        The replica Pods are not identical - Pod identity

    Pod Identity

        Maintains identity for each pod

        Create from same specification, but not interchangeable!

        Persistent identifier is kept across any rescheduling, due to a restart

    Why is this identity necessary


Scaling database applications

        When you start with a single mysql Pod it will be used for reading / writing data

        When add a second replica it can not act the same way

        If you allow two independent mysql instances to change the same data you will end up with data inconsistency

        Only one instance is able to write, reading is fine

        The read/write instance is called the main and the others are called replicas

        They do not use the same physical storage /data/vol/pv-0, /data/vol/pv-1, /data/vol/pv-2

        They must continuously synchronize the data

        The replicas must know about each change to be up to date

        Main            Replica         Replica         Replica
        mysql-0         mysql-1         mysql-2         mysql-3


        PV              PV              PV              PV - New
        data B          data B          data B          data B
        data A          data A          data A          data A
        /data/vol/pv-0  /data/vol/pv-1  /data/vol/pv-2  /data/vol/pv-3

        When a new Pod joins the existing setup, it first must clone itself from the previous Pod (pv-2)

        Once it has the previous clone it starts the continuous synchronization to listen to any updates from master Pod

        Temporary storage theoretically possible, but data will be lost when all Pods die

        With data persistence, data will survive, even when all Pods die

        Persistent Volume lifecycle isn't tied to other components lifecycle


        The way to do this, configuring persistent volumes for your stateful sets

            Each Pod has it's own state

            When it dies the storage has the state of the original Pod

            For this to work it important to use remote storage

            If the Pod gets rescheduled to another node the previous storage must be available this node as well

            Can't do this with local volume storage because they are tied to the node


        Pod Identity

            Every Pod has its own identifier

                Stateless Pods get random hash, Stateful Pods get fixed ordered names $(statefulset name)-$(ordinal)

                Stateful with three replicas

                mysql-0     mysql-1     mysql-2   
                Main        replica     replica


                Next Pod is only created, if previous is up and running

                Deletion of StatefulSet or scale down to replica 1

                Deletion in reverse order, starting from the last one, once it is removed it will delete the next (3, 2, 1)

                Protect data and state

                Each Pod gets its own DNS Endpoint from Service

                1) loadbalancer service         Same as Deployment

                2) individual service name for each Pod, which deployment Pods do not have

                    ${pod name}.${governing service domain} service name defined in statefuleSet

                    a) predictable pod name mysql-0

                    b) fixed individual DNS name mysql-0.svc2

                        When Pod restarts:

                            IP address changes

                            name and endpoint stays the same

                Sticky Identity

                    retain state

                    retain role


                Replicating stateful apps

                    It's complex

                    Kubernetes helps you

                    You still have to configure it

                    Must configure the cloning and data synchronization

                    Make remote storage available, as well as manage and backup


            Stateful applications are not a perfect candidate for containerized environments

            Docker Kubernetes is perfect for stateless applications
































# Usage


    