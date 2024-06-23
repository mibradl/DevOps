# Chapter7
This module we explore building Image with Dockerfile.

# Name: Building Image with Dockerfile

# Description: 

Dockerfile

In this exercise we will simulate packaging the containers and JavaScript Application

    Commit the Application to Git

    Trigger the CI/CD pipeline

    Builds JavaScript Application and creates docker image

    Pushes docker image to docker repository


What is a Dockerfile?

    Copy artifact (jar, war, bundle.js) into a docker image


We will create a blueprint for building images, called a Dockerfile


Image Environment Blueprint

Install node

set MONGO_DB_USERNAME=admin
set MONGO_DB_PASSWORD=password

create /home/app folder

copy current folder files to /home/app

start the app with: "node server.js"


Dockerfile

Text file containing set of instructions used to build a Docker Image

FROM node                   Specifies the base image to use for the new image

                            Start by basing it on another image (Node, JavaScript), so that it can run our Node application, instead of a Linux base. Otherwise we would have to install Node ourselves.

ENV MONGO_DB_USERNAME=admin
    MONGO_DB_PASSWORD=password

                            Optionally define environment variables

RUN mkdir -p /home/app      RUN - execute any LINUX command

                            This directory is created INSIDE of the container!

                            Commands apply to the container environment only

COPY . /home/app            COPY - executes on the HOST machine!

                            COPY <src> <dest>    HOST ------> Container

CMD ["node", "server.js"]   Node is pre-installed, because of base image

                            CMD = entrypoint command

                            You can have multiple RUN commands, but CMD is just one



Create the Dockerfile

        Create Dockerfile with contents in Visio Studio


Image Layers

        app: 1.0                From node:20-alpine

        node:20-alpine          From alpine:3.17, which has the app version installed on top of it (node)

        alpine:3.17             Alpine Linux Image version that has node installed on top of



Build the Dockerfile

        Save the Dockerfile with the contents in the app path

        Create build - docker build -t my-app:1.0 .

        docker build is the function

                            -t  flags docker that a tag is being added

                                        name and version of the build

                                                the location of the build   . current directory


App including Dockerfile is commited to Git

        Once the Dockerfile is commited with the code, Jenkins will build an image based on the Dockerfile (as we just did)

        To share the image it is pushed to a docker repository

        A tester can take it from there (by performing a pull) and tested, or a Dev server can pull it from there


Start "my-app" container to verify:

        app starts successfully

        app  environment is configured correctly


        docker run my-app:1.0

        docker run my-app:1.0
        app listening on port 3000!


To optimize the app and ensure the latest dependencies are picked up add the following arguments to Dockerfile 

        Add WORKDIR /home/app

        Add RUN npm install

When you adjust the Dockerfile, you Must rebuild the image


% docker run my-app:1.0
app listening on port 3000!

/ # env
NODE_VERSION=20.14.0
HOSTNAME=63c6f53e5283
YARN_VERSION=1.22.22
SHLVL=1
HOME=/root
OLDPWD=/home/app
MONGO_DB_USERNAME=admin
TERM=xterm
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
MONGO_DB_PWD=password
PWD=/

/home/app # ls
Dockerfile         index.html         node_modules       package.json
images             mongo.yaml         package-lock.json  server.js


# Usage

% docker build -t my-app:1.0 .
[+] Building 13.4s (11/11) FINISHED                                                                                                                       docker:desktop-linux
 => [internal] load build definition from Dockerfile                                                                                                                      0.1s
 => => transferring dockerfile: 403B                                                                                                                                      0.0s
 => [internal] load metadata for docker.io/library/node:20-alpine                                                                                                         1.0s
 => [auth] library/node:pull token for registry-1.docker.io                                                                                                               0.0s
 => [internal] load .dockerignore                                                                                                                                         0.0s
 => => transferring context: 2B                                                                                                                                           0.0s
 => [1/5] FROM docker.io/library/node:20-alpine@sha256:804aa6a6476a7e2a5df8db28804aa6c1c97904eefb01deed5d6af24bb51d0c81                                                   7.5s
 => => resolve docker.io/library/node:20-alpine@sha256:804aa6a6476a7e2a5df8db28804aa6c1c97904eefb01deed5d6af24bb51d0c81                                                   0.0s
 => => sha256:804aa6a6476a7e2a5df8db28804aa6c1c97904eefb01deed5d6af24bb51d0c81 7.67kB / 7.67kB                                                                            0.0s
 => => sha256:082567d367e7816c7c9ddea38b0258ffbd812e3b6cc2eb84fcab748a13a76f4d 1.72kB / 1.72kB                                                                            0.0s
 => => sha256:7d574aa246b25137d960aef810914b4f7441a8a1704b9fa23dce4e559e1e9574 6.36kB / 6.36kB                                                                            0.0s
 => => sha256:ec99f8b99825a742d50fb3ce173d291378a46ab54b8ef7dd75e5654e2a296e99 3.62MB / 3.62MB                                                                            1.0s
 => => sha256:826542d541ab88dafafd1e8c6ccccf1e870336f6d48d1feff10c4720d0bebe58 42.18MB / 42.18MB                                                                          4.2s
 => => sha256:dffcc26d5732092ab309ca3f1a2d775be8eff526ef6154005da170f2d7f81108 1.39MB / 1.39MB                                                                            0.7s
 => => sha256:db472a6f05b5e1bbd31ff3a00655998a08dfa5530073d19969a4be0690e70cc2 452B / 452B                                                                                0.9s
 => => extracting sha256:ec99f8b99825a742d50fb3ce173d291378a46ab54b8ef7dd75e5654e2a296e99                                                                                 0.4s
 => => extracting sha256:826542d541ab88dafafd1e8c6ccccf1e870336f6d48d1feff10c4720d0bebe58                                                                                 3.0s
 => => extracting sha256:dffcc26d5732092ab309ca3f1a2d775be8eff526ef6154005da170f2d7f81108                                                                                 0.1s
 => => extracting sha256:db472a6f05b5e1bbd31ff3a00655998a08dfa5530073d19969a4be0690e70cc2                                                                                 0.0s
 => [internal] load build context                                                                                                                                         1.4s
 => => transferring context: 23.51MB                                                                                                                                      1.4s
 => [2/5] RUN mkdir -p /home/app                                                                                                                                          1.0s
 => [3/5] COPY ./app /home/app                                                                                                                                            0.8s
 => [4/5] WORKDIR /home/app                                                                                                                                               0.0s
 => [5/5] RUN npm install                                                                                                                                                 2.7s
 => exporting to image                                                                                                                                                    0.3s
 => => exporting layers                                                                                                                                                   0.3s
 => => writing image sha256:73195b47cdd4d3d8e72b3ed7c85a3857be6426d2c05cc5851dd7efc95c7a8ff5                                                                              0.0s
 => => naming to docker.io/library/my-app:1.0 




 % docker images
