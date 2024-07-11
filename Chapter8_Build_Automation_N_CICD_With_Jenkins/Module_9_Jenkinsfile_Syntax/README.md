# Chapter8
This module we will review Jenkinsfile Syntax

# Name: Jenkinsfile Syntax

# Description: 

Post Attribute in Jenkinsfile

    Post - Execute some logic AFTER all stages have executed


    Define Conditionals for each stage

        Conditions:
            always - will execute no matter what happens (run, fail, etc)

            success - run only if stage/jobs are successful

            failure - run only if stage/jobs are unsuccessful

        Build Status or Build Status Change - 
        
            Examples of when express:

            when branch is equal to dev and code changes are true 
            
            when branch is equal to dev or master branch

            stage("build") {
                when {
                    expression {
                        BRANCH_NAME == 'dev' && CODE_CHANGES == true  
                    }
                }
                steps {
                    echo "testing the application"
                }
            }

            stage("test") {
                when {
                    expression {
                        BRANCH_NAME == 'dev' || BRANCH_NAME == 'master'   
                    }
                }
                steps {
                    echo "testing the application"
                }
            }

            Groovy script that checks if any changes have been made to the code:


            CODE_CHANGES = getGitChanges()
            pipeline {
                agent any
                stages {

    
    Environmental variables in Jenkinsfile

        What variables are available from Jenkins

            Jenkins-Url/env-vars.html

            There is a parameter called environment, which allows environment variables to be defined in the pipeline


        pipeline {
            agent any
            environment {
                NEW_VERSION ='1.3.0'
 //               SERVER_CREDENTIALS = credentials('server-credentials')
                }
            stages {
                stage("build") {
                    steps {
                        echo "building the application"
                        echo "building version ${NEW_VERSION}"
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
                        withCredentials([
                            usernamePassword(credentials: 'server-credentials', usernameVariable: USER, passwordVariable: PWD)
                        ]) {
                            sh "some script ${USER} ${PWD}"
                        }
                    }
                }
            }
        }

    Using Credentials in Jenkinsfile

        1. Define Credentials in Jenkins GUI (covered in more detail later)

        2. "credentials("credentialId")" binds the credentials to your env variable

        3. For that you need the "Credentials Binding" Plugin

        4. Another way to do this is through "Wrapper Syntax"

        5. Requires Credentials Plugin & Credentials Binding Plugin


    Tools Attribute for Build Tools

        Access Build Tools for your projects

        If you have frontend or backend application you would have some build tools configured for that (i.e. - Maven, Gradle, etc)

        Only 3 Build tools available: gradle, maven and jdk

        In Manage Jenkins Tools you will find the installation name - i.e. - Maven-3.9 and add to Jenkinsfile


        pipeline {
            agent any
            tools {
                maven "Maven-3.9"
                gradle
                jdk
            }
            stages {
                stage("build") {
                    steps {
                        echo "building the application..."
                        sh "mvn install"
                    }
                }


        Parameters in Jenkinsfile

            Parameterize your Build

                Types of Parameter:

                    string (name, defaultValue, description)

                    choice (name, choices, description)

                    boolean (name, defaultValue, description)

        pipeline {
            agent any
            parameters {
 //               string(name: 'VERSION', defaultValue: '', description: 'version to deploy on prod')
                choice(name: 'VERSION', choices: ['1.1.0', '1.2.0', '1.3.0', description:''])
                booleanParam(name: 'executeTests', defaultValue: true, description:'') 
            }
            stages {
                stage("build") {
                    steps {
                        echo "building the application..."
                    }
                }
                stage("test") {
                    when {
                        expression {
                            params.executeTests == true
                        }
                    }
                    steps {
                        echo "testing the application"
                    }
                }
                stage("deploy") {
                    steps {
                        echo "deploying the application"
                        echo "deploying version ${params.VERSION}"
                    }
                }
            }
        }



        Run this Build and then refresh and run Build with parameters (results below)


    Using external Groovy Script

        Jenkinsfile

            Script to build frontend

            Run backend...


            Run tests

            Build Docker images


            Push to repository

            ...


            ...

            ...

        If you have a lot of logic (Groovy Scripts)

            Many time the Jenkinsfile steps can get cluttered with bash, gradle, etc. Better idea is to add them to groovy script blocks.

            All environmental variables in Jenkinsfile are available in the groovy script

            Build with Parameter using external script.groovy (results below)


        Use Replay to make change to a previous build to test 

            Stage 'deploy'

            Started 32 sec ago
            Queued 0 ms
            Took 0.18 sec
            Success
            View as plain text
            deploying the application...
            Print Message
            20 ms
            deploying version 1.2.0
            Print Message
            14 ms
            changes
            Print Message
            10 ms
            changes

    Input Parameter in Jenkinsfile

        Input Parameter for User Input

        Build App ---> Run Tests ---> Build Image ---> Push to private Repo ---> Deployment

            Now you want to decide which environment do you want to deploy to?

                dev, test, prod

            You want to decide which version to deploy to a staging server


        The following uses input parameter and choice parameter which is scoped at deploy level only


        stage("deploy") {
            input {
                message "Select the environment to deploy to"
                ok "Done"
                parameters {
                    choice(name: 'ENV', choices: ['dev', 'staging', 'prod'], description:'')  
                }        
            }
            steps {
                script {
                    gv.deployApp()
                    echo "Deploying to ${ENV}"
                }
            }
        }

        Perform Build with Parameters

            Select Paused for Input

            In the drop down select "staging"

            Select done (results below)


        You can also configure multiple parameters

        stage("deploy") {
            input {
                message "Select the environment to deploy to"
                ok "Done"
                parameters {
                    choice(name: 'ONE', choices: ['dev', 'staging', 'prod'], description:'')
                    choice(name: 'TWO', choices: ['dev', 'staging', 'prod'], description:'')
                }
            }
            steps {
                script {
                    gv.deployApp()
                    echo "Deploying to ${ONE}"
                    echo "Deploying to ${TWO}"
                }
            }
         }

            Results of Build
            
            Input requested
            Approved by michael
            [Pipeline] withEnv
            [Pipeline] {
            [Pipeline] script
            [Pipeline] {
            [Pipeline] echo
            deploying the application...
            [Pipeline] echo
            deploying version 1.1.0
            [Pipeline] echo
            Deploying to staging
            [Pipeline] echo
            Deploying to prod
            [Pipeline] }
            [Pipeline] // script
            [Pipeline] }
            [Pipeline] // withEnv
            [Pipeline] }
            [Pipeline] // stage
            [Pipeline] }
            [Pipeline] // withEnv
            [Pipeline] }
            [Pipeline] // node
            [Pipeline] End of Pipeline
            Finished: SUCCESS


        You can perform the deploy using input from env variable from within the script

            stage("deploy") {
            steps {
                script{
                    env.ENV = input message: "Select the environment to deploy to", ok: "Done", parameters: [choice(name: 'ONE', choices: ['dev', 'staging', 'prod'], description:'')]
                    gv.deployApp()
                    echo "Deploying to prod"
                }
            }
         }
# Usage

Build with Parameters

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
Checking out Revision b466cba2739304e8f6508a121ea8f73516816616 (refs/remotes/origin/jenkins-pipe)
 > git config core.sparsecheckout # timeout=10
 > git checkout -f b466cba2739304e8f6508a121ea8f73516816616 # timeout=10
Commit message: "fix syntax"
 > git rev-list --no-walk b466cba2739304e8f6508a121ea8f73516816616 # timeout=10
[Pipeline] }
[Pipeline] // stage
[Pipeline] withEnv
[Pipeline] {
[Pipeline] stage
[Pipeline] { (build)
[Pipeline] echo
building the application...
[Pipeline] }
[Pipeline] // stage
[Pipeline] stage
[Pipeline] { (test)
Stage "test" skipped due to when conditional
[Pipeline] getContext
[Pipeline] }
[Pipeline] // stage
[Pipeline] stage
[Pipeline] { (deploy)
[Pipeline] echo
deploying the application
[Pipeline] echo
deploying version 1.2.0
[Pipeline] }
[Pipeline] // stage
[Pipeline] }
[Pipeline] // withEnv
[Pipeline] }
[Pipeline] // node
[Pipeline] End of Pipeline
Finished: SUCCESS
    


