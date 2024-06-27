# Chapter7
This module we will demo Docker Volumes.

# Name: Docker Volumes Demo

# Description: 

Docker Volumes in practice

    This is simple Nodejs, MongoDB application that we going to attach the volume to so we don't lose data when the application is restarted.

    Define Named Volume in Docker Compose File

        version: '3'
        services:
        mongodb:
            image: mongo
            ports:
            - 27017:27017
            environment:
            - MONGO_INITDB_ROOT_USERNAME=admin
            - MONGO_INITDB_ROOT_PASSWORD=password
            volumes:
             - mongo-data:/data/db
        mongo-express:
            image: mongo-express
            restart: always
            ports:
            - 8081:8081
            environment:
            - ME_CONFIG_MONGODB_ADMINUSERNAME=admin
            - ME_CONFIG_MONGODB_ADMINPASSWORD=password
            - ME_CONFIG_MONGODB_SERVER=mongodb
            depends_on:
            - "mongodb"
        volumes:
           mongo-data:/data/db 
             driver: local 


        List all the volumes you are using in any of your containers

        1. host volume name

        2. path inside of the container - /data/db is the default location where data is written inside the container

        mongo-data the host name is attaching the path on the containter

        For mysql:/var/lib/mysql

        For postgres:/var/lib/postgres/data

        The path differs for each database!


        Restart Docker Compose to see how it works

            docker-compose down - stops and removes containers

            docker-compose up - Builds, recreates containers


        Attaches volume storage to the newly started container and replicate the data inside the container, and we achieve data persistence inside the             container.


        See where docker volumes are located

            Windows - C:\ProgramData\docker\volumes

            Linux - /var/lib/docker/volumes

            Mac - /var/lib/docker/volumes - each volume has it's own hash

                cfc1ab...ddf22/_data - will contain all the files and data that is persisted
                

        To access the shell of the Docker VM to view volume information

            docker run -it --privileged --pid=host debian nsenter -t 1 -m -u -n -i sh


        Inside the container - /data/db

# Usage

% docker-compose -f docker-compose.yaml down
WARN[0000] /Users/michaelbradley/new-project/js-app/docker-compose.yaml: `version` is obsolete 
[+] Running 3/3
 ✔ Container js-app-mongo-express-1  Removed                                                                                                                              0.7s 
 ✔ Container js-app-mongodb-1        Removed                                                                                                                              0.6s 
 ✔ Network js-app_default            Removed 
    

% docker-compose -f docker-compose.yaml up  
WARN[0000] /Users/michaelbradley/new-project/js-app/docker-compose.yaml: `version` is obsolete 
[+] Running 3/3
 ✔ Network js-app_default            Created                                                                                                                              0.1s 
 ✔ Container js-app-mongodb-1        Created                                                                                                                              0.1s 
 ✔ Container js-app-mongo-express-1  Created                                                                                                                              0.1s 
Attaching to mongo-express-1, mongodb-1
mongo-express-1  | Waiting for mongo:27017...
mongodb-1        | about to fork child process, waiting until server is ready for connections.

