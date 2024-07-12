# Chapter8
This module we will create a full pipeline with Jenkins.

# Name: Create Complete Pipeline

# Description: 

Complete a full pipeline with Jenkins

    As we did with the Freestyle pipeline, we will create a complete pipeline using maven

    
    Build Java App ---> Build Image ----> Push to private Repo


    Build Java Jar

pipeline {
    agent any
    tools {
        'maven-3.9'
    }
    stages {
        stage("build jar") {
            steps {
                script {
                    echo "build the application..."
                    sh "mvn package"   
               }
            }
        }
    }
...
}

    Build Docker Image - Will require credentials created in Jenkins ID docker-hub-repo

        stage("build image") {
            steps {
                script {
                    echo "build the application..."
                    withCredentials([usernamePassword(credentialsId: 'docker-hub-repo', passwordVariable: 'PASS', usernameVarialbe: 'USER')]) {
                        sh 'docker build -t bradlmi/demo-app:jma-3.0 .'
                        sh 'echo $PASS | docker login -u $USER --password-stdin'
                        sh 'docker push bradlmi/demo-app:jma-3.0'
                     }
                }
            }
        }
The above step will also push the application to docker repo


Push to docker repo using groovy script

def buildJar() {
    echo "building the application..."
    sh "mvn package"
}

def buildImage() {
    echo "building the docker image..."
    withCredentials([usernamePassword(credentialsId: 'docker-hub-repo', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
        sh 'docker build -t bradlmi/demo-app:jma-3.0 .'
        sh 'echo $PASS | docker login -u $USER --password-stdin'
        sh 'docker push bradlmi/demo-app:jma-3.0'
    }
}

def deployApp() {
    echo "deploying the application..."
}
return this



# Usage

Perform Build Jar, Build Image and Push to Docker private repo

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
Checking out Revision a2370aac01d8d4aa4cb61790d32b1533a953234c (refs/remotes/origin/jenkins-pipe)
 > git config core.sparsecheckout # timeout=10
 > git checkout -f a2370aac01d8d4aa4cb61790d32b1533a953234c # timeout=10
Commit message: "correct spelling"
 > git rev-list --no-walk 44f194c21d2939d2367567631a8437ca758a615c # timeout=10
[Pipeline] }
[Pipeline] // stage
[Pipeline] withEnv
[Pipeline] {
[Pipeline] stage
[Pipeline] { (Declarative: Tool Install)
[Pipeline] tool
[Pipeline] envVarsForTool
[Pipeline] }
[Pipeline] // stage
[Pipeline] withEnv
[Pipeline] {
[Pipeline] stage
[Pipeline] { (build jar)
[Pipeline] tool
[Pipeline] envVarsForTool
[Pipeline] withEnv
[Pipeline] {
[Pipeline] script
[Pipeline] {
[Pipeline] echo
build the application...
[Pipeline] sh
+ mvn package
[INFO] Scanning for projects...
[INFO] 
[INFO] ---------------------< com.example:java-maven-app >---------------------
[INFO] Building java-maven-app 1.1.0-SNAPSHOT
[INFO]   from pom.xml
[INFO] --------------------------------[ jar ]---------------------------------
[INFO] 
[INFO] --- resources:3.3.1:resources (default-resources) @ java-maven-app ---
[WARNING] Using platform encoding (UTF-8 actually) to copy filtered resources, i.e. build is platform dependent!
[INFO] Copying 1 resource from src/main/resources to target/classes
[INFO] 
[INFO] --- compiler:3.6.0:compile (default-compile) @ java-maven-app ---
[INFO] Nothing to compile - all classes are up to date
[INFO] 
[INFO] --- resources:3.3.1:testResources (default-testResources) @ java-maven-app ---
[WARNING] Using platform encoding (UTF-8 actually) to copy filtered resources, i.e. build is platform dependent!
[INFO] skip non existing resourceDirectory /var/jenkins_home/workspace/my-pipeline/src/test/resources
[INFO] 
[INFO] --- compiler:3.6.0:testCompile (default-testCompile) @ java-maven-app ---
[INFO] No sources to compile
[INFO] 
[INFO] --- surefire:3.2.5:test (default-test) @ java-maven-app ---
[INFO] No tests to run.
[INFO] 
[INFO] --- jar:3.4.1:jar (default-jar) @ java-maven-app ---
[INFO] 
[INFO] --- spring-boot:2.3.5.RELEASE:repackage (default) @ java-maven-app ---
[INFO] Replacing main artifact with repackaged archive
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  3.208 s
[INFO] Finished at: 2024-07-12T18:11:53Z
[INFO] ------------------------------------------------------------------------
[Pipeline] }
[Pipeline] // script
[Pipeline] }
[Pipeline] // withEnv
[Pipeline] }
[Pipeline] // stage
[Pipeline] stage
[Pipeline] { (build image)
[Pipeline] tool
[Pipeline] envVarsForTool
[Pipeline] withEnv
[Pipeline] {
[Pipeline] script
[Pipeline] {
[Pipeline] echo
build the application...
[Pipeline] withCredentials
Masking supported pattern matches of $PASS
[Pipeline] {
[Pipeline] sh
+ docker build -t bradlmi/demo-app:jma-3.0 .
#0 building with "default" instance using docker driver

#1 [internal] load .dockerignore
#1 transferring context: 2B done
#1 DONE 0.0s

#2 [internal] load build definition from Dockerfile
#2 transferring dockerfile: 216B done
#2 DONE 0.0s

#3 [auth] library/amazoncorretto:pull token for registry-1.docker.io
#3 DONE 0.0s

#4 [internal] load metadata for docker.io/library/amazoncorretto:8-alpine3.17-jre
#4 DONE 0.2s

#5 [1/3] FROM docker.io/library/amazoncorretto:8-alpine3.17-jre@sha256:91f50a87eff28ce56ff7d28c1638775e00baa11e34ff0e6fd2e38cbcbc56b75e
#5 DONE 0.0s

#6 [internal] load build context
#6 transferring context: 17.20MB 0.2s done
#6 DONE 0.3s

#5 [1/3] FROM docker.io/library/amazoncorretto:8-alpine3.17-jre@sha256:91f50a87eff28ce56ff7d28c1638775e00baa11e34ff0e6fd2e38cbcbc56b75e
#5 CACHED

#7 [2/3] COPY ./target/java-maven-app-*.jar /usr/app/
#7 DONE 0.1s

#8 [3/3] WORKDIR /usr/app
#8 DONE 0.0s

#9 exporting to image
#9 exporting layers
#9 exporting layers 0.2s done
#9 writing image sha256:97b5717517b49af2530e4c463a62b06d8df5169b0be2afd61aa49ed20b6dfb8e done
#9 naming to docker.io/bradlmi/demo-app:jma-3.0 done
#9 DONE 0.2s
[Pipeline] sh
+ docker login -u bradlmi --password-stdin
+ echo ****
WARNING! Your password will be stored unencrypted in /var/jenkins_home/.docker/config.json.
Configure a credential helper to remove this warning. See
https://docs.docker.com/engine/reference/commandline/login/#credential-stores

Login Succeeded
[Pipeline] sh
+ docker push bradlmi/demo-app:jma-3.0
The push refers to repository [docker.io/bradlmi/demo-app]
5f70bf18a086: Preparing
c8a659a137e0: Preparing
5014d7f17bcd: Preparing
b2a6aa582d9a: Preparing
5f70bf18a086: Layer already exists
b2a6aa582d9a: Layer already exists
5014d7f17bcd: Mounted from library/amazoncorretto
c8a659a137e0: Pushed
jma-3.0: digest: sha256:9ab8f59547abbbf2d517574f3cb3968cf8487829fb2d0a9d19ce977b83be995b size: 1158
[Pipeline] }
[Pipeline] // withCredentials
[Pipeline] }
[Pipeline] // script
[Pipeline] }
[Pipeline] // withEnv
[Pipeline] }
[Pipeline] // stage
[Pipeline] stage
[Pipeline] { (deploy)
[Pipeline] tool
[Pipeline] envVarsForTool
[Pipeline] withEnv
[Pipeline] {
[Pipeline] script
[Pipeline] {
[Pipeline] echo
deploying the application
[Pipeline] }
[Pipeline] // script
[Pipeline] }
[Pipeline] // withEnv
[Pipeline] }
[Pipeline] // stage
[Pipeline] }
[Pipeline] // withEnv
[Pipeline] }
[Pipeline] // withEnv
[Pipeline] }
[Pipeline] // node
[Pipeline] End of Pipeline
Finished: SUCCESS

