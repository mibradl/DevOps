# Chapter7
This module we explore the private docker repository

# Name: Docker Registry
AKIATCKAN2GM7Z6HNA5D
# Description: 

Overview

    Docker private repository

    Registry options

    Docker Registry

    Amazon ECR

    Build & tag an image

    docker login

    docker push

Create private Docker repository on AWS

    AWS

    AWS ECR = Docker Registry

        A service providing storage

        Collection of repositories

        A repository per image

        You store a collection of related images with same name, but different versions

        You save different tags (versions) of the same image


Image Naming in Docker registries

        registryDomain/imageName:tag

        In DockerHub

            docker pull mongo:4.2                   Shorthand

            docker pull docker.io/library/mongo:4.2

            By default, docker pull pulls images from Docker Hub

        In AWS ECR 
            
            docker pull 211125391769.dkr.ecr.us-east-1.amazonaws.com/my-app:1.0

            tag

            docker tag my-app:1.0 211125391769.dkr.ecr.us-east-1.amazonaws.com/my-app:1.0

            docker tag = rename the image

                      from my-app:1.0  to   registryDomain/imageName:tag      



        Run command to execute tag docker image and name locally

            docker tag my-app:1.0 211125391769.dkr.ecr.us-east-1.amazonaws.com/my-app:1.0



        Execute command to list docker images locally

            docker images

            REPOSITORY                                            TAG       IMAGE ID       CREATED        SIZE
            211125391769.dkr.ecr.us-east-1.amazonaws.com/my-app   1.0       381e7dfb9ca7   44 hours ago   157MB



        Perform docker push from local to AWS ECR with assigned tag

            The push refers to repository [211125391769.dkr.ecr.us-east-1.amazonaws.com/my-app]
            90d660239f13: Pushed 
            5f70bf18a086: Pushed   

# Usage

michaelbradley@Michaels-iMac-2 ~ % docker tag my-app:1.0 211125391769.dkr.ecr.us-east-1.amazonaws.com/my-app:1.0



michaelbradley@Michaels-iMac-2 ~ % docker images                                                                
REPOSITORY                                            TAG       IMAGE ID       CREATED        SIZE
211125391769.dkr.ecr.us-east-1.amazonaws.com/my-app   1.0       381e7dfb9ca7   44 hours ago   157MB
my-app                                                1.0       381e7dfb9ca7   44 hours ago   157MB
mongo                                                 latest    95b3ba6bed35   4 weeks ago    797MB
redis                                                 latest    aceb1262c1ea   4 weeks ago    117MB
redis                                                 6.2       0bff933a3fa7   4 weeks ago    106MB
postgres                                              15.7      2f77c5a59ad5   6 weeks ago    425MB
postgres                                              13.15     f82ad01a3be6   6 weeks ago    419MB
mongo-express                                         latest    870141b735e7   3 months ago   182MB



michaelbradley@Michaels-iMac-2 ~ % docker push 211125391769.dkr.ecr.us-east-1.amazonaws.com/my-app:1.0          
The push refers to repository [211125391769.dkr.ecr.us-east-1.amazonaws.com/my-app]
90d660239f13: Pushed 
5f70bf18a086: Pushed 
0d1fa5284842: Pushed 
7869ec144a70: Pushed 
ea3a39385d46: Pushed 
ac51c88d51fe: Pushed 
ba9916be9d18: Pushed 
94e5f06ff8e3: Pushed 
1.0: digest: sha256:91157e0a5aa4fc9aab8e5ad0e7a713942e7100bd5bbb040566ed61d9f2b227ad size: 1992