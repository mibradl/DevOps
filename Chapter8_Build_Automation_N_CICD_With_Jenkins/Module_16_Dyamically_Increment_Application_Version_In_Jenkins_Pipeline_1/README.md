# Chapter8
This module we will learn how to version your application.

# Name: Dynamically Increment Application Version in Jenkins Pipeline - Part 1

# Description: 

Versioning your Application


# Usage


General concepts of versioning - Overview

    Gradle, Maven, NPM

    Demo: Java Maven App


Versioning Explanation

    Increment Application Version

        Each build tool or package manager tool keeps a version

        Gradle - build.gradle

        NPM - package.json

        Maven - pom.xml

        Software Versioning

            <version>1.1.0-SNAPSHOT</version>

        You decide how you version your application

        How to increase the version?

        Three parts of a version

        Major   Minor   Patch

            Major version - Contains big changes, breaking changes, NOT backward-compatible (Applies to major apps - facebook, Linkend, JavaScript, etc)

            Minor version - new, but backward-compatible changes, API features

            Patch (incremental) - minor changes and bug fixes, doesn't change API

                <version>1.1.0-SNAPSHOT</version>

                <version>2.3.5-RELEASE</version>

                -SNAPSHOT or -RELEASE is a suffix - More information used in development

            You need to increment the version in e.g. pom.xml file

            
            Automatically increase version inside the build automation

                Jenkins Pipeline

            Build Tools have commands to increcment the version



            Increment version locally

                JAVA MAVEN Project

                mvn build-helper:parse-version - is part of the pom.xml plugin, which finds the version. New version must be based on the old.

                Replaces pom.xml with new version

                Incremental Version

                 mvn build-helper:parse-version version:set \
                 -DnewVersion=\${parsedVersion.majorVersion}.\${parsedVersion.minorVersion}.\${parsedVersion.nextIncrementalVersion} versions:commit

                 Minor Version

                 mvn build-helper:parse-version version:set \
                 -DnewVersion=\${parsedVersion.majorVersion}.\${parsedVersion.nextMinorVersion}.\${parsedVersion.incrementalVersion} versions:commit

                 Major Version

                 mvn build-helper:parse-version version:set \
                 -DnewVersion=\${parsedVersion.nextMajorVersion}.\${parsedVersion.minorVersion}.\${parsedVersion.incrementalVersion} versions:commit

            Same concept for different package managers/build tools

                NPM

                Read version from package.json, increment and save back

                Different ways of doing the version upgrade

            Realistically, this should be done on the CI/CD Pipeline


    Next, we will perform this in Jenkins

                Increment Version   --->    Build App   --->    Build Image ---> Push to Docker Repo

            Implement Application version on Jenkins, using JAVA MAVEN Project

            mvn package = builds jar file with version from pom.xml

            
            Add increment version stage before 'mvn package' build app, in Jenkinsfile

        stage("increment version") {
            steps {
                script {
                    echo 'incrementing app version...'
                    sh 'mvn build-helper:parse-version version:set \
                        -DnewVersion=\\\${parsedVersion.majorVersion}.\\\${parsedVersion.minorVersion}.\\\${parsedVersion.nextIncrementalVersion} \
                        versions:commit'   
                }
            }
        }


    Docker Image Version

        Docker Image is released

    Use Application version for Docker Image version

            Add the build, login and push to build docker image

        stage("build image") {
            steps {
                script {
                    echo 'building the docker image'
                    withCredentials([usernamePassword(credentialsId: 'docker-hub-repo', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                            sh 'docker build -t bradlmi/demo-app:${IMAGE_NAME} .'
                            sh 'echo $PASS | docker login -u $USER --password-stdin'
                            sh 'docker push bradlmi/demo-app:${IMAGE_NAME}'
                    }        
                }
            }
        }


        

















