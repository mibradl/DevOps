# Chapter10
This module we will discuss K8s Namespaces

# Name: Namespaces Organizing Components

# Description: 

What is a Namespace?

Organize resources in namespaces

    A virtual cluster inside of a cluster

    4 namespaces per default

michaelbradley@Michaels-iMac-2 k8s-configuration % kubectl get namespace                 
NAME              STATUS   AGE
default           Active   36h
kube-node-lease   Active   36h
kube-public       Active   36h
kube-system       Active   36h

    kube-system is not meant for your use, Do Not create or modify kube-system

        These are system processes

        Control Plane and kubectl processes

    kube-public

        Contains publicly accessible data

        A configMap that contains cluster information

        michaelbradley@Michaels-iMac-2 k8s-configuration % kubectl cluster-info
        Kubernetes control plane is running at https://127.0.0.1:57321
        CoreDNS is running at https://127.0.0.1:57321/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy

    kube-node-lease   

        Holds information about the heart beat of Nodes

        Each Node has associated lease object in namespace

        Determines the availability of a Node

    default

        Resources you create are located here


  Create own namespace


michaelbradley@Michaels-iMac-2 k8s-configuration % kubectl create namespace my-namespace

namespace/my-namespace created

michaelbradley@Michaels-iMac-2 k8s-configuration % kubectl get namespace

NAME              STATUS   AGE
default           Active   37h
kube-node-lease   Active   37h
kube-public       Active   37h
kube-system       Active   37h
my-namespace      Active   19s

 

 Another way to create you own namespace is using namespace configuration file, because you have a history

apiVersion: v1
kind: Namespace
metadata:
  name: my-namespace


Why use namespaces?

    1. First is everything in one Namespace?  

        If everything defaults to one namespace it becomes difficult to track resources

    2. Group resources into a namespaces

        Database and its resources

        Monitoring (Prometheus and everything it needs)
       
    3. Elastic Stack in namespace

        Elastic Search resources

    4. Nginx-Ingress

    These are ways of logically grouping your resources inside of the cluster

    Officially Kubernetes recommends you should not have namespaces if you have ten or fewer users.

    5. If you have multiple teams

       Many times, same application can create conflicts

       Each team can work in their own namespace without disrupting the other


    6. You want to host both a staging and Development environment, in the same cluster.

    7. Resource sharing blue/green Deployment.

        Two different versions of production

        One is active now and the other is going to be production.

    8. Access and Resource Limits on Namespace.

        Two teams working in the same cluster

        They can only do things in their own Namespace

        By restricting access to the other Namespaces resources

        Limited: CPU, RAM, Storage


    Use cases to use Namaespaces

    1. Structure your components

    2. Avoid conflicts between teams

    3. Share services between different environments

    4. Access and Resource Limits on Namespaces Level


Characteristics of Namespaces

    You can't access most resources from another Namespace

    If you have a ConfigMap in project A you can't use it in project B

    Each Namespace must define its own ConfigMap

    The same applies to Secrets, each Namespace must include its own configuration 

    However, a resource that can be shared is the Service

    For example the service is included in the name

        data:
          database_url: mongodb-service.database

    
    Components that can't be created within a Namespace

        Live globally in a cluster

        You can't isolate them

        Examples of such resources are:

        volumes - accessible to the entire cluster

        persistence volume

        node


You can list those resources that are and are not bound to a namespace:

        kubectl api-resources --namespaced=false

        kubectl api-resources --namespaced=true


Create component in a Namespace

        By default, components are created in a default NS, if not specified

        To add a namespace to a configMap

            kubectl apply -f mongo-configmap.yaml --namespace=my-namespace

        Another way to configure it is in the configuration file

        apiVersion: v1
        kind: ConfigMap
        metadata:
          name: mongdb-configmap
          namespace: my-namespace

To display a resource in a namespace other than default

        kubectl get configmap -n my-namespace


As a best practice;

        Use configuration file over kubectl cmd

        Better documented

        More convenient using automation


Change active namespace

        You can change the default namespace to any other namespace

        Change the active namespace with: kubectl

        kubectl config set-context --current --namespace=my-namespace

            Define namespace for current context

            Subsequent  commands will use that context



        Change the active namespace with: kubens

            More convenient, but needs to be installed separately

            kubens command will list namespaces and highlight the one active

            michaelbradley@Michaels-iMac-2 k8s-configuration % kubens
            default
            kube-node-lease
            kube-public
            kube-system
            my-namespace


            michaelbradley@Michaels-iMac-2 k8s-configuration % kubens my-namespace
            Context "minikube" modified.
            Active namespace is "my-namespace".




# Usage
            brew install kubectx


            michaelbradley@Michaels-iMac-2 k8s-configuration % brew install kubectx
==> Auto-updating Homebrew...
Adjust how often this is run with HOMEBREW_AUTO_UPDATE_SECS or disable with
HOMEBREW_NO_AUTO_UPDATE. Hide these hints with HOMEBREW_NO_ENV_HINTS (see `man brew`).
==> Auto-updated Homebrew!
Updated 3 taps (homebrew/services, homebrew/core and homebrew/cask).
==> New Formulae
libmps                                                        safety                                                        stellar-cli
==> New Casks
bbackupp                                                                                     cork

You have 10 outdated formulae installed.

==> Downloading https://ghcr.io/v2/homebrew/core/kubectx/manifests/0.9.5-2
################################################################################################################################################################################## 100.0%
==> Fetching kubectx
==> Downloading https://ghcr.io/v2/homebrew/core/kubectx/blobs/sha256:6552e91e68ff8abda73be837c80539b47e3aadc73e5f8bab57cbb3bf0356c682
################################################################################################################################################################################## 100.0%
==> Pouring kubectx--0.9.5.all.bottle.2.tar.gz
==> Caveats
zsh completions have been installed to:
  /usr/local/share/zsh/site-functions
==> Summary
🍺  /usr/local/Cellar/kubectx/0.9.5: 15 files, 41.1KB
==> Running `brew cleanup kubectx`...
Disable this behaviour by setting HOMEBREW_NO_INSTALL_CLEANUP.
Hide these hints with HOMEBREW_NO_ENV_HINTS (see `man brew`



michaelbradley@Michaels-iMac-2 k8s-configuration % kubens
default
kube-node-lease
kube-public
kube-system
my-namespace









    