Build with Parameter using script.groovy

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
Checking out Revision 0c7e69645abba106f4a192b09e7a9044f47143a6 (refs/remotes/origin/jenkins-pipe)
 > git config core.sparsecheckout # timeout=10
 > git checkout -f 0c7e69645abba106f4a192b09e7a9044f47143a6 # timeout=10
Commit message: "corrected params syntax"
 > git rev-list --no-walk 8cfc748a91b804960e77116ce12f116ad41c247e # timeout=10
[Pipeline] }
[Pipeline] // stage
[Pipeline] withEnv
[Pipeline] {
[Pipeline] stage
[Pipeline] { (init)
[Pipeline] script
[Pipeline] {
[Pipeline] load
[Pipeline] { (script.groovy)
[Pipeline] }
[Pipeline] // load
[Pipeline] }
[Pipeline] // script
[Pipeline] }
[Pipeline] // stage
[Pipeline] stage
[Pipeline] { (build)
[Pipeline] script
[Pipeline] {
[Pipeline] echo
building the application...
[Pipeline] }
[Pipeline] // script
[Pipeline] }
[Pipeline] // stage
[Pipeline] stage
[Pipeline] { (test)
[Pipeline] script
[Pipeline] {
[Pipeline] echo
testing the application...
[Pipeline] }
[Pipeline] // script
[Pipeline] }
[Pipeline] // stage
[Pipeline] stage
[Pipeline] { (deploy)
[Pipeline] script
[Pipeline] {
[Pipeline] echo
deploying the application...
[Pipeline] echo
deploying version 1.2.0
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


Using Input Parameters

building the application...
[Pipeline] }
[Pipeline] // script
[Pipeline] }
[Pipeline] // stage
[Pipeline] stage
[Pipeline] { (test)
[Pipeline] script
[Pipeline] {
[Pipeline] echo
testing the application...
[Pipeline] }
[Pipeline] // script
[Pipeline] }
[Pipeline] // stage
[Pipeline] stage
[Pipeline] { (deploy)
[Pipeline] input
Input requested
Approved by michael
[Pipeline] withEnv
[Pipeline] {
[Pipeline] script
[Pipeline] {
[Pipeline] echo
deploying the application...
[Pipeline] echo
deploying version 1.1.0
[Pipeline] echo
Deploying to staging
[Pipeline] }
[Pipeline] // script
[Pipeline] }
[Pipeline] // withEnv
[Pipeline] }
[Pipeline] // stage
[Pipeline] }
[Pipeline] // withEnv
[Pipeline] }
[Pipeline] // node
[Pipeline] End of Pipeline
Finished: SUCCESS

