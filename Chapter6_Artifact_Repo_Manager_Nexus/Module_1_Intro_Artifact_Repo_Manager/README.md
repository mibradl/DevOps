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

            
# Usage


    