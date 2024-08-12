# Chapter10
This module we will look at the Kubernetes Architecture
# Name: Kubernetes Architecture

# Description: 

Kubernetes Architecture

    Two types of Nodes or Machines

        Control Plane

        api server
        scheduler
        controller manager
        etcd

        Worker Nodes

        Node
        pod
        container-d
        kubelet


    Node Processes

        Worker Machine in K8s Cluster

        My-app
        DB

        Each Node has multiple Pods on it

        Three processes must be installed on every Node

        Worker Nodes do the actual work 

            Container runtime - docker, container-d & crio

            Kubelet schedules Pods running on the Node. Kubelet interacts with both the container and node

                Kubelet starts the pod with a container inside

            Kube proxy forwards the requests and must be installed every Node


            These must be running on every Node in order for K8s Cluster to run properly



        So, how do yo interact with this cluster?

            How do we:

                Schedule Pod?

                Monitor?

                Re-schedule/restart pod?

                Join a new Node?

        Managing processes are done by the Control Plane Nodes

        Control Plane Processes

        Four processes run on every control plane node

            Api Server - you interact with some client (UI), CLI (Kubelet), Kubernetes API

                It acts as the Cluster gateway, which handles updates or queries

                Acts as a gatekeeper for authentication!

                Anytime there is a request it must go through the API Server, who in turn validates the request

                If fine, forwards request to other processes to schedule the pod or other process

                There is one entrypoint into the cluster

            
            Scheduler

                If you send a request to the API Server to schedule a new Pod after validation it is sent to the Scheduler

                Scheduler has intelligence on the next worker Node to put the Pod, and determines how many resources (CPU, Mem, etc) your Pod will need, checks worker nodes to determine placement

                Whatever worker node has the most resources is where it is placed.

                Kubelet actually starts the Pod on the Node

            Controller Manager

                When Pods die there must be away to identify the Pod has failed.

                Detects cluster state changes. When Pods die Controller Manager tries to recover cluster state as soon as possible.

                Controller Manager makes are request to the scheduler to reschedule those bad Pods

                Based on resource calculation of resources availale on Nodes new Pods are scheduled via the Kubelet to restart the Pod

            etcd

                Key Value Store of a cluster state

                Etcd is the cluster brain!

                Cluster changes get stored in the key value store

                This works because of its data

                How does Scheduler know what resources are available on a Node?

                How does the Control Manager know the cluster state has changed?

                If the cluster is healthy?

                All this information is stored in the etcd

                Application data is NOT stored in etcd

                The API server is load balanced

                Distributed storage across all master nodes


    Example Cluster Setup

            In a small cluster there would probably have:

                2 Control Plane Nodes

                3 Worker Nodes

                    Hardware resources are different on Control Node versus Worker Nodes - CPU | RAM | STORAGE

                Control Plan Nodes have less workload and therefore need fewer resources

                Worker Nodes have higher workload with the containers inside and therefore require more resources.  


        To add new Control Plane / Node server:

                Get new bare metal server 

                Install all the control plane / worker node processes  

                Join to the cluster    




       











# Usage


    