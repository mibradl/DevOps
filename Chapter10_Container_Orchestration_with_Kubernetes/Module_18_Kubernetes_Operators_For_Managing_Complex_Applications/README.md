# Chapter10
This module we will look at Kubernetes Operators

# Name: Kubernetes Operators for Managing Complex Applications

# Description: 

Kubernetes Operators

    used for stateful Applications



    Stateless Applications on K8s

        
        Deployment          ConfigMap           Service

        

        my-app      my-app      my-app              scaled with three replicas

                                                    If one fails control loop recovers one in its place

                                                    If you release a new version you simply update the Deployment configuration and all replicas are restarted with new version

                                                    No backups are necessary for stateless apps

                                                    No control is necessary, after app is deployed

                                                    Updates and scaling are very seamless


                                                    All these tasks are automated by Kubernetes, using this Control Loop mechanism. So, Kubernetes knows what the desired state is based on the configuration files. Thus, it observes state, checks for differences, and takes action


                                                    Hence, it able to recreate pods that have died

                                                    Restart updated pods



    Stateful Applications without Operator

        For the web app you need a database, and data persistence

        Requires more hand-holding

        Throughout the whole lifecyle

        This is difficult because when you create three replicas of a mysql each is different

        Each have their own state and identity

        Order important, which means they must be updated and destroyed in a certain order

        Process is different for each application (MySQL, Postgres, elasticSearch)

        No standard solution

        Normally require manual intervention

        Require people that operate these applications, which goes against best practice of automation, self healing, etc

        Some choose to place application outside K8s??

        Other applications need K8, such as Prometheus, or etcd, which are stateful applications

        So, how do you manage stateful applications, which is the Operator

        

        What is an Operator?



    Stateful Applications with Operator

        Replaces a human Operator with a software Operator

        All the manual tasks that a person would perform is now packee inside of a program
        
        Has intelligence about how to deploy the app

        How to create a cluster of replicas

        How to recover with one replica fails

        Tasks are automated and reusable

        If you have two clustered configured you don't have to manually configure both

        There is one standard automated process that performs this on both



        How does this work?

            Control Loop Mechanism (Observe, Check differences, Take action)

            Kubernetes watches for changes

                if a replica dies it creates a new one

                did a configuration change, it applies current configuration

                new image get applied, it updates the image with correct version

            Makes use of CRD's (Custom Resource Definitions)

                Custom K8s component (extends K8s API)

                By default you have these components (Sevice, Deployment, StatefulSet)

                On top of that you can create your own custom component (CRDs)

                
            Operator via the Custom Loop takes default components and thru Domain/app-specific knowledge automates entire lifecycle of the app it operates


    Summary

        Managing complete lifecycle of statless apps

            No Business logic

        K8s can't automate the process natively for stateful apps, thus it uses extensions called Operators 

        Use it's own Operator for each application (prometheus-operator, mysql-operator, postgres-operator)



        Who creates Operators?

            Operators are built by experts in the business logic of installing, running and updating that specific application (prometheus, postgres, elasticsearch, mysql)


            my-sql      my-sql      my-sql     my-sql

            A my-sql team builds an Operator for:

                How to create mysql cluster

                How to run it

                How to sychronize the data

                How to update it


            There are separate operator for each application

            Many already exist from various communities

            There is an OperatorHub.io where thes Operators can be looked up

            
            Operator SDK to create own operator

            












# Usage


    