REPOSITORY      TAG       IMAGE ID       CREATED         SIZE
my-app          1.0       73195b47cdd4   2 minutes ago   157MB
mongo           latest    95b3ba6bed35   4 weeks ago     797MB
redis           latest    aceb1262c1ea   4 weeks ago     117MB
redis           6.2       0bff933a3fa7   4 weeks ago     106MB
postgres        15.7      2f77c5a59ad5   6 weeks ago     425MB
postgres        13.15     f82ad01a3be6   6 weeks ago     419MB
mongo-express   latest    870141b735e7   3 months ago    182MB
    

docker run my-app:1.0

        docker run my-app:1.0
        app listening on port 3000!


% docker build -t my-app:1.0 .
[+] Building 0.7s (11/11) FINISHED                                                                                                                        docker:desktop-linux
 => [internal] load build definition from Dockerfile                                                                                                                      0.0s
 => => transferring dockerfile: 403B                                                                                                                                      0.0s
 => [internal] load metadata for docker.io/library/node:20-alpine                                                                                                         0.5s
 => [auth] library/node:pull token for registry-1.docker.io                                                                                                               0.0s
 => [internal] load .dockerignore                                                                                                                                         0.0s
 => => transferring context: 2B                                                                                                                                           0.0s
 => [1/5] FROM docker.io/library/node:20-alpine@sha256:804aa6a6476a7e2a5df8db28804aa6c1c97904eefb01deed5d6af24bb51d0c81                                                   0.0s
 => [internal] load build context                                                                                                                                         0.2s
 => => transferring context: 342.50kB                                                                                                                                     0.2s
 => CACHED [2/5] RUN mkdir -p /home/app                                                                                                                                   0.0s
 => CACHED [3/5] COPY ./app /home/app                                                                                                                                     0.0s
 => CACHED [4/5] WORKDIR /home/app                                                                                                                                        0.0s
 => CACHED [5/5] RUN npm install                                                                                                                                          0.0s
 => exporting to image                                                                                                                                                    0.0s
 => => exporting layers                                                                                                                                                   0.0s
 => => writing image sha256:73195b47cdd4d3d8e72b3ed7c85a3857be6426d2c05cc5851dd7efc95c7a8ff5                                                                              0.0s
 => => naming to docker.io/library/my-app:1.0  


 % docker logs 63c6f53e5283
app listening on port 3000!
michaelbradley@Michaels-iMac-2.local /Users/michaelbradley/new-project/js-app/app [master]
% docker exec -it 63c6f53e5283 /bin/bash
OCI runtime exec failed: exec failed: unable to start container process: exec: "/bin/bash": stat /bin/bash: no such file or directory: unknown

What's next:
    Try Docker Debug for seamless, persistent debugging tools in any container or image → docker debug 63c6f53e5283
    Learn more at https://docs.docker.com/go/debug-cli/
michaelbradley@Michaels-iMac-2.local /Users/michaelbradley/new-project/js-app/app [master]
% docker exec -it 63c6f53e5283 /bin/sh  
/home/app # ls
Dockerfile         index.html         node_modules       package.json
images             mongo.yaml         package-lock.json  server.js
/home/app # cd /
/ # ls
bin    dev    etc    home   lib    media  mnt    opt    proc   root   run    sbin   srv    sys    tmp    usr    var


/ # env
NODE_VERSION=20.14.0
HOSTNAME=63c6f53e5283
YARN_VERSION=1.22.22
SHLVL=1
HOME=/root
OLDPWD=/home/app
MONGO_DB_USERNAME=admin
TERM=xterm
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
MONGO_DB_PWD=password
PWD=/
