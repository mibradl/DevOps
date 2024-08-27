# Chapter10
This module we will look at how to secure your cluster - Authorization with RBAC

# Name: Secure Your Cluster with RBAC

# Description: 

Lecture Overiew

How Authentication and Authorization works in Kubernetes

How to configure their Users, Groups and permissions

Authorization with Role Based Access Control

Which K8s resources to use to define permissions in the cluster


Why Authorization?

Why do we need permissions?

    Admins need full access, to administer the cluster(s)   

    Developers require limited access

        Least privilege rule - meaning you give the user only what they need, not more



Role & Role Binding


        Name Service            Name Service

        App                     App


        How do you restrict access to only their name space?

        This is acheived using RBAC

        With Role component you can define namespaced permissions

            Bound to a specific name space

            What resources in that name space you can access

            Pod     Deployment      Service        Secret

            What actions can you do with this resource

            list    get     update      delete


            NS

                Can view Pods

                Can create Pods

                Can view ConfigMap


        Role only defines the resources and permissions

        No information on who gets these permissions



        How do we attach role definitions to a person or team?

        For this we have Role Binding

            We link (bind) a role to a User or Group 

            add mike to dev-team

        All members of the group get permissions defined in the Role



    Cluster Role & Cluster Role Binding

        What about Administrators?

            K8s Admins 

                Managing NameSpaces in a Cluster

                Configuring Cluster-wide Volumes

                    Using Cluster-wide Operations

        Cluster Role

            Defines resources and permissions CLUSTER-WIDE

                Can view Nodes

                Can create Nodes

                Can delete Nodes

                ** Cluster Admin

            For the Admins we would create cluster-role, admin-group and attach the cluster role to the admin-group (cluster-role binding)


    Users and Groups in Kubernetes

        How do we create Users and Groups?

        Kubernetes does not manage User natively

        Admins can choose from different authentication stategies

        No Kubernetes Objects exist for representing normal user accounts


    Relies on external Sources for Authentication

        Static Token File 

            token, user, uid

        Certificates

        3rd Party Identity Service

    Admins configures external source

    API Server handles authentication of all the requests

    API uses one of these configured authentication methods

        user.csv
        fjaf-dfpojpjo, user_1, u10001
        opeuqrpooiqjp, user_2, u10002

        Pass token file via     --token-auth-file=/users.csv        command line option

        kube-apiserver --token-auth-file=/users.csv         [other options]

    If you want to add users to a group add the group column

        user.csv
        fjaf-dfpojpjo, user_1, u10001, group1
        opeuqrpooiqjp, user_2, u10002, group2

    For Certs, you will have to manually create certificates for users

    Or configure LDAP as a authentication source

    K8s allows you to configure these external services

    But you have to manage them yourself as K8s Admin



    Service Accounts

        We gave Admins Access to whole cluster through ClusterRole

        Developers NameSpaced access through Role


        Authorization for Applications

            Human Users

            Application Users

                Applications are inside the cluster and outside the cluster

                Monitoring apps, which collect metrics from other apps from within cluster

                Microservices needing access only within their namespaces

            External Applications - CI/CD Server (Jenkins) deploying  apps inside the cluster

            Terraform infrastructure provisioning tools configuring the cluster

            Using least privilege we want Apps to have only the permission it needs!


        K8s component that represents an Application User

            Service Account

            User is not a K8s component on its own

            kubectl create serviceaccount sa1

            We will get a service account inside the cluster which represents the Apps

            Link Service Account to Role with RoleBinding

            Link Service Account to ClusterRole with ClusterRoleBinding


        Example Configuration Files

            Role Configuration File - where we define what resource have what kind of access

            We have three sections

                apiGroups "" indicates the core API Group

                resources - K8s components like Pods, Deployments etc.

                verbs - the action on a resource

                    "get", "list" (read-only) or 
                    "create", "update" (read-write) 



            apiVersion: rbac.authorization.k8s.io/v1
            kind: Role
            metadata:
              name: developer
            rules: 
            - apiGroups: [""]
              resources: ["pods"]
              verbs: ["get", "create", list]
            - apiGroups: [""]
              resources: ["secrets"]
              verbs: ["get"]


            Uses Default Namae Space

            If you would like to scope the NameSpace add namespace: my-app under metadata

            Set more granular access

            By setting resourceNames under verbs - define access to only certain pods in the namespace

              verbs: ...
              resourceNames: ["my-app"]

              verbs: ...
              resourceNames: ["my-db"]


    RoleBinding Configuration File

            apiVersion: rbac.authorization.k8s.io/v1
            kind: RoleBinding
            metadata:
              name: jane-developer-binding                      Bind Subject
            subjects:
            - kind: User
              name: jane
              apiGroup: rbac.authorization.k8s.io
            roleRef:
              kind: Role                                        to Role
              name: developer
              apiGroups: rbac.authorization.k8s.io


            subjects:
            - kind: Group
              name: "dev-admins"
              apiGroups: rbac.authorization.k8s.io    

            subjects:
            - kind: ServiceAccount
              name: default
              namespace: kube-system


    "ClusterRole" Configuration File

            apiVersion: rbac.authorization.k8s.io/v1
            kind: ClusterRole
            metadata:
              name: cluster-admin
            rules:
            - apiGroups: [""]
              resources: ["nodes"]
              verbs: ["get", "create", "list", "update"]   


            apiVersion: rbac.authorization.k8s.io/v1
            kind: ClusterRole
            metadata:
              name: cluster-admin
            rules:
            - apiGroups: [""]
              resources: ["pods", "services", "deployments"]
              verbs: ["get", "create", "list", "update"] 


        Define access for cluster-wide resources

        Define access for namespaced resources

            apiVersion: rbac.authorization.k8s.io/v1
            kind: ClusterRole
            metadata:
              name: cluster-admin
            rules:
            - apiGroups: [""]
              resources: ["namespaces"]
              verbs: ["get", "create", "list", "update"]

            Access to Pods across all Namespaces


    "ClusterRoleBinding" Configuration File

            apiVersion: rbac.authorization.k8s.io/v1
            kind: ClusterRoleBinding
            metadata:
              name: read-secrets-global                      Bind Subject
            subjects:
            - kind: Group
              name: cluster-admins
              apiGroup: rbac.authorization.k8s.io
            roleRef:
              kind: ClusterRole                                        to Role
              name: cluster-admins
              apiGroups: rbac.authorization.k8s.io


    Creating & Viewing RBAC Resources

        Create Role, ClusterRole etc just like any other Kubernetes component

            apiVersion: rbac.authorization.k8s.io/v1
            kind: Role
            metadata:
              namespace: my-app
              name: developer
            rules: 
            - apiGroups: [""]
              resources: ["pods"]
              verbs: ["get", "create", list]
              resourceNames: ["my-app"]
            - apiGroups: [""]
              resources: ["pods"]
              verbs: ["list"]
              resourceNames: ["mydb"]

            kubectl apply -f developer-role.yaml

        View them with get or describe command

            kubectl get roles

            kubectl describe role developer


    Checking API Access

        kubectl provides a auth can-i sub-command

        to quickly check if current user can perform a given action

            kubectl auth can-i create deployments --namespace dev

        Admins can also check permissions of other users in the cluster to see what permission someone has in a name space


    
    Wrap up

        Layers of Security

        Two different levels

        1) Authentication

            Jenkins sends a request to API Server

                Create new Service in Default namespace

                On first level API server will authenticate the user and checks Jenkins User is authenticated

                Is Jenkins allowed to connect to the cluster at all?

                You can enable multiple authentication methods at once


        2) Authorization

           RBAC is one of multiple Authorization Modes

           With RBAC: Role, Cluster Role and binding are checked

                Does User have permission to perform this specific task 




        
















          
        

















# Usage


    