Docker Hub Private Repo

TAG

jma-3.0

Last pushed 3 minutes ago by bradlmi

docker pull bradlmi/demo-app:jma-3.0
Digest	OS/ARCH	Last pull
Compressed Size
9ab8f59547ab


Push to docker repo using script.groovy

Stage 'build image'

Started 14 sec ago
Queued 0 ms
Took 1.9 sec
Success
View as plain text
maven-3.9
Use a tool from a predefined Tool Installation
24 ms
Fetches the environment variables for a given tool in a list of 'FOO=bar' strings suitable for the withEnv step.
28 ms
building the docker image...
Print Message
21 ms
docker build -t bradlmi/demo-app:jma-3.0 .
Shell Script
0.78 sec
echo $PASS | docker login -u $USER --password-stdin
Shell Script
0.28 sec
docker push bradlmi/demo-app:jma-3.0
Shell Script
0.52 sec
+ docker push bradlmi/demo-app:jma-3.0
The push refers to repository [docker.io/bradlmi/demo-app]
5f70bf18a086: Preparing
c8a659a137e0: Preparing
5014d7f17bcd: Preparing
b2a6aa582d9a: Preparing
b2a6aa582d9a: Layer already exists
5f70bf18a086: Layer already exists
5014d7f17bcd: Layer already exists
c8a659a137e0: Layer already exists
jma-3.0: digest: sha256:9ab8f59547abbbf2d517574f3cb3968cf8487829fb2d0a9d19ce977b83be995b size: 1158