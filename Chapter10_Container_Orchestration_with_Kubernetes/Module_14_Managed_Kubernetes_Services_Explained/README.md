# Chapter10
This module we will explore managing kubernetes services

# Name: Managed Kubernetes Services Explained

# Description: 

Managing Kubernetes on the cloud


    Managed Kubernetes Service

    Build a Use Case

    Application to deploy on Kubernetes

        Web app with database

        This would be deployed to K8s cluster

        You want you application available from browser via https://

        Security for Cluster

        Data persistence for DB

            Create environment in Dev and Prod so you can properly test before releasing

        Setup as efficiently as possible


Managed versus Unmanaged K8s Cluster

    Kubernetes on Cloud Platform like Linode

    Two Options

        Option 1

        Create own cluster from scratch, comprised of 6 Nodes - 3 Control Nodes and 3 Worker Nodes

        Install Control Plane processes on Control Node

        Install Worker processes; kubectl, kube-proxy and container runtime, such as docker on Worker Nodes

        Once you have set them up you have a Kubernetes Cluster

        Now you can deploy your application to the cluster

        This is your responsibility to manage (including security and backups)

        Including the underlying infrastructure and servers

        Not practical when you want to setup things fast and easily


        Option 2

        Cloud providers offer managed K8s service

        On Linode its called a Linode K8s Engine - LKE

        You don't have to create a cluster from scratch

        Most is done for you automatically by Linode platform

        You only care about Worker Nodes

        You will three or more worker nodes with all the necessary processes, including docker runtime

        Everything pre-installed


        What about the Control Plane Nodes?

        Who creates them?

        Who manages them?

        Control Plane Nodes created and managed by Cloud Provider

        You only pay for the Worker Nodes

        Less effort and time



    Managed K8s cluster

        LKE as an Example

        1) Spin up your K8s cluster on cloud

            Choose Worker Nodes and their resources

            Select region/data center

            Connect using kubectl

        2) Data Persistence for your cluster

            Mongo DB Database with Node.js application

            3 replicas storage for all 3 (types: cloud, nfs and local)

            You need to configure it yourself

                a) Create Physical storage

                b) Persistent Volume

                c) Attach volumes to your database

                Linode Block Storage eliminates these steps

                    1) You use Linodes Storage Class

                    2) Linode creates:

                        Persistent Volumes 
                        
                        With Physical Storage

        3) Load balancing your K8s cluster

            Node.js App running

            MongoDB app 

            Storage configured 

            Services and Ingress enable access from the browser

            Ingress is part of Kubernetes

                Manages routing of incoming requests

                From browser to internal services

            You need to install and run Ingress Controller in K8s cluster

            And configure your routing rules, and route incoming traffic to Node.js application


            How does this work in Managed K8s Service or LKE specifically?

                It happens through Linodes LoadBalancer

                Cloud provider's own Load balancer implementation

                Which is the entrypoint of your cluster


            Load balancer for Worker Nodes

                A cluster with two nodes

                One with database application

                One with web server, which is exposed using public IP address

                Browser can send requests directly to the web server


            Challenges

                You can't scale this application

                If you start getting a lot of traffic your application will become a bottleneck

                If you make changes or have to restart it your server becomes unavailable


           Linode's NodeBalancer

                If you add a NodeBalancer, it takes incoming requests and directs it to the web server

                NodeBalancer will get the public IP address

                Web server only accessible thru private IP

                You can add multiple web servers under the load balancer so you can forward your requests to web servers and scale up/down

                This can be transparent to users

                Setup entry only once

                You can create session stickiness

                    Route subsequent requests for same client to the same backend

                Secure your connection with SSL Certificate

                    Configure with cert Manager

                The concepts you learned on Linode can be used for other cloud platforms as well


            4) Data Centers for your K8s cluster

                Region = Dallas

                You want to host your application in closer procsimity to your users

                This is where availability zones come in


            5) Move app from one cloud to another

                Move all data to our private cloud

                Migrate part to a private cloud

                Migrate part of your app to another cloud platform


                When using a cloud provider, you use services specific to that cloud provider

                    your app gets closely tied up to that cloud provider

                    you may require re-programming and re-configuration 

                Vendor lock-in


            6) Automating Tasks

                Your setup grows

                Your infrastructure configuration gets more complex

                Your DevOps team grows

                Automate as much as possible

                    automate creating

                    automate configuring

                    your infrastructure

                    automate deploying your apps and services

                Automation Tools:

                    Terraform

                        Providers - that can access Linode Resources

                    Ansible

                        Modules
























































# Usage


    