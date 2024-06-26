# Chapter7
This module we review how to deploy the application.

# Name: Deploy the application

# Description: 

Overview - Deploying the application

    Image from a private repository - AWS ECR

    Deploy multiple containers

    Objective:
    
    pull the my-app:1.0 from AWS ECR to the Dev server

    pull mongo express and mongodb from Docker Hub to the Dev server



    Pre-requisite

        The server needs to login to pull from PRIVATE repository

        Login no needed for PUBLIC Docker Hub



    This Docker-Compose file will be be used on the server to deploy all the applications/services


    Update connection URL in application


    Sample of server.js code with mongoUrlLocal = "mongodb://admin:password@mongodb"

            // use when starting application locally with node command
        let mongoUrlLocal = "mongodb://admin:password@localhost:27017";

        // use when starting application as docker container, part of docker-compose
        let mongoUrlDockerCompose = "mongodb://admin:password@mongodb";

        // pass these options to mongo client connect request to avoid DeprecationWarning for current Server Discovery and Monitoring engine
        let mongoClientOptions = { useNewUrlParser: true, useUnifiedTopology: true };

        // "user-account" in demo with docker
        let databaseName = "user-account";
        let collectionName = "users";

        app.get('/get-profile', function (req, res) {
        let response = {};
        // Connect to the db using local application or docker compose variable in connection properties
        MongoClient.connect(mongoUrlLocal, mongoClientOptions, function (err, client) {
            if (err) throw err;

            let db = client.db(databaseName);

            let myquery = { userid: 1 };

            db.collection(collectionName).findOne(myquery, function (err, result) {
            if (err) throw err;
            response = result;
            client.close();

            // Send response
            res.send(response ? response : {});
            });
        });
        });

        app.post('/update-profile', function (req, res) {
        let userObj = req.body;
        // Connect to the db using local application or docker compose variable in connection properties
        MongoClient.connect(mongoUrlLocal, mongoClientOptions, function (err, client) {
            if (err) throw err;



michaelbradley@Michaels-iMac-2 js-app % docker-compose -f docker-compose.yaml up
WARN[0000] /Users/michaelbradley/new-project/js-app/docker-compose.yaml: `version` is obsolete 
[+] Running 3/3
 ✔ Container js-app-my-app-1         Created                                                                                                                                                             0.3s 
 ✔ Container js-app-mongodb-1        Recreated                                                                                                                                                           0.4s 
 ✔ Container js-app-mongo-express-1  Recreated                                                                                                                                                           0.1s 
Attaching to mongo-express-1, mongodb-1, my-app-1
mongo-express-1  | Waiting for mongo:27017...
my-app-1         | app listening on port 3000!
    

        mongo-express-1  | Welcome to mongo-express 1.0.2
mongo-express-1  | ------------------------
mongo-express-1  | 
mongo-express-1  | 
mongodb-1        | {"t":{"$date":"2024-06-25T23:13:44.549+00:00"},"s":"I",  "c":"NETWORK",  "id":22943,   "ctx":"listener","msg":"Connection accepted","attr":{"remote":"172.23.0.4:46516","uuid":{"uuid":{"$uuid":"6a014c94-fe22-4d66-a1a2-0074f5b675f3"}},"connectionId":1,"connectionCount":1}}
mongodb-1        | {"t":{"$date":"2024-06-25T23:13:44.559+00:00"},"s":"I",  "c":"NETWORK",  "id":51800,   "ctx":"conn1","msg":"client metadata","attr":{"remote":"172.23.0.4:46516","client":"conn1","negotiatedCompressors":[],"doc":{"driver":{"name":"nodejs","version":"4.13.0"},"os":{"type":"Linux","name":"linux","architecture":"x64","version":"6.6.31-linuxkit"},"platform":"Node.js v18.20.3, LE (unified)|Node.js v18.20.3, LE (unified)"}}}
mongodb-1        | {"t":{"$date":"2024-06-25T23:13:44.571+00:00"},"s":"I",  "c":"NETWORK",  "id":22943,   "ctx":"listener","msg":"Connection accepted","attr":{"remote":"172.23.0.4:46532","uuid":{"uuid":{"$uuid":"753350b8-6d78-4753-b5e2-7d327064c58e"}},"connectionId":2,"connectionCount":2}}
mongodb-1        | {"t":{"$date":"2024-06-25T23:13:44.578+00:00"},"s":"I",  "c":"NETWORK",  "id":51800,   "ctx":"conn2","msg":"client metadata","attr":{"remote":"172.23.0.4:46532","client":"conn2","negotiatedCompressors":[],"doc":{"driver":{"name":"nodejs","version":"4.13.0"},"os":{"type":"Linux","name":"linux","architecture":"x64","version":"6.6.31-linuxkit"},"platform":"Node.js v18.20.3, LE (unified)|Node.js v18.20.3, LE (unified)"}}}
mongodb-1        | {"t":{"$date":"2024-06-25T23:13:44.622+00:00"},"s":"I",  "c":"ACCESS",   "id":6788604, "ctx":"conn2","msg":"Auth metrics report","attr":{"metric":"acquireUser","micros":0}}
mongodb-1        | {"t":{"$date":"2024-06-25T23:13:44.639+00:00"},"s":"I",  "c":"ACCESS",   "id":5286306, "ctx":"conn2","msg":"Successfully authenticated","attr":{"client":"172.23.0.4:46532","isSpeculative":true,"isClusterMember":false,"mechanism":"SCRAM-SHA-256","user":"admin","db":"admin","result":0,"metrics":{"conversation_duration":{"micros":19046,"summary":{"0":{"step":1,"step_total":2,"duration_micros":1721},"1":{"step":2,"step_total":2,"duration_micros":1189}}}},"extraInfo":{}}}
mongodb-1        | {"t":{"$date":"2024-06-25T23:13:44.642+00:00"},"s":"I",  "c":"NETWORK",  "id":6788700, "ctx":"conn2","msg":"Received first command on ingress connection since session start or auth handshake","attr":{"elapsedMillis":4}}
mongo-express-1  | Mongo Express server listening at http://0.0.0.0:8081
mongo-express-1  | Server is open to allow connections from anyone (0.0.0.0)
mongo-express-1  | basicAuth credentials are "admin:pass", it is recommended you change this in your config.js!
mongodb-1        | {"t":{"$date":"2024-06-25T23:13:55.088+00:00"},"s":"I",  "c":"NETWORK",  "id":22943,   "ctx":"listener","msg":"Connection accepted","attr":{"remote":"172.23.0.4:48104","uuid":{"uuid":{"$uuid":"e8d3a55a-89ae-4405-9d02-168c44b93c59"}},"connectionId":3,"connectionCount":3}}
mongodb-1        | {"t":{"$date":"2024-06-25T23:13:55.090+00:00"},"s":"I",  "c":"NETWORK",  "id":51800,   "ctx":"conn3","msg":"client me

# Usage