# Chapter7
This module we will demo docker compose.

# Name: Docker-Compose - Run Multiple Docker Containers

# Description: 
Running multiple services with Docker Compose


In the previous module we used commands to create the mongodb:

    Create Docker network

    Pulled and started mongodb, using the docker run, port, with env variables, network, name mongo:latest (default)

    Pulled and started mongo-express, using the docker run, port, with env variables, network, name mongo-express:latest (default)


Docker Compose

    Docker run command                                                      mongo-docker-compose.yaml

        docker run -d \                                                     docker-compose

        -name mongodb \                                                     Abstracts away the low-level Docker commands

        -p 27017:27017 \                                                    Provides a higher-level, more human-readable configuration format

        -e MONGO-INITDB_ROOT_USERNAME=admin \

        -e MONGO_INITDB_ROOT_PASSWORD=password \

        -net mongo-network \

        mongo




        docker run -d \                                                     
                                                                            
        -name mongo-express \                                                         
                                                                                     
        -p 8081:8081 \                                                            
                                                                                        
        -e ME_CONFIG_MONGODB_ADMINUSERNAME=admin \                                       
                                                                                        
        -e ME_CONFIG_MONGODB_ADMINPASSWORD=password \ 

        -e ME_CONFIG_MONGODB_SERVER=mongodb \                                           

        -net mongo-network \

        mongo-express



        mongo.yaml

        version:'3'         = version of docker-compose
        services:
           mongodb:        = maps to the container name
            image: mongo    = maps to image on last line
            ports:
            - 27017:27017       HOST:CONTAINTER
            environment:
            - MONGO_INITDB_ROOT_USERNAME=admin
            - MONGO_INITDB_ROOT_PASSWORD=password
           mongo-express:
            image: mongo-express
            ports:
            - 8081:8081
            environment:
            - ME_CONFIG_MONGODB_ADMINUSERNAME=admin
            - ME_CONFIG_MONGODB_ADMINPASSWORD=password
            - ME_CONFIG_MONGODB_SERVER=mongodb

Helps to structure your commands

Simplifies container management

Containers, networks, volumes and configuration using a YAML file

Declarative approach: defining the desired state

Network configuration not required

Communication will happen via container name

By default, Compose sets up a single network for your app

Option to specify your own networks with the top-level "networks" key



Service Orchestration with Docker Compose

    Specify dependencies and relationships between services to ensure proper communication and required services are started in the correct order

    restart always - Always restart the container if it stops or fails

        make sure MongoExpress can connect to MongoDB container

        If the MongoDB is not start when MongoExpress comes up it won't be able to connect to the DB, thus the restart

    restart policy

    depends_on

        Expresses startup and shutdown dependencies between services

    healthcheck



    Docker Compose is already installed with Docker

    docker-compose -f mongo.yaml up

    mongo-express-1  | Mongo Express server listening at http://0.0.0.0:8081
    mongo-express-1  | Server is open to allow connections from anyone (0.0.0.0)


Data Persistence with Containers

    Data is lost on recreation

    Docker volumes - enables persisting data generated by and used by Docker containers


# Usage

michaelbradley@Michaels-iMac-2 app % docker-compose -f mongo.yaml up
WARN[0000] /Users/michaelbradley/new-project/js-app/app/mongo.yaml: `version` is obsolete 
[+] Running 3/3
 ✔ Network app_default            Created                                                                                                                                                                0.1s 
 ✔ Container app-mongodb-1        Created                                                                                                                                                                0.1s 
 ✔ Container app-mongo-express-1  Created                                                                                                                                                                0.1s 
Attaching to mongo-express-1, mongodb-1
mongo-express-1  | Waiting for mongo:27017...

