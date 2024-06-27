# Chapter7
This module we look at the push/pull Nexus Repository.

# Name: Push / PULL Nexus Repository - Hosted Repository

# Description: 

In this module we will be creating docker hosting repository on Nexus. 

    In admin mode, click the gear

    Select Repository on the left
    
    Select create repository

    Next, select docker hosted for type

    Make the following entries:

        Name: docker-hosted

        Blob Store: mystore

    Select Create Repository



    docker-hosted - appears at the top of the Repositories "Managed Repositories" list

        Click and open it. The repo now has certain attributes defined:
        
        Name:  docker-hosted
        Format:  docker
        Type:  hosted
        URL:  http://157.230.22.19:8081/repository/docker-hosted
        Online: accepts in comming request


    Create User Role for Docker Repo

        To start using Nexus a user id must be setup that has access to Nexus repo.

        Under security, select Roles

        Create Role

        Type: Nexus Role

        Role ID: nx-docker    
       
        Add the following privileges:

                nx-repository-view-docker-docker-hosted-*

        save

        Under Security, select Users

        Select existing user - michael

        Grant nx-docker role

        save


    Docker Login to Nexus Docker Repo

        From the docker-hosted page under Repositories, in Nexus

        Configure the HTTP connector, by adding port 8083. Note: Nexus uses port 8081

        save


    From the droplet open the port 8083 from the Firewall

    Next from the Next UI click Realms

        Click on Docker Bearer Token Realm to activate the docker token

        
    Must configure Docker Client to allow insecure Nexus access to docker-hosted repo

    From MacOS or Windows client use Docker Desktop Docker Engine and add IP and port 8083

            
    "builder": {
        "gc": {
        "defaultKeepStorage": "20GB",
        "enabled": true
        }
    },
    "experimental": false,
    "insecure-registries": [
        "157.230.238.59:8083"
    ],
    "features": {
        "buildkit": true
    }


    Click Apply & restart

    Docker Desktop will now allow docker client to send requests to the Nexus registry.

    Now we can authenticate using docker login

    michaelbradley@Michaels-iMac-2 app % docker login 157.230.238.59:8083
    Username: michael
    Password: 
    Login Succeeded
    michaelbradley@Michaels-iMac-2 app %


    docker build -t my-app:2.0 . **Used 2.0 due to conflicts with previous image 


    js-app % docker tag my-app:2.0 157.230.238.59:8083/my-app:2.0

    docker images

    REPOSITORY                                            TAG       IMAGE ID       CREATED         SIZE
    my-app                                                2.0       4f83058baa26   5 minutes ago   157MB
    157.230.238.59:8083/my-app                            2.0       4f83058baa26   5 minutes ago   157MB


    Repository	docker-hosted
    Format	docker
    Component Name	my-app
    Component Version	2.0

    /manifests/sha256:41b3ef26db07718a6925df9bc306a67e4fa1e248fbf01ffd248334f00e15302a - matches digest in push

    docker push 157.230.238.59:8083/my-app:2.0


    Fetch Docker Image from Nexus - PULL

    michaelbradley@Michaels-iMac-2 js-app % curl -u michael:Greater16 -X GET 'http://157.230.238.59:8081/service/rest/v1/components?repository=docker-hosted'
{
  "items" : [ {
    "id" : "ZG9ja2VyLWhvc3RlZDo2ZGZkYTBhODQzZjQyMDFkN2M4MzE3MzFiMjYwOWFlZQ",
    "repository" : "docker-hosted",
    "format" : "docker",
    "group" : null,
    "name" : "my-app",
    "version" : "2.0",
    "assets" : [ {
      "downloadUrl" : "http://157.230.238.59:8081/repository/docker-hosted/v2/my-app/manifests/2.0",
      "path" : "v2/my-app/manifests/2.0",
      "id" : "ZG9ja2VyLWhvc3RlZDo1ZTczOWMxZTFmOGJkYjIwMGU0ZmMzOTk2YjMwZmZiYg",
      "repository" : "docker-hosted",
      "format" : "docker",
      "checksum" : {
        "sha1" : "76ba4a8d57cdcf7657d5f944c65b9817b7ae84a7",
        "sha256" : "41b3ef26db07718a6925df9bc306a67e4fa1e248fbf01ffd248334f00e15302a"
      },
      "contentType" : "application/vnd.docker.distribution.manifest.v2+json",
      "lastModified" : "2024-06-27T02:21:58.195+00:00",
      "lastDownloaded" : null,
      "uploader" : "michael",
      "uploaderIp" : "45.25.106.162",
      "fileSize" : 1992,
      "blobCreated" : null,
      "blobCreated" : "2024-06-27T02:21:58.195+00:00"
    } ]
  } ],
  "continuationToken" : null
}%                                                    

