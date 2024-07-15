# Chapter8
This module we will explore Jenkins Shared Library

# Name: Jenkins Shared Library

# Description: 

What is Jenkins Shared Library? Why do we need it?

Micro-svc A
Micro-svc B
Micro-svc C     ------->        Application
Micro-svc D
Micro-svc...

Java Maven

    Uses micro-services, which are similar in nature

    Each of the micro services are an application and each would have it's own pipeline.

    Multiple projects that all have their own Jenkinsfile, that are building a pipeline in Jenkins.

    Comprised of the same logic in those Jenkinsfiles.

    Would need to copy same logic to all. So, what happens when you have to change somethingß


    Solution

    Jenkins Shared Library

        Extension to the Pipeline

        Has own respository

        Written in groovy

    Another Use Case for Shared Library

        multiple projects

        may not use the same tech stack, but some logic is the same

        E.g. company-wide Nexus Repository

        E.g. company-wide Slack channel

        Share Pipeline Code


    Benefits of Jenkins Shared Library

        Code Reusability

        Consistency and Standardization

        Easy maintenance
        
        Faster pipeline creation

        Improved collaboration

    
    In this module:

        Create the Shared Library (SL)

        Make SL available in Jenkins

        Use the SL in Jenkinsfile

    
    Create Shared Library Project/Repository

        Create Repository

        Write the Groovy Code

        Make the Shared Library available globally or for a specific project

        Use the Share Library in Jenkinsfile to extend the Pipeline


    Open new project in Intellij

        Select Groovy for language type

        Name Project: jenkins-shared-library

        create in new window


    Structure of Shared Library

        vars folder

        Folder contains all the functions that we call from Jenkinsfile

        Each function/execution step is its own Groovy file

            build Jar File

            build Docker Image

            push Docker Image

        src folder

            Helper code


        resources folder

            Use external libraries

            Non groovy files (sql scripts, powershell, shell scripts, json files, etc)


    Create a vars folder under jenkins-shared-library

        From vars directory

            create buildJar.groovy file - to contain function from java-maven-app repo

            create buildImage.groovy file with the function from same

    Create a new repository

        New project in Gitlab

            Create blank project

            Project name: jenkins-shared-library

            Select public for visibility

            Deselect Readme.md

            Create project


    Initialize jenkins-shared-libary (from Intellij)

        cd /Users/michaelbadley/new-projects/jenkins-shared-library

        git init - initialize project locally

            Initialized empty Git repository in /Users/michaelbradley/new-project/jenkins-shared-library/.git/

        connect local and remote repositories

            git remote add origin git@gitlab.com:drmibradl/jenkins-shared-library.git

            git add .

            git commit -m "initial commit"

            git push -u origin master

        Remote Gitlab

        Name	                    Last commit	    Last update
        .idea	                    Initial commit  4 minutes ago
        lib	                        Initial commit  4 minutes ago
        src	                        Initial commit  4 minutes ago
        vars	                    Initial commit  4 minutes ago
        .gitignore	                Initial commit  4 minutes ago
        jenkins-shared-library.iml	Initial commit  4 minutes ago


