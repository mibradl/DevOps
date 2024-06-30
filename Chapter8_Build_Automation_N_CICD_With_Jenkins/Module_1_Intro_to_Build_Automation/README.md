# Chapter8
This module we will introduce the Jenkins Build Automation.

# Name: Introduction to Build Automation?

# Description: 

What is Build  Automation?

    Built application locally

    Built Docker Images

    Pushed to Nexus Repository

        You don't do this on your local machine


        Developer       Git Repo        should be tested and build

    When do you need to do it?

        Stash working changes and move to Master branch - update local state

            Login to correct Docker Repository

                Setup the test environment to run tests

                    Execute tests (meanwhile you CAN't code)

                        Build Docker Image and push to repo

                                                        Publish new Release
    
    Let a dedciated server execute it!

        Developer       Git Repo        should be tested and build

                                    This should be executed automatically
                                                        

        Test environment will be prepared to run the test

        Docker credentials configured

        All necessary tools must be installed to execute

            docker, npm, gradle

        Trigger build automatically

        test code       build application       push to repository      deploy to server

        ** this process is called build automation and there are certain tools that do this.

        One of the tools that does this is Jenkins

    What is Jenkins?

        Software that you install on a dedicated server

        Has UI for configuration

        Install all the tools you need (docker, gradle/maven/npm, etc.)

        Configure the automatic trigger of the workflow


    What can you do with Jenkins?

        Run tests

        Build artifacts

        Publish those artifacts

        Deploy artifacts (dev, staging, production)

        Send notifications that these tasks weere executed


    Integration with other technologies

        Needs to integrate with many other tools

        docker, build tools, nexus, ecr,  repositories, deployment servers

        Uses plugins to make it easy to integrate

    Example

        
        
        Code hosted on Gitlab

            Java App with Gradle

                Build Docker Images

                    Push to Nexus Repo

                        Deploy to AWS EC2

    ** Plugins help you execute each of those steps automatically with Jenkins


    How does it work? What do yo need to configure?

    Run tests  - Build tools need to be available

        With build tools you execute the test commands:
        npm test, gradlew test, mvn test, ...

        Configure test environment (e.g. test database)

    Build Application - Build tools or Docker available

        With Docker you execute Docker commands:
        docker build, ...

        With build tools you build cmds: npm package, gradlew build, ...

    Publish Docker Image - Store Credentials in Jenkins

        To authenticate to a Docker Repository

        Jenkins User must have access to all these technologies and platforms

        Must install Jenkins and prepare everything

        Setup needs to be done only once

        Plugins, credentials etc. can be used for different projects






        


# Usage


    