# Usage

root@ubuntu-s-4vcpu-8gb-nyc1-01:/opt# ls
digitalocean  nexus-3.69.0-02  nexus-3.69.0-02-java8-unix.tar.gz  sonatype-work
root@ubuntu-s-4vcpu-8gb-nyc1-01:/opt# netstat -lnpt
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name    
tcp        0      0 0.0.0.0:8081            0.0.0.0:*               LISTEN      4046/java           
tcp        0      0 0.0.0.0:8083            0.0.0.0:*               LISTEN      4046/java           
tcp        0      0 127.0.0.1:44363         0.0.0.0:*               LISTEN      4046/java           
tcp        0      0 127.0.0.53:53           0.0.0.0:*               LISTEN      739/systemd-resolve 
tcp        0      0 127.0.0.54:53           0.0.0.0:*               LISTEN      739/systemd-resolve 
tcp6       0      0 :::22                   :::*                    LISTEN      1/init  
    

michaelbradley@Michaels-iMac-2 app % docker login 157.230.238.59:8083
Username: michael
Password: 
Login Succeeded


Push Image to Nexus Repo

michaelbradley@Michaels-iMac-2 js-app % docker build -t my-app:2.0 .
[+] Building 11.5s (11/11) FINISHED                                                                                                                                                      docker:desktop-linux
 => [internal] load build definition from Dockerfile                                                                                                                                                     0.0s
 => => transferring dockerfile: 403B                                                                                                                                                                     0.0s
 => [internal] load metadata for docker.io/library/node:20-alpine                                                                                                                                        0.7s
 => [auth] library/node:pull token for registry-1.docker.io                                                                                                                                              0.0s
 => [internal] load .dockerignore                                                                                                                                                                        0.0s
 => => transferring context: 2B                                                                                                                                                                          0.0s
 => [1/5] FROM docker.io/library/node:20-alpine@sha256:df01469346db2bf1cfc1f7261aeab86b2960efa840fe2bd46d83ff339f463665                                                                                  5.4s
 => => resolve docker.io/library/node:20-alpine@sha256:df01469346db2bf1cfc1f7261aeab86b2960efa840fe2bd46d83ff339f463665                                                                                  0.0s
 => => sha256:b0691ab91e2774c9a10e06e87cd53c04d60fec055f28d158efb4768080e5beb0 1.39MB / 1.39MB                                                                                                           0.5s
 => => sha256:b76f83d65b1b4ebbbf1dc6c3a171a2658ebb2d3b726dbfdd7b8bfd645698f5f9 451B / 451B                                                                                                               0.3s
 => => sha256:df01469346db2bf1cfc1f7261aeab86b2960efa840fe2bd46d83ff339f463665 7.67kB / 7.67kB                                                                                                           0.0s
 => => sha256:24c14a8a192a6e81d0942929a344f7a4bdf0db8e3b3c77d64a5eb8a4b0c759b7 1.72kB / 1.72kB                                                                                                           0.0s
 => => sha256:fe82994959881783fd8dd4b838559f7a3da80126d2fd6aa965b905e587e114b7 6.38kB / 6.38kB                                                                                                           0.0s
 => => sha256:ba1057ec209ef729ee564099a0340d0b78324c97778af46879c287bc13e2ebe8 42.19MB / 42.19MB                                                                                                         3.8s
 => => extracting sha256:ba1057ec209ef729ee564099a0340d0b78324c97778af46879c287bc13e2ebe8                                                                                                                1.4s
 => => extracting sha256:b0691ab91e2774c9a10e06e87cd53c04d60fec055f28d158efb4768080e5beb0                                                                                                                0.0s
 => => extracting sha256:b76f83d65b1b4ebbbf1dc6c3a171a2658ebb2d3b726dbfdd7b8bfd645698f5f9                                                                                                                0.0s
 => [internal] load build context                                                                                                                                                                        0.3s
 => => transferring context: 342.43kB                                                                                                                                                                    0.3s
 => [2/5] RUN mkdir -p /home/app                                                                                                                                                                         0.3s
 => [3/5] COPY ./app /home/app                                                                                                                                                                           0.6s
 => [4/5] WORKDIR /home/app                                                                                                                                                                              0.0s
 => [5/5] RUN npm install                                                                                                                                                                                4.1s
 => exporting to image                                                                                                                                                                                   0.3s
 => => exporting layers                                                                                                                                                                                  0.2s
 => => writing image sha256:4f83058baa2623e1b1c50a22ab8f77578c35d42e5a78ee85fb934d2c2d1bbf54                                                                                                             0.0s
 => => naming to docker.io/library/my-app:2.0                                                                                                                                                            0.0s

