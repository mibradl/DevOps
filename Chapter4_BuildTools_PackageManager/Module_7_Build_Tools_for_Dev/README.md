# Chapter4
This module we reviewed how to build tools in Dev.

# Name: Build Tools for Development (Managing Dependencies)

# Description: 


Managing Dependencies

You also need the build tools locally when developing the App

    Run App locally
    Need to run tests

    Maven & Gradle use Dependencies file

    Maven uses pom.xml and dependency block

    Gradle uses build.gradle fil and dependencies block

    Dependencies file = managing the dependencies for a project

Dependencies come from their respective repositories (Maven and Gradle use the same remote repo)

    MVN Repository


How does it work?

    Need a library for ElasticSearch with name and version

    1. Find a dependency with name and version
    
    2. You add it to the dependencies file (e.g. pom.xml)

    3. Dependency gets downloaded locally (e.g. local maven repo)


Install Dependencies

    When we execute the build CLI it automatically loads dependencies

    
# Usage

  