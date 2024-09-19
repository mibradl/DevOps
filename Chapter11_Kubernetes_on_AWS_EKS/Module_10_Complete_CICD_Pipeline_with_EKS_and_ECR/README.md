# Chapter11
This module we will introduce Container Services on AWS

# Name: Introduction to Container Services on AWS

# Description: 


Overview 

    Three AWS Container Services

        Elastic Container Registry

        Elastic Container Service

            EC2 vs Fargate

        Elastic Kubernetes Service


    Container Orchestration

        Orchestrating your containters

        Microservices Apps need containers for each

        Need to deploy containers

        Containers are deployed on AWS EC2 Instances (each having docker installed)

        How are the managed?

            What resources are still available?

            Where to schedule next container?

            Are containers still running?

            How to setup load balancing?

            How to compare actual and desired state?

            Automation Tool?


    Container Orchestration Tool

        Managing, scaling and deploying containers

            Docker Swarm

            Kubernetes

            Mesos

            Nomad

            Amazon Elastic Container Service

    What is Elastic Container Service?

        Container Orchestration Service

        Manages the whole container lifecycle

            start

            rescheduled

            load-balanced


    How does ECS work?

        Run containerized application cluster on AWS

        ECS cluster contains all the services used to manage the containers

        ECS is a control plane used to manage all your servers that are running containers


    Where are these containers hosted?

        On actual machines (i.e. = VMS)

        Which virtual machines?

        In AWS these are the EC2 instances

        EC2 instances will be connected to the Control Plane and managed by the ECS Cluster


    Which services are running your EC2 Instance

        Container Runtime (Docker Agent)

        ECS Agent - for Control Plane Communication


    ECS hosted on EC2 Instance

        Manages the containers

        You still need to manage the Virtual Machine

            Create EC2 instances

            Join to ECS cluster

            Check whether enough resources

            Manager Operating System

            Docker Runtime, ECS Agent


        Full access and control of your infrastructure


    ECS versus Fargate

        ECS hosted on AWS Fargate

        Delegate Infrastructure Management also to AWS

            Containter Orchestration

            Hosting Infrastructure Management

        Fargate is alternative to EC2 Instances

        How does Fargate work?

            Serverless way to launch containers

                No Server in your AWS account. It is managed by AWS

            No need to provision and manage servers

            Launch container with Fargate

            No EC2 instance provisioned yet

            Provisions a server on demand

            No need to provision and manage servers

            Only the infrastructure resources needed to run containers

            Easily scales up & down without fixed resources defined beforehand

        EC2 Instance Pricing

            Pay for whole server

        AWS Fargate Pricing

            Based on how much capacity

        Infrastructure managed by AWS Fargate

        Containers managed by AWS

        You only need to manage your Application


            You develop the application

            Create containers

            Deploy to AWS


    Integration with other AWS Services

        AWS ecosystem available

        CloudWatch for Monitoring

        Elastic Load Balancing 

        IAM for Users and Permissions

        VPC for Networking


    Elastic Kubernetes Service

    What if you want to use Kubernetes

        Kubernetes          77%

        OpenShift           9%

        Swarm               5%

        Mesos               4%

        Rancher             3%

        Amazon ECS          2%

    EKS for managing Kubernetes cluster on AWS infrastructure

    Alternative to ECS



    Both managing the Control Plane but:


    EKS                                                             ECS

    You already use K8s (same API)                                  Specific to AWS

    Kubernetes is open-source                                       Migration difficult

    Easier to migrate to another platform                           Less complex appliations

    Large community (Helm charts etc)                               ECS Control Plane is free                         


    How Does EKS Work?

    Control Plane - Scheduling and Orchestration

        You create a cluster which represents the Control Plane

        EKS deploys and manages Kubernetes Control Plane Nodes in the background

        K8s Control Plane Services already installed on them

        High Availability - Control Plane Nodes replicated across Availability Zones

        The Etcd Store is also added and replicated across all zones


    Worker Nodes

        You infrastructure that actually runs your Nodes and Pods

        You will create EC2 instances

        Called Compute Fleet of virtual servers and connect them to EKS

        Communicate between Control Plane and Compute Fleet

    EKS with EC2 instances - self-managed

        ECS = via ECS Agent                 EKS = via K8s Worker Processes

        You need to manage infrastructure for Worker Nodes


    EKS with Nodegroup - semi-managed

        Creates, deletes EC2 Instances for you, but you need to configure it


    EKS with Fargate - fully-managed



    Steps to create EKS cluster

        Setting up a EKS cluster

            Provision an EKS Cluster with Control Plane Nodes

            Setup Worker Nodes by Creating a group of EC2 Instances using a Node Group

            Connect Nodegroup to EKS Cluster or Fargate as an alternative

            Next you can connect to the environment using kubectl to deploy containerized Apps



    Elastic Container Registry (ECR)

        What is Amazon ECR?

            Repository for Docker Images

            Store, manage and Docker container images

            Alternative to: DockerHub, Nexus

            Advantages:

                integrates well with other AWS Services

                easy to connect and configure Service with ECR


            Easy to use:

                Write code and package as Docker image

                Push docker images to private repository

                Use endpoint to download image from cluster


        In Summary

        CI/CD Pipeline

        GitLab  ---> Build App  --->    Build Image     --->    Push to Private Repo  --->  Deploy to EKS/EC2 Instances






















































# Usage


    