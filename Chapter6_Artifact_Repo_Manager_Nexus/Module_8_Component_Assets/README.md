# Chapter6
This module we review Component versus Asset

# Name: Component and Asset

# Description: 

 What is the difference between a Component and Asset?

 The items that were uploaded to Nexus are Components

    com
        example
                java-maven-app - top level are the Components

                        1.1.0-SNAPSHOT - Assets
                        ...
                        meta-data

                my-app - top level are the Components

                        1.0-SNAPSHT - Assets
                        ...
                        meta-data


Components

    Abstract

    What we are uploading (the application that was built)

    Term "component" refers to any type or format (docker, jar, zip)


Assets

    Actual physical packages/files

    1 component = 1 or more assets

Docker

    Docker format gives assets unique identifiers (Docker layers)

        Docker Layers == Assets

        E.g. 2 Docker Images => 2 Components, but share same assets




# Usage


    