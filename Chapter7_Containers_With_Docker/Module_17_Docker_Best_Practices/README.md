# Chapter7
This module we review best practice for docker

# Name: Docker Best Practice

# Description: 

Best Practice

    Use Official Docker Images as Base Image

        Instead of using base operating system image

        Don't install packages by yourself

        Use official "node" image as base image

        Cleaner Dockerfile

        Official Docker Image

        Verified and already built with best practices


    It will use the latest tag

        FROM node == FROM node:latest

        You might get different docker image versions

        This might break things

        Latest tag is unpredictable

    Use Specific Image Versions

        Do Not use a random latest tag

        Fixate the version

        The more specific, the better


    Different version numbers and different operating system distros


        Which one do you choose? Does it matter?

        If based on a Full-Blown Operating System Distro - ubuntu, centos, debian

            System Utilities packages in

                Larger Image Size

                Will have more security vulnerabilities

                Introduce unnecessary security issues from the beginning




        Your image will never use many OS features


                Smaller Images

                    Less storage space

                    Transfer image faster

                    Leaner Operating System Distro - Minimize attack surface

                    Only bundle necessary utilities (e.g. Busybox instead of GNU Core Utilities)

                    Could be based on full-blown OS
                    FROM node:20.0.2

                    Use Image based on a leaner and smaller OS Distro
                        alpine - lightweight Linux distro

                    Security oriented

                    Popular base image


        Optimize Caching Image Layers

                    What are Image Layers?

                        Docker images are built based on a Dockerfile

                        Each command or instruction creates an image layer

                        The base layer consists of several layers it was built with

                        docker history my-app:1.0 - displays image layers with corresponding commands

                    What does Caching an Image Layer mean?

                        Docker caches each layer, saved on local filesystem

                        If nothing has changed in a layer (or any layers preceding it), it will be reused freom cache

                        Allows for faster image building

                        Downloads only added layers


                    Dockerfile Instructions

                        Use Node Apline Image as Base Image

                        Go to /app folder

                        Copy project files into the Docker Image

                        Install project dependencies with npm install


                    Why isn't it used from cache?

                        Once a layer changes, all following layers are re-created as well


                    DO NOT RE-RUN: when project files changes

                    RE-RUN: when package.json file changes

                        Re-structure Dockerfile instructions

                        COPY package.json package-lock.json .

                        RUN npm install --production

                        COPY myapp /app

                        ** if we add or remove a dependency in package.json file it will be copied. If other files are changed package.json, installed dependencies will be cached.

        
        Order Dockerfile commands from least to most frequently changing

                    Least

                        FROM node:20.0.2-alpine

                        COPY package.json package-lock.json .

                        RUN npm install --production

                        COPY myapp /app

                        CMD ["node", "src/index.js"]

                    Most Frequently changed

                Take advantage of caching

                Faster image building and fetching



        We don't need everything inside the Dockerfile Image

                        docker build -t myapp .

                        don't need auto-generated folders (like target build)

                        README files


                How do we exclude these folders and files?

                        In order to reduce the image size 

                        Prevent unintended secrets exposure

                Use .dockerignore to explicitly exclude files and folders

                        Create .dockerignore file in the root directory

                        List files and folders you want to ignore

                        Matching is done using Go's filepath.Match rules


                            # ignore .git and .cache folders
                            .git
                            .cache

                            # ignore all markdown files (md)
                            *.md

                            # ignore sensative files
                            private.keys
                            settings.json


        Contents, that you need for building the image


                        docker build -t myapp .


        DON't need them in the final image to run the app

                        docker run myapp

                        Many artifacts get used during the build, development tools, build tools, test dependencies, temporary files

                        If you keep these in your image file it will result in increased image size and increase attack surface

        Specify project dependencies

        Needed for installing dependencies

                        package.json
                        pom.xml

                        Not needed for running the app

        JDK is needed to compile Java source code

                        Not needed to run the Java application
                        
                        Might be using Java build tools for building the application

                            Gradle
                            Maven

                        Also, not needed in final image


        How do we separate the Build Stage from the Runtime Stage?

                        Build Stage

                        FROM tomcat
                        RUN apt-get update \
                            && apt-get -y install maven
                        WORKDIR /app
                        COPY myapp /app
                        RUN mvn package

                        Runtime Stage

                        COPY --from=build /app/target/file.war /usr/local/tomcat/webapps
                        EXPOSE 8080
                        ENTRYPOINT ["java","-jar","/usr/local/lib/demo.jar"]


        Make use of "Multi-Stage Builds"

                        Allows you to use multiple images during the build

                        Dockerfile with 2 Build Stages


                        You can name your stages with AS <name>
                        1st stage: Builds Java App

                        # Build Stage
                        FROM maven AS build
                        WORKDIR /app
                        COPY myapp /app
                        RUN mvn package

                        Each FROM instruction starts a new build stage

                        You can selectively copy artifacts from one stage to another

                        # Run Stage
                        FROM tomcat
                        COPY --from=build /app/target/file.war /usr/local/tomcat
                        ...

                        The final image is created in the last stage, leaving everything behind you don't want in the final image

                        Only the last Dockerfile commands are the Image Layers

                        Previous steps will be discarded, which allows you to separate the build tools and dependencies from what needed at runtime

                        Reduces the size of the final image


    Which OS user will be used to start the application

                        By default when a docker user does not specify a user it uses root user (uid 0)

                        Few use cases, where the container needs to run as root. This is a "Security Bad Practice"

                        When docker image is started on the host, the container could potentially have root access on the Docker host

                        Easier privilege escalation for an attacker



    Use the Least Privileged User

                        Create a dedicated user and group

                        Don't forget to set required permissions

                        Change to non-root user with USER directive

                        Some Base Images have a generic user bundled in

                            No need to create one yourself
                            Nodejs already has generic user node
                            Nexus already has generic user nexus

                        ...
                        # create group and user
                        RUN groupadd -r tom && useradd -g tom tom

                        # set ownership and permissions
                        RUN chown -R tom:tom /app

                        # switch to user
                        USER tom

                        CMD node index.js





        Use small-sized Official Images


    How to scan the built image for security vulnerabilities?

                        Scan your image for security vulnerabilities

                        docker scout cves myapp:1.0

                        Must login to docker hub to perform scan and then docker login from the command line

                        docker scout cves myapp:1.0

                        Docker uses its own service for the vulnerability scan against it's own database.

                        The database gets updated with known vulnerabilities



# Usage


    