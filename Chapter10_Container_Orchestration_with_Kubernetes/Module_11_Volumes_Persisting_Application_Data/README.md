# Chapter10
This module we will look at persisting data with volumes

# Name: Volumes - Persisting Application Data

# Description: 

Kubernetes Volumes

    How to persist data in Kubernetes using volumes?

        Persistent Volume

        Persistent Volume Claim

        Storage Class


    The Need for Volumes

    my-app      updated     mysql       database

    When the application is restarted the data is loss, because there is no persistence out of the box 

    You need to configure that

Storage Requirements

    Storage that doesn't depend on the pod lifecycle

    Storage must be available on all nodes

    Storage needs to survive even if cluster crashes


Another Use Case for persistent storage


    Reads / Writes from a directory



Persistent Volume

    A cluster resource

    Create via YAML file

    kind:   PersistentVolume

    spec:   e.g. how much storage?


    Needs actual physical storage

        local disk

        nfs server

        cloud storage


    Where does this storage come from and who makes it available to the cluster?

        
        What type of storage do you need?

        You need to create and manage them by yourself (backups, etc)

            external plugin to your cluster

        You can have multiple storage or all configured for your cluster



    Persistent Volume YAML Example


              apiVersion: v1
              kind: PersistentVolume
              metadata:
                name: pv-name
              spec:
                capacity:
                  storage: 5Gi
                volumeMode: Filesystem
                accessModes:
                  - ReadWriteOnce
                persistentVolumeReclaimPolicy: Recycle
                storageClassName: slow
                mountOptions:
                  - hard
                  - nfsers=4.0
                nfs:
                  /path: /dir/path/on/nfs/server
                  nfs-server-ip-address
                
        Use physical storages in spec section

        How much: volumeMode

        Additional params, like access:

        Nfs parameters

    

    Google Cloud 

              apiVersion: v1
              kind: PersistentVolume
              metadata:
                name: test-volume
                labels:
                  topology.kubernetes.io/zone: us-central1-a_us-central1-b
              spec:
                capacity:
                  storage: 400Gi
                volumeMode: Filesystem
                accessModes:
                  - ReadWriteOnce
                gcePersistentDisk:
                  pdName: my-data-disk
                  fsType: ext4
                
        How much: storage

        Google Cloud parameters:




        Local Storage

              apiVersion: v1
              kind: PersistentVolume
              metadata:
                name: pv-name
              spec:
                capacity:
                  storage: 100Gi
                volumeMode: Filesystem
                accessModes:
                  - ReadWriteOnce
                persistentVolumeReclaimPolicy: Delete
                storageClassName: slow
                local:
                  path: /mnt/disks/ssd1
                nodeAffinity:
                  required:
                    noSelectorTerms:
                    - matchExpressions:
                      - key: kubernetes.io/hostname
                        operator: In
                        values:
                        - example-node

            Node Affinity:


            Complete list of storage backends supported by K8s:

                Types of Volumes


        
        Persistent Volumes are NOT namespaced

            PV outside of the namespaces

            Accessible to the whole cluster



    Local vs Remote Volume Types

        Each volume type has its own use case!

        Local volume types violate 2) and 3) requirement for data persistence

            Being tied to 1 specific node

            Surviving cluster crashes

        For DB persistence use remote storage


    Who creates Persistence Volume and when?

        Persistence Volumes are resources that need to be there BEFORE

        The Pod depends on the PV being created


        Two main roles in Kubernetes:

            K8s Admin sets up and maintains the cluster and has enough resources

                Configure Storage resource is provisioned by Admin

                Nfs storage, Cloud storage

                Creates the PV components from these storage backends


            K8s User deploys application in Cluster (directly or CICD pipeline)


            Once the Admin configures storage the developer can use storage, by explicitly configure the application and configuring the YAML file

                Application has to claim the Persistence Volume

                This is done using the Persistence Volume Claim, using YAML configuration


                    apiVersion: v1
                    kind: PersistentVolumeClaim
                    metadata:
                        name: pvc-name                                  pvc-name references the volumes/claimsName in Pod
                    spec:
                      storageClassName: manual
                      volumeMode: Filesystem
                      accessModes:
                        - ReadWriteOnce
                      resources:
                        requests:
                          storage: 10GI

                 PVC claims storage: 10GI, ReadWriteOnce  

                5GI         10GI        8GI

                Use that PVC in Pods configuration

                    apiVersion: v1
                    kind: Pod
                    metadata:
                        name: mypod
                    spec:
                      containers:
                        - name: myfrontend
                          image: nginx
                          volumeMounts:
                          - mountPath: "/var/www/html"
                            name: mypod
                    volumes:
                      - name: mypod
                        persistentVolumeClaim:  
                          claimName: pvc-name
                        

                Now the Pod and all of the containers within the Pod will have access to the Persistence Volume Claim


        Level of Volume Abstraction

                Pods requests the volume thru the PV claim

                Claim tries to find a volume in cluster

                Volume has the actual storage backend

                Claims must exist in the same namespace!

                Volume is mounted into the Pod

                Volume is mounted into the container

                The container can read/write to storage

                If the Pod dies, it is replaced and still has access to storage because of PVC.


                spec:
                  containers:
                    - name: myfrontend
                      image: nginx
                      volumeMounts:
                      - mountPath: "/var/www/html"
                        name: mypod
                  volumes:
                    - name: mypod
                      persistentVolumeClaim:  
                        claimName: pvc-name
                        

        Why so many abstractions?

                Admin provisions the PC storage resource

                User creates claim to PV

                Can't use one component and configure it for all


                As a developer you don't care how or where the Storage is created, as long as its there

                Same as directory, as long as there is sufficient space

                Easier for deploying applications for developers



        ConfigMap and Secret

                Two volume types that need to be mentioned separately

                ConfigMap and Secret

                Both are Local volumes

                Not created via PV and PVC

                Managed by Kubernetes


        Consider a case where you have a Configuration file for you Prometheus Pod

        Or a Certificate file for your Pod

        How this works?

            1) Create ConfigMap and/or Secret component

            2) Mount that into your Pod/container

            apiVersion: v1
            kind: Pod
            metadata:
              name:mypod
            spec:
              containers:
                - name: busybox-containers
                  image: busybox
                  volumeMounts:
                    - name: config-dir
                      mountPath: "/etc/config"
              volumes:
                - name: config-dir
                  configMap:  
                    name: bb-configmap


        What we've covered

            Volume is a directory with some data

            These volumes are accessible in containers in a Pod

            How made available, backed by which storage medium - defined by specific volume types

            

            Pod specifies what volumes to provide

            Where to mount in the containers section?

            Apps can access the mounted data here: /var/www/html


        Different volume types

            elastic-app

            secret

            configm

            pvc - awsElasticBlockStore

            spec:
              containers:
                - name: elastic:latest
                  image: elastic-container
                  ports:
                  - containerPort:9200
                  volumeMounts:
                    - name: es-persistent-storage
                      mountPath: "/var/lib/-storage"
                    - name: es-secret-dir
                      mountPath: /var/lib/secret
                    - name: es-config-dir
                      mountPath: /var/lib/config
              volumes:
                - name: es-persistent-storage
                  persistentVolumeClaim:
                    claimName: es-pv-claim  
                - name: es-secret-dir
                  secret:
                    secretName: es-secret
                - name: es-config-dir
                  configMap:

        Storage Class

            1) Admins configure storage for the storage

            2) Create Persistent Volumes

            3) K8s Users claim PV using PVC

        Admins may have to request storage (hundreds of PV) manually. 

            Storage class provisions Persistent Volumes dynamically

            Whenever PVC claims it

            apiVersion: storage.k8.io/v1
            kind: Storage Class                     kind: Storge Class - create PV dynamically in the background
            metadata:
              name: storage-class-name
            provisioner: kubernetes.io/aws-ebs
            parameters:
              type: io1
              iopsPerGB: "10"
              fsType: ext4


        StorageBackend id defined in the SC component

            Via "provisioner" attribute

            Each storage backend has own provisioner

            Internal provisioner - "kubernetes.io"

            External provisioner

            Configure parameters for storage we want to request for PV

            Abtracts underlying storage provider

            Parameters for that storage

        Storage Class usage

            Requested by PersistentVolumeClaim


            apiVersion: v1
            kind: PersistentVolume
            metadata:
              accessModes:
              - ReadWriteOnce
              resources:
                requests:
                  storage: 100Gi
              storageClassName: storage-class-name
            

            1) Pod claims storage via PVC

            2) PVC requests storage from Storage Class

            3) SC creates PV that meets the needs of the Claim, using provisioner backend








































# Usage


    