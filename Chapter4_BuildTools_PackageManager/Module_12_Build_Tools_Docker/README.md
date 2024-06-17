# Chapter4
This module we looked at build tools and docker.

# Name: Build Tools And Docker

# Description: 

No need to build and move different artifact types (e.g. Jar, War, Zip)

Docker allows you to use a single artifact  - Docker Image

We build those Docker images from the Application

No need for a repository for each file type
    
    Just 1 for Docker Images

No need to install dependencies on the server!

    Execute install command inside Docker Image

Docker makes it easier

    Docker Image is an artifact

Docker Image is an alternative for all other artifact type

You don't have to install npm or java on the server

    Execute everything in the Docker Image

    You still need to build the Apps!

    Still need to build the JAR for the Java app and the package.json for the JavaScript

    Everything is packaged inside of the Docker Image

Dockerfile is used to structure the application directories and perform npm install.


# Usage

