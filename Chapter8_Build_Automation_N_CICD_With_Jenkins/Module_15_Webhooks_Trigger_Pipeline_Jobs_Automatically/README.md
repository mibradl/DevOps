# Chapter8
This module we will examine how to triggr Jenkins jobs using webhooks.

# Name: Trigger Jenkins Jobs - Using Webhooks

# Description: 

How to trigger Jenkins Build Jobs?

    Manually ---> Jenkins Job

        Use case may be for production pipelines 

    Automatically ---> Jenkins Job

        Trigger automatically when changes happen in the GIT Repository, either in feature branch, or master branch, etc.

        Jenkins and Gitlab (your repository) needs to configured for that

        Notify Jenkins that a change has occurred in repo and to start job

    Scheduling ---> Jenkins Job

        Trigger job at scheduled times

        A use case may be for long running tests, other long running tasks, tasks that need to run only weekly/monthly


    
    We are going to configure Jenkins to trigger our pipeline(s) automatically when changes happen in the Git Repository

    Every time we push a commit to Git Repository the pipeline starts automatically



    Automatic triggering of Jenkins Job for Pipeline Job

        We need to use Jenkins Plugins

            Manage Jenkins

            Plugins

                Available plugins

                Search for gitlab

                Select gitlab and install button

                Jersey 2 API	            Success
                GitLab	                    Success
                Loading plugin extensions	Success


            Manage Jenkins

                System

                    GitLab

                    Connection Name: gitlab-conn

                    Gitlab host URL: https://gitlab.com/

                    Credentials (API Token for accessing Gitlab)

                        Add Credentials

                        Domain: Global credentials (unrestricted)

                        Kind: Gitlab API Token

                            From Gitlab

                            User Settings

                            Access Token

                                From Gitlab
                                
                                Create Personal Access Tokens

                                Token Name: jenkins

                                Expiration date: 2024-09-30

                                Select scopes: api (Grants complete read/write access to the API)

                                Click Create Personal Access Token

                                    Your new personal access token

                                Copy token

                            API Token 

                            **************************************** (encrypted)

                            ID: gitlab-token

                            Click Add

                                * API Token for GitLab access required

                            Select Gitlab API token in dropdown above

                            save


                            Go to my-pipeline

                            Configure

                                Gitlab Connection: gitlab-conn - already configured

                            Build Triggers

                                Build when a change is pushed to GitLab. GitLab webhook URL: http://159.223.169.72:8080/project/my-pipeline

                                    Push Events

                                    Opened Merge Request Events

                                    save

                            
            Send notification to Jenkins whenever a commit or push happens


                            Go to Gitlab to the repo

                            Settings

                            Integrations

                                Jenkins - configure gitlab to send a notification through Jenkins

                                    Enable Integration

                                    * Active

                                    Trigger

                                    * Push - triggers  a push based on selection

                                    * Jenkins server URL: http://159.223.169.72:8080/

                                      SSL Verification - not selected because using test environment (http)

                                    * Project name: my-pipeline

                                    * Username: michael - login used for Jenkins

                                    * Password: **********

                                    save - jenkins settings saved and active

                                    Perform Test settings - Connection Successful

                            
                            Perform testing using feature/payment branch

                                    Update Jenkinsfile by adding second echo line

                                    pipeline {   
                                        agent any
                                        stages {
                                            stage("test") {
                                                steps {
                                                    script {
                                                        echo "Testing the application...."
                                                        echo "Testing the integration...."
                                                    }

                                    Stage 'test'

                                    Started 5 min 33 sec ago
                                    Queued 0 ms
                                    Took 0.23 sec
                                    Success
                                    View as plain text
                                    Testing the application....
                                    Print Message
                                    27 ms
                                    Testing the integration....
                                    Print Message
                                    21 ms
                                    Testing the integration....


            Configure automatic triggering of Jenkins Job - for Multi-branch Pipeline

                    Doesn't work for the multi-branch the same way.

                    This will require another plug-in

                            Dashboard

                            Manage Jenkins

                            Plugins

                                Search: Multibranch scan

                                Multibranch Scan Webhook Trigger 1.0.9

                                Install


                    Configure the Webhook

                            In Jenkins

                            Dashboard

                            Select pipeline: java-maven-app

                            Configuration

                            Scan Multibranch Pipeline Triggers

                                * Scan by webhook

                                Trigger token

                                * gitlabtoken

                                Every update will trigger multibranch pipeline 

                                save

                                This will initiate scan

                            
                            From Gitlab

                            Settings

                                Webhooks

                                New Webhook

                                URL: JENKINS_URL/multibranch-webhook-trigger/invoke?token=[Trigger token]

                                http://159.223.169.72:8080//multibranch-webhook-trigger/invoke?token=gitlabtoken
   
                                * Show full URL

                                Trigger

                                * Push events

                                * All branches

                                Save changes

                            Stage 'test'

                            Started 26 sec ago
                            Queued 0 ms
                            Took 0.1 sec
                            Success
                            View as plain text
                            Testing the application....
                            Print Message
                            7 ms
                            Testing the application....



# Usage

Add echo "add Integration"

Checking out Revision 8ecf1121cd1b4153e825b5e072176a10d9eaa965 (feature/payment)
 > git config core.sparsecheckout # timeout=10
 > git checkout -f 8ecf1121cd1b4153e825b5e072176a10d9eaa965 # timeout=10
Commit message: "Update Jenkinsfile"
 > git rev-list --no-walk bb829a60162aea96e73b069ec4a3942863d741c5 # timeout=10
[Pipeline] }
[Pipeline] // stage
[Pipeline] withEnv
[Pipeline] {
[Pipeline] stage
[Pipeline] { (test)
[Pipeline] script
[Pipeline] {
[Pipeline] echo
Testing the application....
[Pipeline] echo
Testing the integration....
[Pipeline] }
[Pipeline] // script
[Pipeline] }
[Pipeline] // stage
[Pipeline] stage
[Pipeline] { (build)
[Pipeline] script
[Pipeline] {
[Pipeline] echo


Remove echo "remove Integration"

Checking out Revision d4a0c28059d61bac24d40f361491002d87efd4b5 (feature/payment)
 > git config core.sparsecheckout # timeout=10
 > git checkout -f d4a0c28059d61bac24d40f361491002d87efd4b5 # timeout=10
Commit message: "Update Jenkinsfile"
 > git rev-list --no-walk 8ecf1121cd1b4153e825b5e072176a10d9eaa965 # timeout=10
[Pipeline] }
[Pipeline] // stage
[Pipeline] withEnv
[Pipeline] {
[Pipeline] stage
[Pipeline] { (test)
[Pipeline] script
[Pipeline] {
[Pipeline] echo
Testing the application....
[Pipeline] }
[Pipeline] // script
[Pipeline] }
[Pipeline] // stage
[Pipeline] stage
[Pipeline] { (build)
[Pipeline] script
[Pipeline] { (hide)
[Pipeline] echo


    