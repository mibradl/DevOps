# Chapter10
This module we will look at Minikube and Kubectl on local Kubernetes Cluster
# Name: MiniKube & Kubectl local Kubernetes Cluster

# Description: 

What is minikube?

What is kubectl

Setup Minikube cluster


    What is Minikube?

    In a production Clustrer Setup

        Multiple Control Plane and Worker nodes

        Separate virtual or physical machines

    Test/Local Cluster Setup - minikube runs both nodes

        Control Plane and Node processes run on a single machine

        Docker pre-installed

    What is Kubectl - command line tool for K8s cluster

        Control Plane processes communicate with API Server

        There are 3 clients that can talk to API

            UI

            API

            CLI - Kubectl the most powerful of the 3 clients

        Worker processes

            enable pods to run on the node

                create pods

                create services

                destroy pods

        Kubectl is for:

            Minikube cluster

            Cloud cluster

    Installation & Create Minikube cluster

    https://minikube.sigs.k8s.io/docs/start/ 

    minikube start
    minikube is local Kubernetes, focusing on making it easy to learn and develop for Kubernetes.

    All you need is Docker (or similarly compatible) container or a Virtual Machine environment, and Kubernetes is a single command away: minikube start

        Run Minikube either as a container or Virtual Machine on your laptop

        Resources

        What you’ll need:

        2 CPUs or more
        2GB of free memory
        20GB of free disk space
        Internet connection
        Container or virtual machine manager, such as: Docker, QEMU, Hyperkit, Hyper-V, KVM, Parallels, Podman, VirtualBox, or VMware Fusion/Workstation

        brew install minikube

        Must start minikube as Container or Virtual Machine on our laptop

        Drivers

            2 Layers of Docker

            1) Minikube runs as Docker container

            2) Docker inside Minikube to run our application

            Minikube has Docker pre-installed to run the containers in the cluster

            Make sure Docker Desktop is running

            minikube start --driver docker command

            minikube status

            Driver means we are hosting Minikube as a container on our local machine

            Minikube has kubectl as dependency

            No separate installation is necessary










# Usage


michaelbradley@Michaels-iMac-2 ~ % minikube start
😄  minikube v1.33.1 on Darwin 12.1
✨  Automatically selected the docker driver
📌  Using Docker Desktop driver with root privileges
👍  Starting "minikube" primary control-plane node in "minikube" cluster
🚜  Pulling base image v0.0.44 ...
💾  Downloading Kubernetes v1.30.0 preload ... 


michaelbradley@Michaels-iMac-2 ~ % minikube status
minikube
type: Control Plane
host: Running
kubelet: Running
apiserver: Running
kubeconfig: Configured

🔥  Creating docker container (CPUs=2, Memory=2200MB) ...
🐳  Preparing Kubernetes v1.30.0 on Docker 26.1.1 ...
    ▪ Generating certificates and keys ...
    ▪ Booting up control plane ...
    ▪ Configuring RBAC rules ...
🔗  Configuring bridge CNI (Container Networking Interface) ...
🔎  Verifying Kubernetes components...
    ▪ Using image gcr.io/k8s-minikube/storage-provisioner:v5
🌟  Enabled addons: default-storageclass, storage-provisioner
🏄  Done! kubectl is now configured to use "minikube" cluster and "default" namespace by default


Get status of nodes

        kubectl get node

        michaelbradley@Michaels-iMac-2 ~ % kubectl get node
        NAME       STATUS   ROLES           AGE   VERSION
        minikube   Ready    control-plane   12m   v1.30.0

        Kubectl CLI ...for configuring the Minikube cluster

        Minikube CLI ...for start up/deleting the cluster