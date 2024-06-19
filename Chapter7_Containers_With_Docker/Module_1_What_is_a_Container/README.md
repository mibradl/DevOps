# Chapter7
This module we explore what a Container is.

# Name: What is a Container?

# Description: 

Overview

What is a Container?

    A way to package applications with all the necessary dependencies and configuration

    Portable artifact easily shared and moved around

    Makes development and deployment more efficient

Where do containers live?

    Containter Repository

    Companies have Private repositories

    There is a public repository for Docker - (Postgres, Redis, Nodejs, Nginx, etc)

        DockerHub - over 100,000 container images

How Docker Containers improve development

    Before containers

        Developers had to install binaries on their computers - (E.g. PostgreSQL, Redis)

            Mac User        Linux User      Windows User


        Installation process different on each OS environment

        Many steps where something could go wrong


How it solves some of the issue with  deploying an application

    Don't have to install any of the the services on your computer

    The container is its own isolated environment

    Everything is packaged with the complete configuration in one environment

    One command that fetches the container and installs the application

    Run same app with 2 different versions

Application Deployment

    Before Containers

        Developer would produce artifact, with a set of instrucations on how to install it (JAR, WAR, Zip and DB instructions)

        Operations would take those instructions and set it up

        Everything was install on the server provided.

        Sometimes there were mistakes with configuration or instructions.


    With containers the developers and operation are working together - DevOps

        Since the configuration is already setup all that needs to be done is to pull it down and run it

        Docker is the most popular example, others include container-d, cri-o, etc

        


# Usage


    