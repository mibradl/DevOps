# Chapter10
This module we will look at ConfigMap and Secret Volume Types

# Name: ConfigMap and Secret Volume Types


# Description: 

ConfigMap and Secret as Kubernetes Volumes

    Mount as Volume Types

    volumes:
      - name: mosquitto-config
        configMap:
          name: mosquitto-config-file
      - name: mosquitto-secret
        secret:
          secretName: mosquitto-secret-file


    Create ConfigMap file

    apiVersion: v1
    kind: ConfigMap
    metadata:
      name: mosquitto-config-file
    data:
      mosquitto.conf: |
        log_dest stdout
        log_type all
        log_timestamp true
        listener 9001


When to use ConfigMap and Secret Volumes?

    Configuration Files usages in Pods?

        some.conf       

    prometheus

    elasticsearch

    mosquitto

    nginx

    my-app

        app.properties

        password.properties

        client-cert

    How to pass these config files to Kubernetes Pods

    Many services need configuration files



ConfigMap and Secret for individual values

    Are used for external configuration

    Of individual values


ConfigMap and Secret for mounting files

    Individual key-value pairs

    Create files that can be mounted inside of the Pod and then inside of the container

    So application inside of the container can access it


Demo

    Mosquitto demo - Message Broker

    Overwriting mosquito.conf

    Adding passwords file

    Create config file and secrets file


    Use case 1st Mosquitto Pod without any volumes

    apiVersion: apps/v1
    kind: Deployment
    metadata:
    name: mosquitto
    labels:
        app: mosquitto
    spec:
    replicas: 1
    selector:
        matchLabels:
        app: mosquitto
    template:
        metadata:
        labels:
            app: mosquitto
        spec:
        containers:
            - name: mosquitto
            image: eclipse-mosquitto:2.0
            ports:
                - containerPort: 1883

        Files exist by default

        kubectl exec -it mosquitto-579f5c4464-dslln -- /bin/sh
/ # ls 
bin                     etc                     media                   mosquitto-no-auth.conf  root                    srv                     usr
dev                     home                    mnt                     opt                     run                     sys                     var
docker-entrypoint.sh    lib                     mosquitto               proc                    sbin                    tmp

/ # cd mosquitto/ ; ls
config  data    log
/mosquitto # 

/mosquitto # cd config/
/mosquitto/config # ls -l
total 40
-rw-r--r--    1 mosquitt mosquitt     40449 Jul 22 23:03 mosquitto.conf

        Nothing in the mosquitto.conf fiel except default settings

        kubectl delete -f mosquitto-without-volumes.yaml
        deployment.apps "mosquitto" deleted
        michaelbradley@Michaels-iMac-2.attlocal.net /Users/michaelbradley/k8s-configuration/volumes

2nd Overwrite the mosquitto.conf file

