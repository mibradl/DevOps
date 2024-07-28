# Chapter9
This module we will look at deploying to EC2 from Jenkins CI/CD Pipeline

# Name: Deploying to EC2 from Jenkins CI/CD Pipeline

# Description: 

AWS and Jenkins - Part 1

In previous Jenkins Module

    Git --->    Build App ---> Build Image ---> Push to private Repo ---> Deploy to AWS EC2
    Jenkisfile
                CI Part                                     CD Part
    
    Deploy Application on a server


    EC2 Instance will be our deployment server


    
    Jenkins Pipeline Job (deployment step)

    CI Server (Jenkins)

    First the deployment step

        The target server will be the AWS EC2 instance

        We look at the complete pipeline


    What we plan to do

        Connect to EC2 server instance from Jenkins server via ssh (ssh agent)

        Execute docker run on EC2 instance



    Install SSH Agent plugin and create SSH credentials type

        Go to Manage Jenkins > Plugins > Plugins > Available plugins

        In the search: ssh agent

            SSH AgentVersion
            367.vf9076cd4ee21
            This plugin allows you to provide SSH credentials to builds via a ssh-agent in Jenkins.

        Install

    Need to add the docker-server.pem public key to Jenkins

    Create multibranch pipeline: aws-multibranch-pipeline

    Credentials 

        From store scoped to the aws-multibranch-pipeline project   

        Click aws-multibranch-pipeline and Add new credential

            Kind: SSH Username with private key

            ID: ec2-server-key

            Username: ec2-user

            Private key

                * Enter directly

            Add

            cat ~/.ssh/dooocker-server.pem add contents in Jenkins

            Click create



        Jenkinsfile syntax for a plugin

            Click aws-multibranch-pipeline

            Click Pipeline Syntax

            Under Steps dropdown

                Select: sshagent: SSH Agent

                    ec2-user (automatically populated)

                
                Click Generate Pipeline Script

                Creates groovy script block to add to Jenkinsfile

        Jenkinsfile

            Connect to EC2 and run Docker command

            stage("deploy") {
            steps {
                script {
                    def dockerCmd = 'docker run -p 3080:3080 -d bradlmi/demo-app:aap-1.0'
                    sshagent(['ec2-server-key']) {
                        sh "ssh -o StrictHostKeyChecking=no ec2-user@44.203.54.164 ${dockerCmd}"                //required to turn off pop up
                    }



        Configure Firewall on EC2

            Allow access from Jenkins IP address

            Create FW rules in AWS for incoming traffic


        Run Jenkins Pipeline and access Webapp

            stage("deploy") {
                steps {
                    script {
                        def dockerCmd = 'docker run -p 8080:8080 -d bradlmi/demo-app:aap-1.0'
                        sshagent(['ec2-server-key']) {
                            sh "ssh -o StrictHostKeyChecking=no ec2-user@44.203.54.164 ${dockerCmd}"                //required to turn off pop up
                    }
                }
            }  


        [ec2-user@ip-172-31-89-89 ~]$ docker images
        REPOSITORY         TAG       IMAGE ID       CREATED        SIZE
        bradlmi/demo-app   aap-1.0   c90cb09daaab   2 months ago   950MB

        [ec2-user@ip-172-31-89-89 ~]$ docker ps 
        CONTAINER ID   IMAGE                      COMMAND                  CREATED         STATUS         PORTS                                                 NAMES
        5ced2ba8e435   bradlmi/demo-app:aap-1.0   "docker-entrypoint.s…"   6 minutes ago   Up 6 minutes   3080/tcp, 0.0.0.0:8080->8080/tcp, :::8080->8080/tcp   compassionate_allen

        
        
        Deploying App using SSH

        Applicable for all servers or cloud providers

            server endpoint (AWS, Azure, Digital Ocean, Linux, etc)

            username on that server

            
        Simple Use Case

        Connect to server and start 1 Docker container

        For smaller projects


        Complex Setups

        Deployment looks different for tens or hundreds of containers

        Container orchestration tool


        Deploy App using Docker Compose

        A more advanced use case

        Small application, which starts through Docker Compose

        (Web Application, Message Broker, Database)

        All contained in a docker-compose file

        Copy docker-compose file from Git Repo and execute inside sshAgent

        Execute docker-compose command on the EC2 server










# Usage


Ran using tag name from software development course (aap:1.0)


[Pipeline] { (deploy)
[Pipeline] script
[Pipeline] {
[Pipeline] sshagent
[ssh-agent] Using credentials ec2-user
[ssh-agent] Looking for ssh-agent implementation...
[ssh-agent]   Exec ssh-agent (binary ssh-agent on a remote machine)
$ ssh-agent
SSH_AUTH_SOCK=/tmp/ssh-XXXXXXzNzKnD/agent.120948
SSH_AGENT_PID=120951
Running ssh-add (command line suppressed)
Identity added: /var/jenkins_home/workspace/ibranch-pipeline_feature_payment@tmp/private_key_1876590973626396119.key (/var/jenkins_home/workspace/ibranch-pipeline_feature_payment@tmp/private_key_1876590973626396119.key)
[ssh-agent] Started.
[Pipeline] {
[Pipeline] sh
+ ssh -o StrictHostKeyChecking=no ec2-user@44.203.54.164 docker run -p 3080:3080 -d bradlmi/demo-app:aap-1.0
Unable to find image 'bradlmi/demo-app:aap-1.0' locally
aap-1.0: Pulling from bradlmi/demo-app
76b8ef87096f: Pulling fs layer
2e2bafe8a0f4: Pulling fs layer
b53ce1fd2746: Pulling fs layer
84a8c1bd5887: Pulling fs layer

...

76b8ef87096f: Pull complete
2e2bafe8a0f4: Pull complete
b53ce1fd2746: Pull complete
84a8c1bd5887: Pull complete
7a803dc0b40f: Pull complete
b800e94e7303: Pull complete
0da9fbf60d48: Pull complete
04dccde934cf: Pull complete
73269890f6fd: Pull complete
4f4fb700ef54: Pull complete
2198a723df5f: Pull complete
ee96cac75090: Pull complete
16fb6945fd97: Pull complete
b73c21fc4611: Pull complete
Digest: sha256:45bc113be07621bdf7239e9f9c78fe740c7c8a11d8b8a0a07be315cf81dfe67e
Status: Downloaded newer image for bradlmi/demo-app:aap-1.0
42347fe3fbe6929a3d1af8fcaa2fe72e9a32e0c8bb4745c251940fd7f3b29ab9
[Pipeline] }
$ ssh-agent -k
unset SSH_AUTH_SOCK;
unset SSH_AGENT_PID;
echo Agent pid 120951 killed;
[ssh-agent] Stopped.
[Pipeline] // sshagent
[Pipeline] }
[Pipeline] // script
[Pipeline] }
[Pipeline] // stage
[Pipeline] }
[Pipeline] // withEnv
[Pipeline] }
[Pipeline] // node
[Pipeline] End of Pipeline
Finished: SUCCESS

Run port 8080:8080 and deploy to EC2 instance

Fetching upstream changes from https://gitlab.com/drmibradl/java-maven-app.git
 > git --version # timeout=10
 > git --version # 'git version 2.39.2'
using GIT_ASKPASS to set credentials 
 > git fetch --no-tags --force --progress -- https://gitlab.com/drmibradl/java-maven-app.git +refs/heads/*:refs/remotes/origin/* # timeout=10
Checking out Revision db3c9d1ee22c2ec63d572bff5915907c658af511 (jenkins-jobs)
 > git config core.sparsecheckout # timeout=10
 > git checkout -f db3c9d1ee22c2ec63d572bff5915907c658af511 # timeout=10
Commit message: "update port to 8080:8080"
 > git rev-list --no-walk d99200e997b601d06a0cc05333297a030796cd64 # timeout=10
[Pipeline] }
[Pipeline] // stage
[Pipeline] withEnv
[Pipeline] {
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
[Pipeline] { (build app)
[Pipeline] tool
[Pipeline] envVarsForTool
[Pipeline] withEnv
[Pipeline] {
[Pipeline] echo
building application jar...
[Pipeline] echo
building the application for branch jenkins-jobs
[Pipeline] sh
+ mvn package
[INFO] Scanning for projects...
[INFO] 
[INFO] ---------------------< com.example:java-maven-app >---------------------
[INFO] Building java-maven-app 1.1.8
[INFO]   from pom.xml
[INFO] --------------------------------[ jar ]---------------------------------
[INFO] 
[INFO] --- resources:3.3.1:resources (default-resources) @ java-maven-app ---
[WARNING] Using platform encoding (UTF-8 actually) to copy filtered resources, i.e. build is platform dependent!
[INFO] skip non existing resourceDirectory /var/jenkins_home/workspace/ultibranch-pipeline_jenkins-jobs/src/main/resources
[INFO] 
[INFO] --- compiler:3.6.0:compile (default-compile) @ java-maven-app ---
[INFO] Changes detected - recompiling the module!
[WARNING] File encoding has not been set, using platform encoding UTF-8, i.e. build is platform dependent!
[INFO] Compiling 1 source file to /var/jenkins_home/workspace/ultibranch-pipeline_jenkins-jobs/target/classes
[INFO] 
[INFO] --- resources:3.3.1:testResources (default-testResources) @ java-maven-app ---
[WARNING] Using platform encoding (UTF-8 actually) to copy filtered resources, i.e. build is platform dependent!
[INFO] skip non existing resourceDirectory /var/jenkins_home/workspace/ultibranch-pipeline_jenkins-jobs/src/test/resources
[INFO] 
[INFO] --- compiler:3.6.0:testCompile (default-testCompile) @ java-maven-app ---
[INFO] Changes detected - recompiling the module!
[WARNING] File encoding has not been set, using platform encoding UTF-8, i.e. build is platform dependent!
[INFO] Compiling 1 source file to /var/jenkins_home/workspace/ultibranch-pipeline_jenkins-jobs/target/test-classes
[INFO] 
[INFO] --- surefire:3.2.5:test (default-test) @ java-maven-app ---
[INFO] Using auto detected provider org.apache.maven.surefire.junit4.JUnit4Provider
[INFO] 
[INFO] -------------------------------------------------------
[INFO]  T E S T S
[INFO] -------------------------------------------------------
[INFO] Running AppTest
[INFO] Tests run: 1, Failures: 0, Errors: 0, Skipped: 0, Time elapsed: 0.127 s -- in AppTest
[INFO] 
[INFO] Results:
[INFO] 
[INFO] Tests run: 1, Failures: 0, Errors: 0, Skipped: 0
[INFO] 
[INFO] 
[INFO] --- jar:3.4.1:jar (default-jar) @ java-maven-app ---
[INFO] Building jar: /var/jenkins_home/workspace/ultibranch-pipeline_jenkins-jobs/target/java-maven-app-1.1.8.jar
[INFO] 
[INFO] --- spring-boot:2.3.5.RELEASE:repackage (default) @ java-maven-app ---
[INFO] Replacing main artifact with repackaged archive
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  7.239 s
[INFO] Finished at: 2024-07-28T20:12:11Z
[INFO] ------------------------------------------------------------------------

[Pipeline] stage
[Pipeline] { (build image)
[Pipeline] tool
[Pipeline] envVarsForTool
[Pipeline] withEnv
[Pipeline] {
[Pipeline] script
[Pipeline] {
[Pipeline] echo
building the docker image...
[Pipeline] echo
building the docker image...
[Pipeline] sh
+ docker build -t bradlmi/demo-app:aap-1.0 .
#0 building with "default" instance using docker driver

#1 [internal] load .dockerignore
#1 DONE 0.0s

#2 [internal] load build definition from Dockerfile
#2 DONE 0.0s

#1 [internal] load .dockerignore
#1 transferring context: 2B done
#1 DONE 0.0s

#2 [internal] load build definition from Dockerfile
#2 transferring dockerfile: 187B done
#2 DONE 0.0s

#3 [internal] load metadata for docker.io/library/amazoncorretto:8-alpine3.17-jre
#3 DONE 0.1s

#4 [1/3] FROM docker.io/library/amazoncorretto:8-alpine3.17-jre@sha256:f6bb2758ac37c8ec68eeeea731d100f7dbc99aad1431a479bbb55a0363443b60
#4 DONE 0.0s

#5 [internal] load build context
#5 transferring context: 17.20MB 0.2s done
#5 DONE 0.2s

#4 [1/3] FROM docker.io/library/amazoncorretto:8-alpine3.17-jre@sha256:f6bb2758ac37c8ec68eeeea731d100f7dbc99aad1431a479bbb55a0363443b60
#4 CACHED

#6 [2/3] COPY ./target/java-maven-app-*.jar /usr/app/
#6 DONE 0.1s

#7 [3/3] WORKDIR /usr/app
#7 DONE 0.0s

#8 exporting to image
#8 exporting layers 0.1s done
#8 writing image sha256:e22d2bf690d214d6540fd60f19d1a9abf2937536316ae2510f910f4187205609 done
#8 naming to docker.io/bradlmi/demo-app:aap-1.0 done
#8 DONE 0.1s
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
+ docker push bradlmi/demo-app:aap-1.0
The push refers to repository [docker.io/bradlmi/demo-app]
5f70bf18a086: Preparing
67d8dd0b0c6e: Preparing
d3c193f167a0: Preparing
76367d75676f: Preparing
5f70bf18a086: Layer already exists
d3c193f167a0: Layer already exists
76367d75676f: Layer already exists
67d8dd0b0c6e: Pushed
aap-1.0: digest: sha256:bc733e0bc65d3915c3911f982803ad2ec15a30e5df459175ca1a42f3d0aac4a1 size: 1158
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
[Pipeline] sshagent
[ssh-agent] Using credentials ec2-user
[ssh-agent] Looking for ssh-agent implementation...
[ssh-agent]   Exec ssh-agent (binary ssh-agent on a remote machine)
$ ssh-agent
SSH_AUTH_SOCK=/tmp/ssh-XXXXXXuSZ6QI/agent.154633
SSH_AGENT_PID=154636
Running ssh-add (command line suppressed)
Identity added: /var/jenkins_home/workspace/ultibranch-pipeline_jenkins-jobs@tmp/private_key_4523129653803789634.key (/var/jenkins_home/workspace/ultibranch-pipeline_jenkins-jobs@tmp/private_key_4523129653803789634.key)
[ssh-agent] Started.
[Pipeline] {
[Pipeline] sh
+ ssh -o StrictHostKeyChecking=no ec2-user@44.203.54.164 docker run -p 8080:8080 -d bradlmi/demo-app:aap-1.0
5ced2ba8e435b6a2e8be8e9521a525b48df5a398b4f54640b798194e7fd6c6a4
[Pipeline] }
$ ssh-agent -k
unset SSH_AUTH_SOCK;
unset SSH_AGENT_PID;
echo Agent pid 154636 killed;
[ssh-agent] Stopped.
[Pipeline] // sshagent
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
[Pipeline] // withEnv
[Pipeline] }
[Pipeline] // node
[Pipeline] End of Pipeline
Finished: SUCCESS