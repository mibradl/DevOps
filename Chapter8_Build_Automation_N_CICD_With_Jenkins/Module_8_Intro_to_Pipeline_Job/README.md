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

Started by user michael
Obtained Jenkinsfile from git https://gitlab.com/drmibradl/java-maven-app.git
[Pipeline] Start of Pipeline
[Pipeline] node
Running on Jenkins in /var/jenkins_home/workspace/my-pipeline
[Pipeline] {
[Pipeline] stage
[Pipeline] { (Declarative: Checkout SCM)
[Pipeline] checkout
The recommended git tool is: git
using credential docker-hub-repo
 > git rev-parse --resolve-git-dir /var/jenkins_home/workspace/my-pipeline/.git # timeout=10
Fetching changes from the remote Git repository
 > git config remote.origin.url https://gitlab.com/drmibradl/java-maven-app.git # timeout=10
Fetching upstream changes from https://gitlab.com/drmibradl/java-maven-app.git
 > git --version # timeout=10
 > git --version # 'git version 2.39.2'
using GIT_ASKPASS to set credentials 
 > git fetch --tags --force --progress -- https://gitlab.com/drmibradl/java-maven-app.git +refs/heads/*:refs/remotes/origin/* # timeout=10
 > git rev-parse refs/remotes/origin/jenkins-pipe^{commit} # timeout=10
Checking out Revision f94be2740fbb5a8d0bc06e5eea3577783b7dfbf1 (refs/remotes/origin/jenkins-pipe)
 > git config core.sparsecheckout # timeout=10
 > git checkout -f f94be2740fbb5a8d0bc06e5eea3577783b7dfbf1 # timeout=10
Commit message: "add new Jenkinsfile"
 > git rev-list --no-walk f94be2740fbb5a8d0bc06e5eea3577783b7dfbf1 # timeout=10
[Pipeline] }
[Pipeline] // stage
[Pipeline] withEnv
[Pipeline] {
[Pipeline] stage
[Pipeline] { (build)
[Pipeline] echo
building the application
[Pipeline] }
[Pipeline] // stage
[Pipeline] stage
[Pipeline] { (test)
[Pipeline] echo
testing the application
[Pipeline] }
[Pipeline] // stage
[Pipeline] stage
[Pipeline] { (deploy)
[Pipeline] echo
deploying the application
[Pipeline] }
[Pipeline] // stage
[Pipeline] }
[Pipeline] // withEnv
[Pipeline] }
[Pipeline] // node
[Pipeline] End of Pipeline
Finished: SUCCESS
    