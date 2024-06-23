# Chapter7
This module we explore a demo project - Developing with Docker Containers.

# Name: Developing with Docker

# Description: 

Developing with Docker Containers

    JS and Nodejs application

    Mongodb Docker container

Docker in Software Development

    Develop UI backend using JavaScript with simple html and NodeJs on the backend

        localhost:3000/my-app

    To integrate this we will use a docker container with a MongoDB

    To simplify updates to the MongoDB we will create a MongoExpress UI that interfaces with the MongoDB.

        localhost:8081/db/my-db


Let's review how this all works

    MongeDB and MongoExpress - download latest images from docker hub

        DevOps % docker pull mongo
        Using default tag: latest
        latest: Pulling from library/mongo
        7646c8da3324: Pull complete 
        11f24d004130: Pull complete 
        1dfef8821fc6: Pull complete 
        bd2b135d9110: Pull complete 
        76154bf34598: Pull complete 
        a93134489020: Pull complete 
        fd1fa78ffcfb: Pull complete 
        d91b3273507c: Pull complete 
        Digest: sha256:bd38dc3d2895c7434b9b75c86525642efe3d65e4c6aadfe397486d7cc89406f0
        Status: Downloaded newer image for mongo:latest
        docker.io/library/mongo:latest


        docker pull mongo-express
        Using default tag: latest
        latest: Pulling from library/mongo-express
        619be1103602: Pull complete 
        7e9a007eb24b: Pull complete 
        5189255e31c8: Pull complete 
        88f4f8a6bc8d: Pull complete 
        d8305ae32c95: Pull complete 
        45b24ec126f9: Pull complete 
        9f7f59574f7d: Pull complete 
        0bf3571b6cd7: Pull complete 
        Digest: sha256:1b23d7976f0210dbec74045c209e52fbb26d29b2e873d6c6fa3d3f0ae32c2a64
        Status: Downloaded newer image for mongo-express:latest
        docker.io/library/mongo-express:latest


        michaelbradley@Michaels-iMac-2 DevOps % docker images| grep mongo
        REPOSITORY      TAG       IMAGE ID       CREATED        SIZE
        mongo           latest    95b3ba6bed35   4 weeks ago    797MB
        mongo-express   latest    870141b735e7   3 months ago   182MB


    Docker Setup

    Docker Network - Docker creates it's own isolated docker network and containers can communicate over this network. Other apps outside this network must communicate over TCP using IP/DNS with a opened port.

        localhost:27017

    In this network we will configure a backend NodeJS application, a MongoDB and a Mongo Express UI

    The browser which is outside the docker network will connect us IP address and port


    Creating a docker network

        michaelbradley@Michaels-iMac-2 DevOps % docker network create mongo-network

    
        michaelbradley@Michaels-iMac-2 DevOps % docker network ls
        NETWORK ID     NAME            DRIVER    SCOPE
        a03ce6e24737   bridge          bridge    local
        80e6110b908e   host            host      local
        37019f61ee4e   mongo-network   bridge    local
        8fc3b5c03502   none            null      local


