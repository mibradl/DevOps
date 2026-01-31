# Chapter10
This module we will look at Ingress connection outside the cluster

# Name: Ingress Connecting To Application Outside Cluster 

# Description: 

Kubernetes Ingress

What is Ingress?

Ingress YAML Configuration

When do we need Ingress?

Ingress Controller


External Services versus Ingress


External

        http://my-service-ip:port       browser

        my-service-ip:port              external
       
        my-app                          pod


        Not secure



Ingress

        https://my-app.com              pod

        my-app-ingress                  ingress

        my-app-service                  internal

        my-app                          pod



        The way to do that is using Ingress

        IP address and port is not opened!

        If a request comes from the browser, it first will reach ingress

        It then will be re-routed to the internal service

        Will eventuall end up at the Pod


YAML Configuration Files

External vs Ingress


        Example of Ingress

        apiVersion: networking.k8s.io/v1
        kind: Ingress                                                   kind: Ingress
        metadata:
        name: myapp-ingress 
        spec:
        rules:                                                          Routing rules:
        - host: myapp.com
            http:                                                           Forward request to the internal service
            paths:                                          
                - path: /                                                   http://my-app.com  would be issued in the browser
                pathType: Prefix                                            This is the uri following the url
                backend:
                    service:
                    name: myapp-internal-service                            browsser request would be rerouted to the internal service
                    port:
                        number: 8080

        Valid domain address

        Map domain name to the IP address, which is the entrypoint


        kind: Service
        metadata:
          name: myapp-internal-service
        spec:
            selector:
              app: myapp
            ports:
              - protocol: TCP
                port: 8080
                targetPort: 8080

        No nodePort in internal service

        Instead of LoadBalancer default type, ClusterIP
                

    Ingress and Internal Service configuration


How to configure Ingress

  The YAML created Ingress component

      You need an implementation of Ingress

      Which is Ingress Controller

      Evaluates and processes Ingress rules in your cluster

      Manage redirections

      Entrypoint to cluster

      Many third-party implementations


      Install K8s Nginx Ingress Controller




      Must consider the environment on which your cluster runs

        If you are running Cloud Service Provider:

            Google Cloud          AWS

            Azure                 Digital Ocean


            Out of the box K8s solutions

            Own virtualized LoadBalancer

            Request would hit the Cloud Load Balancer and would be routed to Ingress-controller

            You can configure it in different ways

              Advantage:

              You don't have to implement LoadBalancer yourself


      Running Bare Metal

            You need to configure some kind of entrypoint

            Either inside of cluster or outside as separate server

                Proxy Server

                Software or hardware solution

                  Separate server

                  Public IP address and open ports

                  Act as Entrypoint to cluster

                  No server in K8s cluster is accessible from outside!


                    http://my-app.com

                    Proxy server

                    Ingress Controller checks Ingress rules (for what rules apply to the request)



        Configure Ingress in Minikube

              Automatically starts the K8s NGINX implementation of Ingress Controller

              Minikube uses minikube tunnel to enable ingress access

              Ingress Controller configured with just one command

              Create Ingress rule


                  Enable minikube dashboard

                  minikube dashboard


              Not accessible externally!


              Once the resources are running for pod and service dashboard the rules can be created


              Run kubectl apply -f dashboard-ingress.yaml


               minikube tunnel

              ✅  Tunnel successfully started

              📌  NOTE: Please do not close this terminal as this process must stay alive for the tunnel to be accessible ...

              🏃  Starting tunnel for service mongo-express-service.
              ❗  The service/ingress dashboard-ingress requires privileged ports to be exposed: [80 443]
              🔑  sudo permission will be asked for it.
              🏃  Starting tunnel for service dashboard-ingress.


              Multiple paths for same host

              http://myapp.com/analytics
                        |
                        V
              analytics service
                        |
                        V
              analytics pod


              http://myapp.com/shopping
                        |
                        V
              shopping service
                        |
                        V
              shopping pod

              apiVersion: networking.k8s.io/v1
              kind: Ingress
              metadata:
                name: simple-fanout-example
                annotations:
                  nginx.ingress.kubernetes.io/rewrite-target: /
              spec:
                rules:
                - host: myapp.com
                  http:
                    paths:
                      - path: /analytics
                        backend:
                          service:
                            name: analytics-service
                            port:
                              number: 3000
                      - path: /analytics
                        backend:
                          service:
                            name: analytics-service
                            port:
                              number: 8080

              Multijple sub-domains or plugins


              Instead of:

                      http://myapp.com/analytics


                      http://analytics.myapp.com


              Instead of 1 hosts and multipe paths

              You have multiple hosts with 1 path.
              Each host represents a subdomain



          Configuring TLS Certificate - https://

          tls

          secretName



              apiVersion: networking.k8s.io/v1
              kind: Ingress
              metadata:
                name: tls-example-ingress
              spec:
                tls:
                - hosts: 
                  - myapp.com
                  secretName: myapp-secret-tls                                secret name
                rules:
                - host: myapp.com
                  http:
                    paths:
                      - path: /analytics
                        backend:
                          service:
                            name: myapp-internal-service
                            port:
                              number: 8080


              apiVersion: v1
              kind: Secret
              metadata:
                name: myapp-secret-tls                                        secret name
                namespace: default
              data:
                tls.crt: base64 encoded cert
                tls.key: base64 encoded key
              type: kubernetes.io/tls




              Data keys need to be "tls.crt" and "tls.key"

              Values are file contents Not file paths/location

              Secret component must be in the saem namespace as the Ingress component, can't reference a secret from another namespace
              
                    








                




# Usage


     kubectl get ns
NAME                   STATUS   AGE
default                Active   3d22h
kube-node-lease        Active   3d22h
kube-public            Active   3d22h
kube-system            Active   3d22h
kubernetes-dashboard   Active   68m
my-namespace           Active   2d9h


kubectl get all -n kubernetes-dashboard
NAME                                            READY   STATUS    RESTARTS   AGE
pod/dashboard-metrics-scraper-b5fc48f67-52vf9   1/1     Running   0          92m
pod/kubernetes-dashboard-779776cb65-qhfmt       1/1     Running   0          92m

NAME                                TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)    AGE
service/dashboard-metrics-scraper   ClusterIP   10.102.219.190   <none>        8000/TCP   92m
service/kubernetes-dashboard        ClusterIP   10.110.27.89     <none>        80/TCP     92m

NAME                                        READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/dashboard-metrics-scraper   1/1     1            1           92m
deployment.apps/kubernetes-dashboard        1/1     1            1           92m

NAME                                                  DESIRED   CURRENT   READY   AGE
replicaset.apps/dashboard-metrics-scraper-b5fc48f67   1         1         1       92m
replicaset.apps/kubernetes-dashboard-779776cb65       1         1         1       92m

Create Ingress Dashboard

kubectl apply -f dashboard-ingress.yaml 
ingress.networking.k8s.io/dashboard-ingress created

kubectl get ingress -n kubernetes-dashboard
NAME                CLASS    HOSTS           ADDRESS   PORTS   AGE
dashboard-ingress   <none>   dashboard.com             80      3m9s
