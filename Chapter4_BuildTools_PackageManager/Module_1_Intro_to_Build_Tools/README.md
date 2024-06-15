# Chapter4
This module we explored the introduction to build tools & Package Manager.

# Name: What are Build and Package Manager Tools

# Description: 

Build and Package Manager Tools

You are on a development team and you are responsible for creating the application and working with the DevOps team to ensure it gets tested deployed to life cycle. App eventually needs to be deployed from a repository and on to a production server.

How do you deploy a software application? The Application Code and its dependencies so it is accessible to it's end users.

Package application into a single movable file, known as an application artifact.
    packaging = building the code into an artifact

Building the code:

    Compiling the code
    Compressing hundreds of files to one single file

The artifact must be stored in a storage location, know as artifact repository.

    Nexus
    JFrog

Every time you release a new version of the application, or you want to make a new version for testers to test before the release, you must create an artifact, save it in the artifact repository and deploy it on the server

To deploy it multiple times, yu must have a backup, just in case dev crashes or the testers need to run locally and test it.

What kind of a file is the artifact?  What kind of format is it?

Artifact file looks different for each programming language.

    Java application will have a jar - java archive or war - web archive file
    Includes whole code plus dependencies
        Spring framework - libraries
        datetime libraries
        pdf process libraries...

    dev     test        production


# Usage