Run Mongo containers

        michaelbradley@Michaels-iMac-2 DevOps % docker run -d \
        > -p 27017:27017 \
        > -e MONGO_INITDB_ROOT_USERNAME=admin \
        > -e MONGO_INITDB_ROOT_PASSWORD=password \
        > --name mongodb \
        > --net mongo-network \
        > mongo
        cf16b7a7b8d521fbf1fc157751f62cb7acb4ca336f19b9ee2756764f6ee0c781


        michaelbradley@Michaels-iMac-2 DevOps % docker logs mongodb |grep 27017
        {"t":{"$date":"2024-06-22T20:34:11.418+00:00"},"s":"I",  "c":"CONTROL",  "id":4615611, "ctx":"initandlisten","msg":"MongoDB starting","attr":{"pid":27,"port":27017,"dbPath":"/data/db","architecture":"64-bit","host":"cf16b7a7b8d5"}}
        {"t":{"$date":"2024-06-22T20:34:11.418+00:00"},"s":"I",  "c":"CONTROL",  "id":21951,   "ctx":"initandlisten","msg":"Options set by command line","attr":{"options":{"net":{"bindIp":"127.0.0.1","port":27017,"tls":{"mode":"disabled"}},"processManagement":{"fork":true,"pidFilePath":"/tmp/docker-entrypoint-temp-mongod.pid"},"systemLog":{"destination":"file","logAppend":true,"path":"/proc/1/fd/1"}}}}
        {"t":{"$date":"2024-06-22T20:34:11.978+00:00"},"s":"I",  "c":"NETWORK",  "id":23015,   "ctx":"listener","msg":"Listening on","attr":{"address":"/tmp/mongodb-27017.sock"}}
        {"t":{"$date":"2024-06-22T20:34:11.979+00:00"},"s":"I",  "c":"NETWORK",  "id":23016,   "ctx":"listener","msg":"Waiting for connections","attr":{"port":27017,"ssl":"off"}}
        {"t":{"$date":"2024-06-22T20:34:13.300+00:00"},"s":"I",  "c":"NETWORK",  "id":23017,   "ctx":"listener","msg":"removing socket file","attr":{"path":"/tmp/mongodb-27017.sock"}}
        {"t":{"$date":"2024-06-22T20:34:14.326+00:00"},"s":"I",  "c":"CONTROL",  "id":4615611, "ctx":"initandlisten","msg":"MongoDB starting","attr":{"pid":1,"port":27017,"dbPath":"/data/db","architecture":"64-bit","host":"cf16b7a7b8d5"}}
        {"t":{"$date":"2024-06-22T20:34:14.905+00:00"},"s":"I",  "c":"NETWORK",  "id":23015,   "ctx":"listener","msg":"Listening on","attr":{"address":"/tmp/mongodb-27017.sock"}}
        {"t":{"$date":"2024-06-22T20:34:14.905+00:00"},"s":"I",  "c":"NETWORK",  "id":23016,   "ctx":"listener","msg":"Waiting for connections","attr":{"port":27017,"ssl":"off"}}

        michaelbradley@Michaels-iMac-2 DevOps % docker run -d \        
        -p 8081:8081 \     
        -e ME_CONFIG_MONGODB_ADMINUSERNAME=admin \   
        -e ME_CONFIG_MONGODB_ADMINPASSWORD=password \
        --net mongo-network \
        --name mongo-express \
        -e ME_CONFIG_MONGODB_SERVER=mongodb \
        mongo-express
        fd95b07d09c1a60f7d681d25917a542efd33bfdf74cb4009d57a542982c90820

        michaelbradley@Michaels-iMac-2 DevOps % docker logs mongo-express
        Waiting for mongo:27017...

        Welcome to mongo-express 1.0.2
        ------------------------


        Mongo Express server listening at http://0.0.0.0:8081

        % cd app 
            michaelbradley@Michaels-iMac-2.local /Users/michaelbradley/new-project/js-app/app [master]
            % ls
            images			index.html		package-lock.json	package.json		server.js
            michaelbradley@Michaels-iMac-2.local /Users/michaelbradley/new-project/js-app/app [master]
            % clear

            michaelbradley@Michaels-iMac-2.local /Users/michaelbradley/new-project/js-app/app [master]
            % pwd
            /Users/michaelbradley/new-project/js-app/app
            michaelbradley@Michaels-iMac-2.local /Users/michaelbradley/new-project/js-app/app [master]
            % npm install

            added 154 packages, and audited 155 packages in 3s

            12 packages are looking for funding
            run `npm fund` for details

            3 vulnerabilities (2 moderate, 1 high)

            To address all issues, run:
            npm audit fix

            Run `npm audit` for details.

            
            Connected to browser from nodejs - localhost:3000

                Added users and db from collection

            michaelbradley@Michaels-iMac-2.local /Users/michaelbradley/new-project/js-app/app [master]
            % node server.js
            (node:61983) [DEP0040] DeprecationWarning: The `punycode` module is deprecated. Please use a userland alternative instead.
            (Use `node --trace-deprecation ...` to show where the warning was created)
            app listening on port 3000!

            





# Usage


DevOps % docker pull mongo

Using default tag: latest
latest: Pulling from library/mongo
7646c8da3324: Pull complete 
11f24d004130: Pull complete 
1dfef8821fc6: Pull complete 
bd2b135d9110: Pull complete 
76154bf34598: Pull complete 
a93134489020: Pull complete 
fd1fa78ffcfb: Pull complete 
d91b3273507c: Pull complete 
Digest: sha256:bd38dc3d2895c7434b9b75c86525642efe3d65e4c6aadfe397486d7cc89406f0
Status: Downloaded newer image for mongo:latest
docker.io/library/mongo:latest


docker pull mongo-express
Using default tag: latest
latest: Pulling from library/mongo-express
619be1103602: Pull complete 
7e9a007eb24b: Pull complete 
5189255e31c8: Pull complete 
88f4f8a6bc8d: Pull complete 
d8305ae32c95: Pull complete 
45b24ec126f9: Pull complete 
9f7f59574f7d: Pull complete 
0bf3571b6cd7: Pull complete 
Digest: sha256:1b23d7976f0210dbec74045c209e52fbb26d29b2e873d6c6fa3d3f0ae32c2a64
Status: Downloaded newer image for mongo-express:latest
docker.io/library/mongo-express:latest