mongo-express-1  | Welcome to mongo-express 1.0.2
mongo-express-1  | ------------------------
mongo-express-1  | 
mongo-express-1  | 
mongodb-1        | {"t":{"$date":"2024-06-26T03:32:43.618+00:00"},"s":"I",  "c":"NETWORK",  "id":22943,   "ctx":"listener","msg":"Connection accepted","attr":{"remote":"172.24.0.3:38422","uuid":{"uuid":{"$uuid":"760ca0bb-b58e-47f2-8631-61de2c8dd065"}},"connectionId":1,"connectionCount":1}}
mongodb-1        | {"t":{"$date":"2024-06-26T03:32:43.625+00:00"},"s":"I",  "c":"NETWORK",  "id":51800,   "ctx":"conn1","msg":"client metadata","attr":{"remote":"172.24.0.3:38422","client":"conn1","negotiatedCompressors":[],"doc":{"driver":{"name":"nodejs","version":"4.13.0"},"os":{"type":"Linux","name":"linux","architecture":"x64","version":"6.6.31-linuxkit"},"platform":"Node.js v18.20.3, LE (unified)|Node.js v18.20.3, LE (unified)"}}}
mongodb-1        | {"t":{"$date":"2024-06-26T03:32:43.647+00:00"},"s":"I",  "c":"NETWORK",  "id":22943,   "ctx":"listener","msg":"Connection accepted","attr":{"remote":"172.24.0.3:38434","uuid":{"uuid":{"$uuid":"1480d61e-9705-4093-b35c-553e06c7690a"}},"connectionId":2,"connectionCount":2}}
mongodb-1        | {"t":{"$date":"2024-06-26T03:32:43.651+00:00"},"s":"I",  "c":"NETWORK",  "id":51800,   "ctx":"conn2","msg":"client metadata","attr":{"remote":"172.24.0.3:38434","client":"conn2","negotiatedCompressors":[],"doc":{"driver":{"name":"nodejs","version":"4.13.0"},"os":{"type":"Linux","name":"linux","architecture":"x64","version":"6.6.31-linuxkit"},"platform":"Node.js v18.20.3, LE (unified)|Node.js v18.20.3, LE (unified)"}}}
mongodb-1        | {"t":{"$date":"2024-06-26T03:32:43.665+00:00"},"s":"I",  "c":"ACCESS",   "id":6788604, "ctx":"conn2","msg":"Auth metrics report","attr":{"metric":"acquireUser","micros":0}}
mongodb-1        | {"t":{"$date":"2024-06-26T03:32:43.684+00:00"},"s":"I",  "c":"ACCESS",   "id":5286306, "ctx":"conn2","msg":"Successfully authenticated","attr":{"client":"172.24.0.3:38434","isSpeculative":true,"isClusterMember":false,"mechanism":"SCRAM-SHA-256","user":"admin","db":"admin","result":0,"metrics":{"conversation_duration":{"micros":19682,"summary":{"0":{"step":1,"step_total":2,"duration_micros":1226},"1":{"step":2,"step_total":2,"duration_micros":36}}}},"extraInfo":{}}}
mongodb-1        | {"t":{"$date":"2024-06-26T03:32:43.686+00:00"},"s":"I",  "c":"NETWORK",  "id":6788700, "ctx":"conn2","msg":"Received first command on ingress connection since session start or auth handshake","attr":{"elapsedMillis":3}}
mongo-express-1  | Mongo Express server listening at http://0.0.0.0:8081
mongo-express-1  | Server is open to allow connections from anyone (0.0.0.0)
mongo-express-1  | basicAuth credentials are "admin:pass", it is recommended you change this in your config.js!
mongodb-1        | {"t":{"$date":"2024-06-26T03:32:54.130+00:00"},"s":"I",  "c":"NETWORK",  "id":22943,   "ctx":"listener","msg":"Connection accepted","attr":{"remote":"172.24.0.3:52528","uuid":{"uuid":{"$uuid":"b0b02e05-257f-4373-94ba-6d0b3215929f"}},"connectionId":3,"connectionCount":3}}


michaelbradley@Michaels-iMac-2 js-app % docker-compose -f docker-compose.yaml down
WARN[0000] /Users/michaelbradley/new-project/js-app/docker-compose.yaml: `version` is obsolete 
[+] Running 3/2
 ✔ Container js-app-mongo-express-1  Removed                                                                                                            0.2s 
 ✔ Container js-app-mongodb-1        Removed                                                                                                            0.2s 
 ✔ Network js-app_default            Removed  


% docker run -it --privileged --pid=host debian nsenter -t 1 -m -u -n -i sh
Unable to find image 'debian:latest' locally
latest: Pulling from library/debian
fea1432adf09: Pull complete 
Digest: sha256:a92ed51e0996d8e9de041ca05ce623d2c491444df6a535a566dabd5cb8336946
Status: Downloaded newer image for debian:latest
/ # 


 docker run -it --privileged --pid=host debian nsenter -t 1 -m -u -n -i sh