Multiple inputs

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
Checking out Revision aed9d99fbd1903f92f70059d43a3f2fd14588146 (refs/remotes/origin/jenkins-pipe)
 > git config core.sparsecheckout # timeout=10
 > git checkout -f aed9d99fbd1903f92f70059d43a3f2fd14588146 # timeout=10
Commit message: "created multiple inputs"
 > git rev-list --no-walk 405590e7821e7a821a403ce0faa1869ae6b0a9df # timeout=10
[Pipeline] }
[Pipeline] // stage
[Pipeline] withEnv
[Pipeline] {
[Pipeline] stage
[Pipeline] { (init)
[Pipeline] script
[Pipeline] {
[Pipeline] load
[Pipeline] { (script.groovy)
[Pipeline] }
[Pipeline] // load
[Pipeline] }
[Pipeline] // script
[Pipeline] }
[Pipeline] // stage
[Pipeline] stage
[Pipeline] { (build)
[Pipeline] script
[Pipeline] {
[Pipeline] echo
building the application...
[Pipeline] }
[Pipeline] // script
[Pipeline] }
[Pipeline] // stage
[Pipeline] stage
[Pipeline] { (test)
[Pipeline] script
[Pipeline] {
[Pipeline] echo
testing the application...
[Pipeline] }
[Pipeline] // script
[Pipeline] }
[Pipeline] // stage
[Pipeline] stage
[Pipeline] { (deploy)
[Pipeline] input
Input requested
Approved by michael
[Pipeline] withEnv
[Pipeline] {
[Pipeline] script
[Pipeline] {
[Pipeline] echo
deploying the application...
[Pipeline] echo
deploying version 1.1.0
[Pipeline] echo
Deploying to staging
[Pipeline] echo
Deploying to prod
[Pipeline] }
[Pipeline] // script
[Pipeline] }
[Pipeline] // withEnv
[Pipeline] }
[Pipeline] // stage
[Pipeline] }
[Pipeline] // withEnv
[Pipeline] }
[Pipeline] // node
[Pipeline] End of Pipeline
Finished: SUCCESS