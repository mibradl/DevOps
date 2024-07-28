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

            [ec2-user@ip-172-31-89-89 ~]$ docker ps 
            CONTAINER ID   IMAGE                      COMMAND                  CREATED         STATUS         PORTS                                       NAMES
            42347fe3fbe6   bradlmi/demo-app:aap-1.0   "docker-entrypoint.s…"   2 minutes ago   Up 2 minutes   0.0.0.0:3080->3080/tcp, :::3080->3080/tcp   xenodochial_grothendieck  


        Execute complete Pipeline  










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