Unable to find image 'debian:latest' locally
latest: Pulling from library/debian
fea1432adf09: Pull complete 
Digest: sha256:a92ed51e0996d8e9de041ca05ce623d2c491444df6a535a566dabd5cb8336946
Status: Downloaded newer image for debian:latest
/ # ls
bin             containers      home            init            parent-distro   run             src             usr
bpf-legacy.o    dev             host-network.o  lib             proc            sbin            sys             var
bpf.o           etc             host_mnt        mnt             root            services        tmp
/ # ls /var/lib/docker
buildkit    containers  engine-id   image       network     overlay2    plugins     runtimes    stats       swarm       tmp         volumes
/ # ls /var/lib/docker/volumes/
05775f90b5bbc57c26b6dd4dff00fc4c75a699e9d92d82045d7862f15ed804ab  90b4bc6913529761054f02115781371440804cd37215b7144656385ed8d18982
10867b55819b3c1a29f96332e8308c272cdf9471c0fdda266b0cf31190856e3f  90d910a79f83ee2d5f3271c8dae014954e3e5f7368b477b933059b80e51319a4
1a65d802fb681d9d1db45afb25f02b0fe2a42b5a53385fad1d5ce3851f84086f  9daab6761db6251f7c70d72ad1bcdfdc718c73381fbef7c9d3ab8aded687a9d0
1fc0d7953dc89ee2dd50605638e3e4bf13d186b052e8a8dc4c4f1653310bc087  a07427ce0d1a53aa029623653ec1e1f404eac38e7d9109d77acaeeacbceb8a20
26ad3d8e1d51416735cd7bddb55b800f8ea2d21828e01f55574af39af5faa8ae  a118f4ada79d0828b466b0e33bafad2ff652526ba59251fa915094dcb06b86f1
309a1ad5bc7e103fd72545bcd70140484cd56d091d86581864ce1fb926e1bb62  aa12685cbfd2cbe17eaca2f2b9dd5706eaac4866623a3f360d2923c67fce7d06
32441f95d08ce4d2c8c1c39ad2dea1b4d38f0fdd178dc12f53773ee380b17b91  acf1b5c25c7924dc46f23c48878192b369d4ff8818061ae9a14e225cdb497189
328ac7c27ec6b504ceb6c323d1f542eca862120aed93af1ee8187bb9b6e8ddb6  b08d046b5d8994fba0a83456f673716fe1004b69a51967149e4d062d3721acd3
38ffea3366759505d144c69a376cb5edfe05f002a8c593a32e2625975ada4257  b30897b1029563338cac88df672fce7b392399ac2b8ff4d9838cfcebd9b04602
461ccb408c7a047a1839b2f2c0aaa0d391fd50c9e4df52fd2fe1bdfb27f48cc6  b5dbd170fb3b4f8fc6e31d7acf92fd8411770f2fe12d2a3915ab1048ed4245b3
47161fe7807f18d781d99833ab71f3828ada1b9469740345f6ae9cc3be1f81f7  b8bb287786342aa6a36c9bded7e7e7ea0b87c03671c10b46f3ce4108cb8d18a1
488fe40b75e28d71a3ebff59c1573be1c614d75040802886fcfb698b0d74d0fa  backingFsBlockDev
4e39a2a0cb93978391ac3e84ee772b6db8c8f4f9c5d5635b2bd59469a95f52c8  bb92872ada37e6da76272043389bc86330dbf981bb8c09398acfa7b01c1594cc
56216a0c87d634c580820ba6239529ba517ca7e61ef8bbf6dd40f43a5a496b2f  c3488b8883772e8f8135bf2752b5e9d8371157075bfcf47962c68f7a9ae0f970
56f0e03ba8201f016c1f016f632b389062f6a63c55886f27d4b6724ad898c0fc  c388264e170a19544a952d1b748c608afa332a1aefae7102c8ab1cc71799bc13
571fe9726cf04c111320fde06b62da8ff2aaa87a71006cbe56ccf8c2d815ff6e  ce6456e01b3b4d65b7cfe7769f586dbc5e548ec0623db2a1df43d9577eac5f93
58538dbb146da66a9a84c5d57c2ef7c8213f67fa2b2f0b790cd1288f606f1965  d6bba40cba115b307eae8dac06a5dd3b0adb4a916a8eefee947484961e272f5a
589c40587fdf01358b09ae7ad0f6c0658ef061f5013a59a77e484c53f171485b  ddd5c2075dbf0e988d2714e53931229de43f33caf6657a9dbb88eed2296f482b <-"anonymous"
58ca7411c881c746f2abca878f63b051ba149518142bc6df96a6ae4538b72493  deac8437b8a0aa4dc93a6f5fc021d1a6a6bca7cef11c683c8686e2bd3f21b075
59ef6881ef52365422dd3f30f550a881ff8048f7f7ce89e8e42d16c2757b8837  e363310d85df99c15af8923a02bf7df1f8d36343d2f6b044e0c8c63b6da8e9b9
5b45c878b93f3697c956db857b1fa6c66ffbb01985c24a4a7eb5adb868c39be9  e69096e42ecaad9c68d9c9ffd964d2117c820e04337332a24d41f094a383d469
6530321b840b18abea7d7df53ed4df65fe1aa8db14b15f80172e072269cd998c  f23a33def30ee9d749e07d1e892d1664a39f1b661a20a30e3f0928c9e398caf1
76d8b7acf9ae9da40fb7212045994556e2bc64cf5afb44a0a482a10bf7d78295  f6867497ebdad80afb8470762596b4120983cf0a57a2db494961456f3ff3f4c6
8382c52f3c55ecdb9d0e6046d00a17d943797719e51a2319b6be40be6714f343  feafa6792af9c457d8f32e32689fb216aa6cd1002eccebdd6659ceb886699afc
8766b0a31a99b4c93ea5b019e17eb64702a38b72177e40103b4d69c2d49954fe  js-app_mongo-data   <---- This is "named volume"
8f75ddb7344538138a4461e04223212d3bdcce410ecf77c2d0a1d39de615fc4c  metadata.db
/ # ls /var/lib/docker/volumes/js-app_mongo-data/
_data
/ # ls /var/lib/docker/volumes/js-app_mongo-data/_data/
WiredTiger                            collection-0--3591392946069804031.wt  index-1--3591392946069804031.wt       index-9--3591392946069804031.wt
WiredTiger.lock                       collection-0-1224673895080427945.wt   index-1-1224673895080427945.wt        journal
WiredTiger.turtle                     collection-2--3591392946069804031.wt  index-3--3591392946069804031.wt       mongod.lock
WiredTiger.wt                         collection-4--3591392946069804031.wt  index-5--3591392946069804031.wt       sizeStorer.wt
WiredTigerHS.wt                       collection-7--3591392946069804031.wt  index-6--3591392946069804031.wt       storage.bson
_mdb_catalog.wt                       diagnostic.data                       index-8--3591392946069804031.wt


