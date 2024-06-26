# Chapter7
This module we explore Docker Volumes and Data Persistence.

# Name: Docker Volumes

# Description: 

Overview

When do we need Docker Volumes?  It is used for data persistence

        Used for databases

        Other stateful applications


What are the use cases?

        Container lives on a Host

        Virtual File System lives on the container - /var/lib/mysql/data. But there is no persistence, so if the container goes down data is lost. When re-creating a container it starts from a fresh state.

        Data gets automatically replicated. Host to the Container and vice versa. 

What is Docker Volumes?

        On the Host there is a physical file system - /home/mount/data.  The way volumes work we plug the physical host file system into the virtual file system.

        Folder in  physical host file system is mounted into the virtual file system of Docker

Three Volume Types
         
        Host Volumes                                                                   HOST            CONTAINER
        1. The docker run command is how docker volumes are created.  -v /home/mount/data:/var/lib/mysql/data 

        You decide where on the host file system the reference is made

        Anonymous Volumes
        2. -v /var/lib/mysql/data - for each container a folder is automatically generated and get mounted.
        /var/lib/docker/volumes/random-hash/_data

        
        Named Volumes
        3. -v name:/var/lib/mysql/data - you can reference the volume by name

        The most commonly used and the one that should be used in production is named volumes.

Docker Volumes in docker-compose

        Named Volumes

            version: '3'

            services:

                mongodb:

                    image: mongo

                    ports:
                     - 27017:27017

                    volumes:          on the container level you define the path volumes will be mounted
                     - db-data:/var/lib/mysql/data  -  the name of volume is db-data, followed the path in the container

                mongo-express:

                    image: mongo-express

                    ...

            volumes:  the name of the volumes you want to use in a container

                db-data




Docker Volumes in docker-compose file



# Usage


    