michaelbradley@Michaels-iMac-2 DevOps % docker images

REPOSITORY      TAG       IMAGE ID       CREATED        SIZE
mongo           latest    95b3ba6bed35   4 weeks ago    797MB
redis           latest    aceb1262c1ea   4 weeks ago    117MB
redis           6.2       0bff933a3fa7   4 weeks ago    106MB
postgres        15.7      2f77c5a59ad5   6 weeks ago    425MB
postgres        13.15     f82ad01a3be6   6 weeks ago    419MB
mongo-express   latest    870141b735e7   3 months ago   182MB
    

michaelbradley@Michaels-iMac-2 DevOps % docker network create mongo-network

michaelbradley@Michaels-iMac-2 DevOps % docker network ls
        NETWORK ID     NAME            DRIVER    SCOPE
        a03ce6e24737   bridge          bridge    local
        80e6110b908e   host            host      local
        37019f61ee4e   mongo-network   bridge    local
        8fc3b5c03502   none            null      local


michaelbradley@Michaels-iMac-2 DevOps % docker logs mongodb |grep 27017

{"t":{"$date":"2024-06-22T20:34:11.418+00:00"},"s":"I",  "c":"CONTROL",  "id":4615611, "ctx":"initandlisten","msg":"MongoDB starting","attr":{"pid":27,"port":27017,"dbPath":"/data/db","architecture":"64-bit","host":"cf16b7a7b8d5"}}
{"t":{"$date":"2024-06-22T20:34:11.418+00:00"},"s":"I",  "c":"CONTROL",  "id":21951,   "ctx":"initandlisten","msg":"Options set by command line","attr":{"options":{"net":{"bindIp":"127.0.0.1","port":27017,"tls":{"mode":"disabled"}},"processManagement":{"fork":true,"pidFilePath":"/tmp/docker-entrypoint-temp-mongod.pid"},"systemLog":{"destination":"file","logAppend":true,"path":"/proc/1/fd/1"}}}}
{"t":{"$date":"2024-06-22T20:34:11.978+00:00"},"s":"I",  "c":"NETWORK",  "id":23015,   "ctx":"listener","msg":"Listening on","attr":{"address":"/tmp/mongodb-27017.sock"}}
{"t":{"$date":"2024-06-22T20:34:11.979+00:00"},"s":"I",  "c":"NETWORK",  "id":23016,   "ctx":"listener","msg":"Waiting for connections","attr":{"port":27017,"ssl":"off"}}
{"t":{"$date":"2024-06-22T20:34:13.300+00:00"},"s":"I",  "c":"NETWORK",  "id":23017,   "ctx":"listener","msg":"removing socket file","attr":{"path":"/tmp/mongodb-27017.sock"}}
{"t":{"$date":"2024-06-22T20:34:14.326+00:00"},"s":"I",  "c":"CONTROL",  "id":4615611, "ctx":"initandlisten","msg":"MongoDB starting","attr":{"pid":1,"port":27017,"dbPath":"/data/db","architecture":"64-bit","host":"cf16b7a7b8d5"}}
{"t":{"$date":"2024-06-22T20:34:14.905+00:00"},"s":"I",  "c":"NETWORK",  "id":23015,   "ctx":"listener","msg":"Listening on","attr":{"address":"/tmp/mongodb-27017.sock"}}
{"t":{"$date":"2024-06-22T20:34:14.905+00:00"},"s":"I",  "c":"NETWORK",  "id":23016,   "ctx":"listener","msg":"Waiting for connections","attr":{"port":27017,"ssl":"off"}}


michaelbradley@Michaels-iMac-2 DevOps % docker run -d \        
        -p 8081:8081 \     
        -e ME_CONFIG_MONGODB_ADMINUSERNAME=admin \   
        -e ME_CONFIG_MONGODB_ADMINPASSWORD=password \
        --net mongo-network \
        --name mongo-express \
        -e ME_CONFIG_MONGODB_SERVER=mongodb \
        mongo-express
        fd95b07d09c1a60f7d681d25917a542efd33bfdf74cb4009d57a542982c90820

        michaelbradley@Michaels-iMac-2 DevOps % docker logs mongo-express
        Waiting for mongo:27017...

        Welcome to mongo-express 1.0.2
        ------------------------


        Mongo Express server listening at http://0.0.0.0:8081


        michaelbradley@Michaels-iMac-2.local /Users/michaelbradley/new-project/js-app/app [master]
            % node server.js
            (node:61983) [DEP0040] DeprecationWarning: The `punycode` module is deprecated. Please use a userland alternative instead.
            (Use `node --trace-deprecation ...` to show where the warning was created)
            app listening on port 3000!