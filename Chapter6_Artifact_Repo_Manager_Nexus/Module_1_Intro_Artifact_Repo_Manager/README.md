# Chapter6
This module we cover the intro to Artifact Repository Manager

# Name: Intro to Artifact Repository Manager

# Description: 

What is an Artifact Repository?

    Artifacts are Apps built into a single file

    It can have different artifact formats (JAR, WAR, ZIP, TAR, etc)

    Artifact repository is where you store those artifacts!

    Artifact repository needs to support this specific format

    Repository for each file/artifact type


What is an Artifact Repository Manager?

    A company might have formats in Java, Python, Microsoft

    You need different repositories for them

    Different software for each repository?


Solution

    Just 1 application for managing all!

    Artifactory Repository Manager

        Nexus

        JFrog

    Nexus is one of the most popular

    Upload and store different build artifacts

    Retrieve (download) artifact later

    Central storage

    Private Repository you would use in a company


Public repository Managers


    E.g. Libraries/Frameworks you use as dependencies

        MVN for Java/jar files - Maven Centraol Repository

            Make own project public available

        NPM for JavaScript 

    Nexus

        Host own repositories

        Proxy repository - allows you to fetch everything from Nexus

            Company internal

            Public

            maven repo ==> Maven Central Repository

        Nexus is available as Open source and commercial

        Multiple repositories for different formats

        Repository Formats

        Apt         Composer        Conan       CPAN        Docker
        ELPA        Git LFS         Go          Helm        Maven
        npm         Nuget           P2*         PyPI        RR          RAW
        RubyGems    Yum

        Features of Repository Manager

        Integrate with LDAP

            Configure access management

        Flexible and powerful REST API for integration with other tools

            Why?    not manually managed!

                    build automation CI/CD      Jenkins     GitLab



                                GitLAB CI/CD

            remote git  >>  Tests >>    Build & Package >>  Artifact Repository >>  Deployment
            repository
                ^
                ^
            Merge code
            changes


                    GitLab will execute pipeline (that you have configured)

                    To release new code changes to the end user

        
        
        Need integration with Jenkins and Nexus to push artifacts to Nexus, and in turn fetch those artifacts from remote server

        The REST API sits in the center of the CI/CD process


                                Nexus

        tests   build package   artifact repo   deploy dev  deploy stage    deploy prod

        
        
        
        Integrate with LDAP

        Flexible and powerful REST API for integration with other tools

        Backup and restore - Nexus has its own storage mechanism

        Multiple format support (zip, tar, docker, etc.)

        Metadata tagging (labelling and tagging artifacts)

        Clean-up policies - necessary to limit usage of storage

            automatically delete files that match condition

        Search functionality

            across projects, artifact repos etc.

        User token support for system user authentication


# Usage


    