Make Shared Library globally available

        From Jenkins

        Manage Jenkins 

        System - configure global settings and path

        Global Pipeline Libraries (trusted)

            Add

            Library Name: jenkins-shared-library

            Default version: master

            Allow default version to be overridden - CHECK

            Retrieval method
                
                Modern SCM

                Git

                Project Repository: https://gitlab.com/drmibradl/jenkins-shared-library.git

                Credentials: mibradl@hotmail.com/******

        
        Use Shared Library in Jenkins

            Create New Branch in gitlab

            Select New Branch

            Branch name: jenkins-shared-lib

            Create branch


        Create multibranch pipeline, using java-maven-app endpoint.

        Perform multibranch scan from jenkins-shared-lib branch

        Check console output

        [Pipeline] sh
+ echo ****
+ docker login -u bradlmi --password-stdin
WARNING! Your password will be stored unencrypted in /var/jenkins_home/.docker/config.json.
Configure a credential helper to remove this warning. See
https://docs.docker.com/engine/reference/commandline/login/#credential-stores

Login Succeeded
[Pipeline] sh
+ docker push bradlmi/demo-app:jma-3.0
The push refers to repository [docker.io/bradlmi/demo-app]
5f70bf18a086: Preparing
5eb30fbcef3f: Preparing
5014d7f17bcd: Preparing
b2a6aa582d9a: Preparing
5014d7f17bcd: Layer already exists
5f70bf18a086: Layer already exists
b2a6aa582d9a: Layer already exists
5eb30fbcef3f: Pushed
jma-3.0: digest: sha256:1fc3097d4940c2c9198ddc27e81953a2747343f7444d1110ab861621e5c9f306 size: 1158
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
deploying the application...
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

Using Parameters in Shared Library

    Add variable param for the build - buildImage.groovy

    #!/user/bin/env groovy

    def call(String imageName) {
        echo "building the docker image..."
        withCredentials([usernamePassword(credentialsId: 'docker-hub-repo', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
            sh "docker build -t $imageName ."
            sh 'echo $PASS | docker login -u $USER --password-stdin'
            sh "docker push $imageName"
        }
    }


    Add variable value to the build method in jenkinsfile, with version tag 4.0

    stage("build image") {
            steps {
                script {
                    buildImage 'bradlmi/demo-app:jma-4.0'
                }
            }
    }



    Perform buid of build jar

    Stage 'build jar'

    Started 24 sec ago
    Queued 0 ms
    Took 5.2 sec
    Success
    View as plain text
    maven-3.9
    Use a tool from a predefined Tool Installation
    26 ms
    Fetches the environment variables for a given tool in a list of 'FOO=bar' strings suitable for the withEnv step.
    32 ms
    building the application for branch jenkins-shared-lib
    Print Message
    13 ms
    mvn package
    Shell Script
    4.9 sec
    + mvn package
    [INFO] Scanning for projects...
    [INFO] 
    [INFO] ---------------------< com.example:java-maven-app >---------------------
    [INFO] Building java-maven-app 1.1.0-SNAPSHOT
    [INFO]   from pom.xml
    [INFO] --------------------------------[ jar ]---------------------------------
    [INFO] 


    Results of build for java-maven-app

    Stage 'build image'

    Started 19 sec ago
    Queued 0 ms
    Took 2.6 sec
    Success
    View as plain text
    maven-3.9
    Use a tool from a predefined Tool Installation
    58 ms
    Fetches the environment variables for a given tool in a list of 'FOO=bar' strings suitable for the withEnv step.
    54 ms
    building the docker image...
    Print Message
    23 ms
    docker build -t bradlmi/demo-app:jma-4.0 .
    Shell Script
    1.3 sec
    echo $PASS | docker login -u $USER --password-stdin
    Shell Script
    0.28 sec
    docker push bradlmi/demo-app:jma-4.0
    Shell Script
    0.52 sec
    + docker push bradlmi/demo-app:jma-4.0
    The push refers to repository [docker.io/bradlmi/demo-app]
    5f70bf18a086: Preparing
    5eb30fbcef3f: Preparing
    5014d7f17bcd: Preparing
    b2a6aa582d9a: Preparing
    5f70bf18a086: Layer already exists
    b2a6aa582d9a: Layer already exists
    5eb30fbcef3f: Layer already exists
    5014d7f17bcd: Layer already exists
    jma-4.0: digest: sha256:1fc3097d4940c2c9198ddc27e81953a2747343f7444d1110ab861621e5c9f306 size: 1158


    Results of push to docker private repo

    TAG

    jma-4.0

    Last pushed 6 minutes ago by bradlmi

    docker pull bradlmi/demo-app:jma-4.0
    Digest	OS/ARCH	Last pull
    Compressed Size
    1fc3097d4940

    linux/amd64

    ---

    57.62 MB


Extract logic into groovy classes

    From the jenkins-shared-library local branch

    From the src directory

        create new package name com.example

        create new class from the com.example directory


        #!/user/bin/env groovy

        import com.example.Docker

        def call(String imageName) {
            return new Docker(this).buildDockerImage(imageName)
        }


        create Docker.groovy file with serializable to ensure persistence in case pipeline paused

        #!/user/bin/env groovy
        package com.example

        class Docker implements Serializable {

            def script

            Docker (script) {
                this.script = script
            }

            def buildDockerImage(String imageName) {
                script.echo "building the docker image..."
                script.withCredentials([script.usernamePassword(credentialsId: 'docker-hub-repo', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                    script.sh "docker build -t $imageName ."
                    script.sh "echo '${script.PASS}' | docker login -u '${script.USER}' --password-stdin"
                    script.sh "docker push $imageName"
                }
            }
        }



Split "buildDockerImage" into separate steps

Create three different functions

        1. Build

        def buildDockerImage(String imageName) {
            script.echo "building the docker image..."
            script.sh "docker build -t $imageName ."
        }

        2. Login

        def dockerLogin() {
            script.withCredentials([script.usernamePassword(credentialsId: 'docker-hub-repo', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                script.sh "echo '${script.PASS}' | docker login -u '${script.USER}' --password-stdin"
            }
        }

        3. Push

        def dockerPush(String imageName) {
            script.sh "docker push $imageName"
        }

    Add vars and separate groovy files

        dockerLogin

        dockerPush


    Separate steps in Jenkinsfile

    stage("build and push image") {
            steps {
                script {
                    buildImage 'bradlmi/demo-app:jma-4.0'
                    dockerLogin()
                    dockerPush 'bradlmi/demo-app:jma-4.0'
                }
            }
        }

    Results

    building the docker image...
    Print Message
    13 ms
    docker build -t bradlmi/demo-app:jma-4.0 .
    Shell Script
    0.82 sec
    echo '${PASS}' | docker login -u 'bradlmi' --password-stdin
    Shell Script
    0.29 sec
    docker push bradlmi/demo-app:jma-4.0
    Shell Script


Project scoped Shared Library

    Dashboard ---> Manage Jenkins ---> System

    Delete Global Pipeline Library

    Save

    
    In the Jenkinsfile the @Library statement will no longer work, because it was previously deleted in Jenkins.

    Delete this line - @Library('jenkins-shared-library') and replace with library method


    Jenkinsfile was configured to include shared library scoped at the project level and demonstrates same resources are accessible.

        #!/usr/bin/env groovy

        library identifier: 'jenkins-shared-library@master', retriever: modernSCM(
                [$class: 'GitSCMSource',
                remote: 'https://gitlab.com/drmibradl/jenkins-shared-library.git',
                credentialsId: 'gitlab-credentials'])




# Usage


 % git init
hint: Using 'master' as the name for the initial branch. This default branch name
hint: is subject to change. To configure the initial branch name to use in all
hint: of your new repositories, which will suppress this warning, call:
hint:
hint:   git config --global init.defaultBranch <name>
hint:
hint: Names commonly chosen instead of 'master' are 'main', 'trunk' and
hint: 'development'. The just-created branch can be renamed via this command:
hint:
hint:   git branch -m <name>
Initialized empty Git repository in /Users/michaelbradley/new-project/jenkins-shared-library/.git/
   

   Check Console Output for Multibranch build

 > git fetch --no-tags --force --progress -- https://gitlab.com/drmibradl/jenkins-shared-library.git +refs/heads/*:refs/remotes/origin/* # timeout=10
Checking out Revision 5db0b30534883d47e470cf1ad43b5a670816ee11 (master)
 > git config core.sparsecheckout # timeout=10
 > git checkout -f 5db0b30534883d47e470cf1ad43b5a670816ee11 # timeout=10
Commit message: "Initial commit"
 > git rev-list --no-walk 5db0b30534883d47e470cf1ad43b5a670816ee11 # timeout=10
[Pipeline] Start of Pipeline
[Pipeline] node
Running on Jenkins in /var/jenkins_home/workspace/ava-maven-app_jenkins-shared-lib
[Pipeline] {
[Pipeline] stage
[Pipeline] { (Declarative: Checkout SCM)
[Pipeline] checkout
The recommended git tool is: git
using credential 6fafcc34-bd15-4440-8151-b68c1754898a
Cloning the remote Git repository
Cloning with configured refspecs honoured and without tags
Cloning repository https://gitlab.com/drmibradl/java-maven-app.git
 > git init /var/jenkins_home/workspace/ava-maven-app_jenkins-shared-lib # timeout=10
Fetching upstream changes from https://gitlab.com/drmibradl/java-maven-app.git
 > git --version # timeout=10
 > git --version # 'git version 2.39.2'
using GIT_ASKPASS to set credentials 
 > git fetch --no-tags --force --progress -- https://gitlab.com/drmibradl/java-maven-app.git +refs/heads/*:refs/remotes/origin/* # timeout=10
 > git config remote.origin.url https://gitlab.com/drmibradl/java-maven-app.git # timeout=10
 > git config --add remote.origin.fetch +refs/heads/*:refs/remotes/origin/* # timeout=10
Avoid second fetch
Checking out Revision fe0612c3a65f429b488820973b0955896a3a1bfe (jenkins-shared-lib)
 > git config core.sparsecheckout # timeout=10
 > git checkout -f fe0612c3a65f429b488820973b0955896a3a1bfe # timeout=10
Commit message: "correct maven syntax in jenkinsfile"
First time build. Skipping changelog.
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
[Pipeline] { (init)
[Pipeline] tool
[Pipeline] envVarsForTool
[Pipeline] withEnv
[Pipeline] {
[Pipeline] script
[Pipeline] {
[Pipeline] load
[Pipeline] { (script.groovy)
[Pipeline] }
[Pipeline] // load
[Pipeline] }
[Pipeline] // script
[Pipeline] }
[Pipeline] // withEnv
[Pipeline] }
[Pipeline] // stage
[Pipeline] stage
[Pipeline] { (build jar)
[Pipeline] tool
[Pipeline] envVarsForTool
[Pipeline] withEnv
[Pipeline] {
[Pipeline] script
[Pipeline] {
[Pipeline] echo
building the application for branch jenkins-shared-lib
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
[INFO] Changes detected - recompiling the module!
[WARNING] File encoding has not been set, using platform encoding UTF-8, i.e. build is platform dependent!
[INFO] Compiling 1 source file to /var/jenkins_home/workspace/ava-maven-app_jenkins-shared-lib/target/classes
[INFO] 
[INFO] --- resources:3.3.1:testResources (default-testResources) @ java-maven-app ---
[WARNING] Using platform encoding (UTF-8 actually) to copy filtered resources, i.e. build is platform dependent!
[INFO] skip non existing resourceDirectory /var/jenkins_home/workspace/ava-maven-app_jenkins-shared-lib/src/test/resources
[INFO] 
[INFO] --- compiler:3.6.0:testCompile (default-testCompile) @ java-maven-app ---
[INFO] No sources to compile
[INFO] 
[INFO] --- surefire:3.2.5:test (default-test) @ java-maven-app ---
[INFO] No tests to run.
[INFO] 
[INFO] --- jar:3.4.1:jar (default-jar) @ java-maven-app ---
[INFO] Building jar: /var/jenkins_home/workspace/ava-maven-app_jenkins-shared-lib/target/java-maven-app-1.1.0-SNAPSHOT.jar
[INFO] 
[INFO] --- spring-boot:2.3.5.RELEASE:repackage (default) @ java-maven-app ---
[INFO] Replacing main artifact with repackaged archive
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  10.667 s
[INFO] Finished at: 2024-07-14T17:24:14Z
[INFO] ------------------------------------------------------------------------
[Pipeline] }
[Pipeline] // script
[Pipeline] }
[Pipeline] // withEnv
[Pipeline] }
[Pipeline] // stage
[Pipeline] stage
[Pipeline] { (build and push image)
[Pipeline] tool
[Pipeline] envVarsForTool
[Pipeline] withEnv
[Pipeline] {
[Pipeline] script
[Pipeline] {
[Pipeline] echo
building the docker image...
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
#6 transferring context: 17.20MB 0.4s done
#6 DONE 0.4s

#5 [1/3] FROM docker.io/library/amazoncorretto:8-alpine3.17-jre@sha256:91f50a87eff28ce56ff7d28c1638775e00baa11e34ff0e6fd2e38cbcbc56b75e
#5 CACHED

#7 [2/3] COPY ./target/java-maven-app-*.jar /usr/app/
#7 DONE 0.2s

#8 [3/3] WORKDIR /usr/app
#8 DONE 0.0s

#9 exporting to image
#9 exporting layers 0.1s done
#9 writing image sha256:e749ff38d3fdfe6a4ca465d06d3d3e25882f6186fefbbca15326cc002a82cd81 done
#9 naming to docker.io/bradlmi/demo-app:jma-3.0 done
#9 DONE 0.2s
[Pipeline] sh
+ echo ****
+ docker login -u bradlmi --password-stdin
WARNING! Your password will be stored unencrypted in /var/jenkins_home/.docker/config.json.
Configure a credential helper to remove this warning. See
https://docs.docker.com/engine/reference/commandline/login/#credential-stores

Login Succeeded
[Pipeline] sh
+ docker push bradlmi/demo-app:jma-3.0
The push refers to repository [docker.io/bradlmi/demo-app]
5f70bf18a086: Preparing
5eb30fbcef3f: Preparing
5014d7f17bcd: Preparing
b2a6aa582d9a: Preparing
5014d7f17bcd: Layer already exists
5f70bf18a086: Layer already exists
b2a6aa582d9a: Layer already exists
5eb30fbcef3f: Pushed
jma-3.0: digest: sha256:1fc3097d4940c2c9198ddc27e81953a2747343f7444d1110ab861621e5c9f306 size: 1158
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
deploying the application...
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


Split into Separate Steps

building the docker image...
Print Message
13 ms
building the docker image...
docker build -t bradlmi/demo-app:jma-4.0 .
Shell Script
0.82 sec
+ docker build -t bradlmi/demo-app:jma-4.0 .
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
#6 transferring context: 90B done
#6 DONE 0.0s
#7 [2/3] COPY ./target/java-maven-app-*.jar /usr/app/
#7 CACHED
#8 [3/3] WORKDIR /usr/app
#8 CACHED
#9 exporting to image
#9 exporting layers done
#9 writing image sha256:e749ff38d3fdfe6a4ca465d06d3d3e25882f6186fefbbca15326cc002a82cd81 done
#9 naming to docker.io/bradlmi/demo-app:jma-4.0 done
#9 DONE 0.0s
echo '${PASS}' | docker login -u 'bradlmi' --password-stdin
Shell Script
0.29 sec
Warning: A secret was passed to "sh" using Groovy String interpolation, which is insecure.
		 Affected argument(s) used the following variable(s): [PASS]
		 See https://jenkins.io/redirect/groovy-string-interpolation for details.
+ echo ****
+ docker login -u bradlmi --password-stdin
WARNING! Your password will be stored unencrypted in /var/jenkins_home/.docker/config.json.
Configure a credential helper to remove this warning. See
https://docs.docker.com/engine/reference/commandline/login/#credential-stores
Login Succeeded
docker push bradlmi/demo-app:jma-4.0
Shell Script
0.52 sec
+ docker push bradlmi/demo-app:jma-4.0
The push refers to repository [docker.io/bradlmi/demo-app]
5f70bf18a086: Preparing
5eb30fbcef3f: Preparing
5014d7f17bcd: Preparing
b2a6aa582d9a: Preparing
b2a6aa582d9a: Layer already exists
5014d7f17bcd: Layer already exists
5f70bf18a086: Layer already exists
5eb30fbcef3f: Layer already exists
jma-4.0: digest: sha256:1fc3097d4940c2c9198ddc27e81953a2747343f7444d1110ab861621e5c9f306 size: 1158


Scoping Shared Library on project level

Started by user michael
 > git rev-parse --resolve-git-dir /var/jenkins_home/caches/git-5cad553192de71d9d989926c524f1ea6/.git # timeout=10
Setting origin to https://gitlab.com/drmibradl/java-maven-app.git
 > git config remote.origin.url https://gitlab.com/drmibradl/java-maven-app.git # timeout=10
Fetching origin...
Fetching upstream changes from origin
 > git --version # timeout=10
 > git --version # 'git version 2.39.2'
 > git config --get remote.origin.url # timeout=10
using GIT_ASKPASS to set credentials 
 > git fetch --tags --force --progress -- origin +refs/heads/*:refs/remotes/origin/* # timeout=10
Seen branch in repository origin/feature/payment
Seen branch in repository origin/jenkins-jobs
Seen branch in repository origin/jenkins-pipe
Seen branch in repository origin/jenkins-shared-lib
Seen branch in repository origin/master
Seen branch in repository origin/starting-code
Seen 6 remote branches
Obtained Jenkinsfile from 194cd966498882472b692db97387394610b8e4e6
[Pipeline] Start of Pipeline
[Pipeline] library
Loading library jenkins-shared-library@master
Attempting to resolve master from remote references...
 > git --version # timeout=10
 > git --version # 'git version 2.39.2'
 > git ls-remote -- https://gitlab.com/drmibradl/jenkins-shared-library.git # timeout=10
Found match: refs/heads/master revision 2cb6d42911165440d78306505bfff72267635f16
The recommended git tool is: NONE
Warning: CredentialId "gitlab-credentials" could not be found.
 > git rev-parse --resolve-git-dir /var/jenkins_home/workspace/ava-maven-app_jenkins-shared-lib@libs/6f8eef6a0257cf201e7ff69679e1215f07b561cf2db734ee5d3d03d8631920f5/.git # timeout=10
Fetching changes from the remote Git repository
 > git config remote.origin.url https://gitlab.com/drmibradl/jenkins-shared-library.git # timeout=10
Fetching without tags
Fetching upstream changes from https://gitlab.com/drmibradl/jenkins-shared-library.git
 > git --version # timeout=10
 > git --version # 'git version 2.39.2'
 > git fetch --no-tags --force --progress -- https://gitlab.com/drmibradl/jenkins-shared-library.git +refs/heads/*:refs/remotes/origin/* # timeout=10
Checking out Revision 2cb6d42911165440d78306505bfff72267635f16 (master)
 > git config core.sparsecheckout # timeout=10
 > git checkout -f 2cb6d42911165440d78306505bfff72267635f16 # timeout=10
Commit message: "correct name of buildDockerImage to match function"
 > git rev-list --no-walk 2cb6d42911165440d78306505bfff72267635f16 # timeout=10
[Pipeline] node
Running on Jenkins in /var/jenkins_home/workspace/ava-maven-app_jenkins-shared-lib
[Pipeline] {
[Pipeline] stage
[Pipeline] { (Declarative: Checkout SCM)
[Pipeline] checkout
The recommended git tool is: git
using credential 6fafcc34-bd15-4440-8151-b68c1754898a
 > git rev-parse --resolve-git-dir /var/jenkins_home/workspace/ava-maven-app_jenkins-shared-lib/.git # timeout=10
Fetching changes from the remote Git repository
 > git config remote.origin.url https://gitlab.com/drmibradl/java-maven-app.git # timeout=10
Fetching without tags
Fetching upstream changes from https://gitlab.com/drmibradl/java-maven-app.git
 > git --version # timeout=10
 > git --version # 'git version 2.39.2'
using GIT_ASKPASS to set credentials 
 > git fetch --no-tags --force --progress -- https://gitlab.com/drmibradl/java-maven-app.git +refs/heads/*:refs/remotes/origin/* # timeout=10
Checking out Revision 194cd966498882472b692db97387394610b8e4e6 (jenkins-shared-lib)
 > git config core.sparsecheckout # timeout=10
 > git checkout -f 194cd966498882472b692db97387394610b8e4e6 # timeout=10
Commit message: "corrected repo definition"
 > git rev-list --no-walk 36509e5bed08884e23023bef0861145dc6ad8ca5 # timeout=10
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
[Pipeline] { (init)
[Pipeline] tool
[Pipeline] envVarsForTool
[Pipeline] withEnv
[Pipeline] {
[Pipeline] script
[Pipeline] {
[Pipeline] load
[Pipeline] { (script.groovy)
[Pipeline] }
[Pipeline] // load
[Pipeline] }
[Pipeline] // script
[Pipeline] }
[Pipeline] // withEnv
[Pipeline] }
[Pipeline] // stage
[Pipeline] stage
[Pipeline] { (build jar)
[Pipeline] tool
[Pipeline] envVarsForTool
[Pipeline] withEnv
[Pipeline] {
[Pipeline] script
[Pipeline] {
[Pipeline] echo
building the application for branch jenkins-shared-lib
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
[INFO] skip non existing resourceDirectory /var/jenkins_home/workspace/ava-maven-app_jenkins-shared-lib/src/test/resources
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
[INFO] Total time:  2.709 s
[INFO] Finished at: 2024-07-15T20:41:02Z
[INFO] ------------------------------------------------------------------------
[Pipeline] }
[Pipeline] // script
[Pipeline] }
[Pipeline] // withEnv
[Pipeline] }
[Pipeline] // stage
[Pipeline] stage
[Pipeline] { (build and push image)
[Pipeline] tool
[Pipeline] envVarsForTool
[Pipeline] withEnv
[Pipeline] {
[Pipeline] script
[Pipeline] {
[Pipeline] echo
building the docker image...
[Pipeline] sh
+ docker build -t bradlmi/demo-app:jma-4.0 .
#0 building with "default" instance using docker driver

#1 [internal] load build definition from Dockerfile
#1 transferring dockerfile: 216B done
#1 DONE 0.0s

#2 [internal] load .dockerignore
#2 transferring context: 2B done
#2 DONE 0.0s

#3 [auth] library/amazoncorretto:pull token for registry-1.docker.io
#3 DONE 0.0s

#4 [internal] load metadata for docker.io/library/amazoncorretto:8-alpine3.17-jre
#4 DONE 0.2s

#5 [1/3] FROM docker.io/library/amazoncorretto:8-alpine3.17-jre@sha256:91f50a87eff28ce56ff7d28c1638775e00baa11e34ff0e6fd2e38cbcbc56b75e
#5 DONE 0.0s

#6 [internal] load build context
#6 transferring context: 90B done
#6 DONE 0.0s

#7 [2/3] COPY ./target/java-maven-app-*.jar /usr/app/
#7 CACHED

#8 [3/3] WORKDIR /usr/app
#8 CACHED

#9 exporting to image
#9 exporting layers done
#9 writing image sha256:e749ff38d3fdfe6a4ca465d06d3d3e25882f6186fefbbca15326cc002a82cd81 done
#9 naming to docker.io/bradlmi/demo-app:jma-4.0 done
#9 DONE 0.0s
[Pipeline] withCredentials
Masking supported pattern matches of $PASS
[Pipeline] {
[Pipeline] sh
Warning: A secret was passed to "sh" using Groovy String interpolation, which is insecure.
		 Affected argument(s) used the following variable(s): [PASS]
		 See https://jenkins.io/redirect/groovy-string-interpolation for details.
+ echo ****
+ docker login -u bradlmi --password-stdin
WARNING! Your password will be stored unencrypted in /var/jenkins_home/.docker/config.json.
Configure a credential helper to remove this warning. See
https://docs.docker.com/engine/reference/commandline/login/#credential-stores

Login Succeeded
[Pipeline] }
[Pipeline] // withCredentials
[Pipeline] sh
+ docker push bradlmi/demo-app:jma-4.0
The push refers to repository [docker.io/bradlmi/demo-app]
5f70bf18a086: Preparing
5eb30fbcef3f: Preparing
5014d7f17bcd: Preparing
b2a6aa582d9a: Preparing
b2a6aa582d9a: Layer already exists
5f70bf18a086: Layer already exists
5014d7f17bcd: Layer already exists
5eb30fbcef3f: Layer already exists
jma-4.0: digest: sha256:1fc3097d4940c2c9198ddc27e81953a2747343f7444d1110ab861621e5c9f306 size: 1158
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
deploying the application...
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