michaelbradley@Michaels-iMac-2 app % docker ps
CONTAINER ID   IMAGE            COMMAND                  CREATED          STATUS         PORTS                      NAMES
751134a7cb2d   debian           "nsenter -t 1 -m -u …"   10 minutes ago   Up 9 minutes                              inspiring_colden
fd709f482542   mongo-express    "/sbin/tini -- /dock…"   6 hours ago      Up 6 hours     0.0.0.0:8081->8081/tcp     js-app-mongo-express-1
f407769290fc   mongo            "docker-entrypoint.s…"   6 hours ago      Up 6 hours     0.0.0.0:27017->27017/tcp   js-app-mongodb-1
52eb3407d623   redis            "docker-entrypoint.s…"   4 days ago       Up 4 days      0.0.0.0:6000->6379/tcp     redis-latest
12e7e7fbe6db   redis:6.2        "docker-entrypoint.s…"   4 days ago       Up 4 days      0.0.0.0:6001->6379/tcp     redis-older
350f92754f59   postgres:15.7    "docker-entrypoint.s…"   6 days ago       Up 6 days      5432/tcp                   second-postgres
3b3b25fb8d76   postgres:13.15   "docker-entrypoint.s…"   6 days ago       Up 6 days      5432/tcp                   some-postgres

michaelbradley@Michaels-iMac-2 app % docker exec -it js-app-mongodb-1 sh

Same data shown inside the mongodb container

# ls /data/db
WiredTiger	   WiredTigerHS.wt			 collection-2--3591392946069804031.wt  index-1--3591392946069804031.wt	index-6--3591392946069804031.wt  mongod.lock
WiredTiger.lock    _mdb_catalog.wt			 collection-4--3591392946069804031.wt  index-1-1224673895080427945.wt	index-8--3591392946069804031.wt  sizeStorer.wt
WiredTiger.turtle  collection-0--3591392946069804031.wt  collection-7--3591392946069804031.wt  index-3--3591392946069804031.wt	index-9--3591392946069804031.wt  storage.bson
WiredTiger.wt	   collection-0-1224673895080427945.wt	 diagnostic.data
