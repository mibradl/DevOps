# Chapter8
This module we will explore Pipeline Jobs

# Name: Introduction to Pipeline Job

# Description: 

In this module we will configure a Pipeline.

        New Item: my-pipeline

        Type: Pipeline

        Click Pipeline or scroll to the bottom, to Pipeline Definition.

        Pipelines are configured using scripts:

            Jenkins Pipelines use Groovy

            Programming Language similar to Java, but less complex

        Two options:

            Pipeline script - allows you to write the script directly in the UI

                Sample Hello World

                GitHub + Maven *  we will be using this one

                    Use Groovy Sandbox - called Inline Pipeline Script

                    - Security feature that provides a restricted execution environment

                    - You can execute limited # of Groovy functions, without needing approval from a Jenkins   Admin. Considered very low risk

                    For special libraries would require approval from Jenkins Admin

                Scripted Pipeline


            Pipeline script from SCM - is written from GIT

                Best practice is Infrastructure as Code:

                Pipeline Script in Git Repository

                This should be included in your source code management, together with your application


                Configure Definition - Pipeline script from SCM

                    SCM: Git

                    Repositories

                        Repository URL: https://gitlab.com/drmibradl/java-maven-app.git

                        Credentials:  bradlmi
                    
                    Branches to build

                        Branch Specifier: */jenkins-jobs


                    Script Path: Jenkinsfile

                * Jenkins will search in the root folder for a file called "Jenkinsfile" that configures the pipeline

                save

            
            Pipeline Syntax. It can be one or the other of the following:

                Scripted

                    First syntax

                    Groovy engine

                    Advanced scripting capabilities, high flexibility

                    Difficult to start with, which is why the declarative is available



                Declarative

                    More recent addition 

                    Easier to get start, but not as powerful as scripted

                    There is a predefined structure


                        pipeline {
                            agent any
                            stages {
                                stage("build") {
                                    steps {

                                    }
                                }
                            }
                        }
                        node {
                            // groovy script
                        }

                        Pipeline must be top-level

                        agent - where to execute

                            Relevant for Jenkins cluster

                        stages - where the work happens

                            stage and steps and give it a name - ("build")

                            you can define as many stages as you want


                            Normall it looks something like this


pipeline {
    agent any
    stages {
        stage("build") {
            steps {
                echo "building the application"
                sh 'npm install'
                sh 'npm build'

            }
        }
        stage("test") {
            steps {
                echo "testing the application"
            }
        }
        stage("deploy") {
            steps {
                echo "deploying the application"
            }
        }
    }
}

# Usage


    