mongo-express-1  | Welcome to mongo-express 1.0.2
mongo-express-1  | ------------------------
mongo-express-1  | 
mongo-express-1  | 
mongodb-1        | {"t":{"$date":"2024-06-23T09:18:19.365+00:00"},"s":"I",  "c":"NETWORK",  "id":22943,   "ctx":"listener","msg":"Connection accepted","attr":{"remote":"172.22.0.3:53888","uuid":{"uuid":{"$uuid":"e375daad-63c4-4a53-ac50-35b3099bb604"}},"connectionId":1,"connectionCount":1}}
mongodb-1        | {"t":{"$date":"2024-06-23T09:18:19.371+00:00"},"s":"I",  "c":"NETWORK",  "id":51800,   "ctx":"conn1","msg":"client metadata","attr":{"remote":"172.22.0.3:53888","client":"conn1","negotiatedCompressors":[],"doc":{"driver":{"name":"nodejs","version":"4.13.0"},"os":{"type":"Linux","name":"linux","architecture":"x64","version":"6.6.31-linuxkit"},"platform":"Node.js v18.20.3, LE (unified)|Node.js v18.20.3, LE (unified)"}}}
mongodb-1        | {"t":{"$date":"2024-06-23T09:18:19.385+00:00"},"s":"I",  "c":"NETWORK",  "id":22943,   "ctx":"listener","msg":"Connection accepted","attr":{"remote":"172.22.0.3:53890","uuid":{"uuid":{"$uuid":"b014a617-cdd7-4c46-ab72-2d76fcf3eec9"}},"connectionId":2,"connectionCount":2}}
mongodb-1        | {"t":{"$date":"2024-06-23T09:18:19.387+00:00"},"s":"I",  "c":"NETWORK",  "id":51800,   "ctx":"conn2","msg":"client metadata","attr":{"remote":"172.22.0.3:53890","client":"conn2","negotiatedCompressors":[],"doc":{"driver":{"name":"nodejs","version":"4.13.0"},"os":{"type":"Linux","name":"linux","architecture":"x64","version":"6.6.31-linuxkit"},"platform":"Node.js v18.20.3, LE (unified)|Node.js v18.20.3, LE (unified)"}}}
mongodb-1        | {"t":{"$date":"2024-06-23T09:18:19.394+00:00"},"s":"I",  "c":"ACCESS",   "id":6788604, "ctx":"conn2","msg":"Auth metrics report","attr":{"metric":"acquireUser","micros":0}}
mongodb-1        | {"t":{"$date":"2024-06-23T09:18:19.409+00:00"},"s":"I",  "c":"ACCESS",   "id":5286306, "ctx":"conn2","msg":"Successfully authenticated","attr":{"client":"172.22.0.3:53890","isSpeculative":true,"isClusterMember":false,"mechanism":"SCRAM-SHA-256","user":"admin","db":"admin","result":0,"metrics":{"conversation_duration":{"micros":15925,"summary":{"0":{"step":1,"step_total":2,"duration_micros":676},"1":{"step":2,"step_total":2,"duration_micros":52}}}},"extraInfo":{}}}
mongodb-1        | {"t":{"$date":"2024-06-23T09:18:19.411+00:00"},"s":"I",  "c":"NETWORK",  "id":6788700, "ctx":"conn2","msg":"Received first command on ingress connection since session start or auth handshake","attr":{"elapsedMillis":2}}
mongo-express-1  | Mongo Express server listening at http://0.0.0.0:8081
mongo-express-1  | Server is open to allow connections from anyone (0.0.0.0)
mongo-express-1  | basicAuth credentials are "admin:pass", it is recommended you change this in your config.js!


michaelbradley@Michaels-iMac-2 app % node server.js
(node:89647) [DEP0040] DeprecationWarning: The `punycode` module is deprecated. Please use a userland alternative instead.
(Use `node --trace-deprecation ...` to show where the warning was created)
app listening on port 3000!


% docker-compose -f mongo.yaml down
WARN[0000] /Users/michaelbradley/new-project/js-app/app/mongo.yaml: `version` is obsolete 
[+] Running 3/2
 ✔ Container app-mongodb-1        Removed                                                                                           0.6s 
 ✔ Container app-mongo-express-1  Removed                                                                                           0.6s 
 ✔ Network app_default            Removed                                                                                           0.1s 
michaelbradley@Michaels-iMac-2.local /Users/michaelbradley/new-project/js-app/app [master]
% docker network ls
NETWORK ID     NAME            DRIVER    SCOPE
a03ce6e24737   bridge          bridge    local
80e6110b908e   host            host      local
37019f61ee4e   mongo-network   bridge    local
8fc3b5c03502   none            null      local