What's next:
    View a summary of image vulnerabilities and recommendations → docker scout quickview 


michaelbradley@Michaels-iMac-2 js-app % docker tag my-app:2.0 157.230.238.59:8083/my-app:2.0

michaelbradley@Michaels-iMac-2 js-app % docker images
REPOSITORY                                            TAG       IMAGE ID       CREATED         SIZE
my-app                                                2.0       4f83058baa26   5 minutes ago   157MB
157.230.238.59:8083/my-app                            2.0       4f83058baa26   5 minutes ago   157MB
211125391769.dkr.ecr.us-east-1.amazonaws.com/my-app   1.0       381e7dfb9ca7   3 days ago      157MB
211125391769.dkr.ecr.us-east-2.amazonaws.com/my-app   1.0       381e7dfb9ca7   3 days ago      157MB

michaelbradley@Michaels-iMac-2 js-app % docker push 157.230.238.59:8083/my-app:2.0

The push refers to repository [157.230.238.59:8083/my-app]
e62c2c5e4587: Pushed 
5f70bf18a086: Pushed 
1c0308c05551: Pushed 
2db6f495ae80: Pushed 
61a4672d547d: Pushed 
64afd0417f62: Pushed 
ccd1c30cd963: Pushed 
94e5f06ff8e3: Pushed 
2.0: digest: sha256:41b3ef26db07718a6925df9bc306a67e4fa1e248fbf01ffd248334f00e15302a size: 1992


michaelbradley@Michaels-iMac-2 js-app % curl -u michael:Greater16 -X GET 'http://157.230.238.59:8081/service/rest/v1/components?repository=docker-hosted'
{
  "items" : [ {
    "id" : "ZG9ja2VyLWhvc3RlZDo2ZGZkYTBhODQzZjQyMDFkN2M4MzE3MzFiMjYwOWFlZQ",
    "repository" : "docker-hosted",
    "format" : "docker",
    "group" : null,
    "name" : "my-app",
    "version" : "2.0",
    "assets" : [ {
      "downloadUrl" : "http://157.230.238.59:8081/repository/docker-hosted/v2/my-app/manifests/2.0",
      "path" : "v2/my-app/manifests/2.0",
      "id" : "ZG9ja2VyLWhvc3RlZDo1ZTczOWMxZTFmOGJkYjIwMGU0ZmMzOTk2YjMwZmZiYg",
      "repository" : "docker-hosted",
      "format" : "docker",
      "checksum" : {
        "sha1" : "76ba4a8d57cdcf7657d5f944c65b9817b7ae84a7",
        "sha256" : "41b3ef26db07718a6925df9bc306a67e4fa1e248fbf01ffd248334f00e15302a"
      },
      "contentType" : "application/vnd.docker.distribution.manifest.v2+json",
      "lastModified" : "2024-06-27T02:21:58.195+00:00",
      "lastDownloaded" : null,
      "uploader" : "michael",
      "uploaderIp" : "45.25.106.162",
      "fileSize" : 1992,
      "blobCreated" : null,
      "blobCreated" : "2024-06-27T02:21:58.195+00:00"
    } ]
  } ],
  "continuationToken" : null
}%                                                    