ConfigMap and Secret must be created and exist before Pod starts in the cluster

        kubectl apply -f config-file.yaml 
        configmap/mosquitto-config-file created

        kubectl get secret
        NAME                    TYPE     DATA   AGE
        mongodb-secret          Opaque   2      5d1h
        mosquitto-secret-file   Opaque   1      94s

        kubectl get configmap
        NAME                    DATA   AGE
        kube-root-ca.crt        1      6d
        mongodb-configmap       1      4d14h
        mosquitto-config-file   1      5m2



    In the configuration file Volumes are mounted into the Pod

    Mount Volumes into Container

    mountPath = path in the filesystem inside the container

    path depends on the application

    Abstraction useful for multiple containers


    kubectl apply -f mosquitto.yaml 
    deployment.apps/mosquitto created

    kubectl get pod
    NAME                                  READY   STATUS    RESTARTS      AGE
    mongo-express-6cfbc86cb6-ph22k        1/1     Running   2 (34h ago)   4d14h
    mongodb-deployment-585bb4fddc-fnshv   1/1     Running   2 (34h ago)   4d16h
    mosquitto-76954b5c5f-xj9ff            1/1     Running   0             73s

     kubectl apply -f mosquitto.yaml 


    deployment.apps/mosquitto created
    michaelbradley@Michaels-iMac-2.attlocal.net /Users/michaelbradley/k8s-configuration/volumes 
    % kubectl get pod
    NAME                                  READY   STATUS    RESTARTS      AGE
    mongo-express-6cfbc86cb6-ph22k        1/1     Running   2 (34h ago)   4d14h
    mongodb-deployment-585bb4fddc-fnshv   1/1     Running   2 (34h ago)   4d16h
    mosquitto-76954b5c5f-xj9ff            1/1     Running   0             73s

    michaelbradley@Michaels-iMac-2.attlocal.net /Users/michaelbradley/k8s-configuration/volumes 
    % kubectl exec -it mosquitto-76954b5c5f-xj9ff -- /bin/sh  
    / # ls
    bin                     etc                     media                   mosquitto-no-auth.conf  root                    srv                     usr
    dev                     home                    mnt                     opt                     run                     sys                     var
    docker-entrypoint.sh    lib                     mosquitto               proc                    sbin                    tmp

    
    
    Secret directory and file created

    / # cd mosquitto/
    /mosquitto # ls
    config  data    log     secret
    /mosquitto # cd secret/
    /mosquitto/secret # 

    /mosquitto/secret # ls 
    secret.file

    Contains plain text


Check conf file

    /mosquitto/secret # cd ..
    /mosquitto # ls
    config  data    log     secret
    /mosquitto # cd config/
    /mosquitto/config # ls -l
    total 0
    lrwxrwxrwx    1 root     root            21 Aug 14 04:18 mosquitto.conf -> ..data/mosquitto.conf

    These are now the contents of the conf file

    /mosquitto/config # cat mosquitto.conf 
    log_dest stdout
    log_type all
    log_timestamp true
    listener 9001
    /mosquitto/config #



    Individual key-value pairs

        Usage as env variables

    Create files

        Mount as volume types


    ConfigMap and Secret are Local Volume Types


# Usage

kubectl apply -f mosquitto-without-volumes.yaml 
deployment.apps/mosquitto created


kubectl get pod 
NAME                                  READY   STATUS    RESTARTS      AGE
mongo-express-6cfbc86cb6-ph22k        1/1     Running   2 (33h ago)   4d14h
mongodb-deployment-585bb4fddc-fnshv   1/1     Running   2 (33h ago)   4d15h
mosquitto-579f5c4464-dslln            1/1     Running   0             92s
    

michaelbradley@Michaels-iMac-2.attlocal.net /Users/michaelbradley/k8s-configuration/volumes 
% kubectl exec -it mosquitto-579f5c4464-dslln -- /bin/sh
/ # 


kubectl exec -it mosquitto-579f5c4464-dslln -- /bin/sh
/ # ls 
bin                     etc                     media                   mosquitto-no-auth.conf  root                    srv                     usr
dev                     home                    mnt                     opt                     run                     sys                     var
docker-entrypoint.sh    lib                     mosquitto               proc                    sbin                    tmp
/ # 


kubectl exec -it mosquitto-579f5c4464-dslln -- /bin/sh
/ # ls 
bin                     etc                     media                   mosquitto-no-auth.conf  root                    srv                     usr
dev                     home                    mnt                     opt                     run                     sys                     var
docker-entrypoint.sh    lib                     mosquitto               proc                    sbin                    tmp


/ # cd mosquitto/ ; ls
config  data    log
/mosquitto # 


/mosquitto # cd config/
/mosquitto/config # ls -l
total 40
-rw-r--r--    1 mosquitt mosquitt     40449 Jul 22 23:03 mosquitto.conf



% kubectl apply -f config-file.yaml 
configmap/mosquitto-config-file created

kubectl get secret
NAME                    TYPE     DATA   AGE
mongodb-secret          Opaque   2      5d1h
mosquitto-secret-file   Opaque   1      94s


kubectl get configmap
NAME                    DATA   AGE
kube-root-ca.crt        1      6d
mongodb-configmap       1      4d14h
mosquitto-config-file   1      5m2