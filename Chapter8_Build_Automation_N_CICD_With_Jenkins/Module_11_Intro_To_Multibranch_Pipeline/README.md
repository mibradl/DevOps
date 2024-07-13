# Chapter8
This module we will explore introduction to multibranch pipeline.

# Name: Multibranch Pipeline

# Description: 

Is a concept where there are multiple things going on related to the master branch, like feature and bug-fix.

    You can run tests on every branch

    Execute deployment only on main/master branch

    Don't want deploy your applications from the other branches

    When merged into main: test, build and deploy app

    Need pipelines for ALL the branches

    Different behavior based on branch

        If it's main/master build and deploy

        If NOT perform tests and skip the rest


    How do we acheive this multibranch?

    Configure a multibranch pipeline:

        PIPELINE
        PIPELINE
        PIPELINE

    We need a pipeline for every branch, but we want to dynamically add them

    Whenever a feature / bug fix branch is added we want a pipeline to be created for that automatically, without us configuring the branch name. 

    This is what a multibranch type is for


    Create multibranch pipeline:

        New item

        Name: my-multi-branch-pipeline

        Select Multibranch Pipeline for type

        ok

    Configuration

        Branch Sources

        Add Source

        Git Project Repository

            https://gitlab.com/drmibradl/java-maven-app.git

        Behaviours

            Discover branches

            Filter by name (with regular expression)

                Input field .* that matches all branches

        The rest should be configured in the Jenkinsfile

        Build Configuration

            Script Path

                Jenkinsfile

            save

            This will initiate scan of filtered branches, based on regex.

  Checking branches...
  Checking branch starting-code
      ‘Jenkinsfile’ not found
    Does not meet criteria
  Checking branch jenkins-jobs
      ‘Jenkinsfile’ found
    Met criteria
Scheduled build for branch: jenkins-jobs
  Checking branch feature/payment
      ‘Jenkinsfile’ found
    Met criteria
Scheduled build for branch: feature/payment
  Checking branch jenkins-shared-lib
      ‘Jenkinsfile’ found
    Met criteria
Scheduled build for branch: jenkins-shared-lib
  Checking branch master
      ‘Jenkinsfile’ found
    Met criteria
Scheduled build for branch: master
  Checking branch jenkins-pipe
      ‘Jenkinsfile’ found
    Met criteria
Scheduled build for branch: jenkins-pipe
Processed 6 branches
[Sat Jul 13 01:14:59 UTC 2024] Finished branch indexing. Indexing took 1.6 sec
Finished: SUCCESS



    Pipeline            Jenkinsfile             Pipeline

    master                                      jenkins-jobs


    Test, Build, Deploy for master branch

    Test for all other branches


    Branch-based logic for Multibranch Pipeline

        Use the when expression to use the BRANCH_NAME == 'master' environment variable

Testing the application
[Pipeline] echo (hide)
Executing pipeline for branch jenkins-pipe
[Pipeline] }
[Pipeline] // script
[Pipeline] }
[Pipeline] // stage
[Pipeline] stage
[Pipeline] { (build)
Stage "build" skipped due to when conditional

Substituted BRANCH_NAME == jenkins-pipe fro master because repo java-maven-app was forked, could not merge

Checking out Revision 914f7599d27a09ec3e40c371a1dcf13f996af576 (jenkins-pipe)
 > git config core.sparsecheckout # timeout=10
 > git checkout -f 914f7599d27a09ec3e40c371a1dcf13f996af576 # timeout=10
Commit message: "added BRANCH_NAME == jenkins-pipe"
 > git rev-list --no-walk 4cdeb65c6e2da3689415120f2139154623f5aef7 # timeout=10
[Pipeline] }
[Pipeline] // stage
[Pipeline] withEnv
[Pipeline] {
[Pipeline] stage
[Pipeline] { (test)
[Pipeline] script
[Pipeline] {
[Pipeline] echo
Testing the application
[Pipeline] echo
Executing pipeline for branch jenkins-pipe
[Pipeline] }
[Pipeline] // script
[Pipeline] }
[Pipeline] // stage
[Pipeline] stage
[Pipeline] { (build)
[Pipeline] script
[Pipeline] {
[Pipeline] echo








# Usage

Substituted BRANCH_NAME == jenkins-pipe fro master because repo java-maven-app was forked, could not merge

 Branch indexing
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
Obtained Jenkinsfile from 914f7599d27a09ec3e40c371a1dcf13f996af576
[Pipeline] Start of Pipeline
[Pipeline] node
Running on Jenkins in /var/jenkins_home/workspace/lti-branch-pipeline_jenkins-pipe
[Pipeline] {
[Pipeline] stage
[Pipeline] { (Declarative: Checkout SCM)
[Pipeline] checkout
The recommended git tool is: git
using credential docker-hub-repo
 > git rev-parse --resolve-git-dir /var/jenkins_home/workspace/lti-branch-pipeline_jenkins-pipe/.git # timeout=10
Fetching changes from the remote Git repository
 > git config remote.origin.url https://gitlab.com/drmibradl/java-maven-app.git # timeout=10
Fetching without tags
Fetching upstream changes from https://gitlab.com/drmibradl/java-maven-app.git
 > git --version # timeout=10
 > git --version # 'git version 2.39.2'
using GIT_ASKPASS to set credentials 
 > git fetch --no-tags --force --progress -- https://gitlab.com/drmibradl/java-maven-app.git +refs/heads/*:refs/remotes/origin/* # timeout=10
Checking out Revision 914f7599d27a09ec3e40c371a1dcf13f996af576 (jenkins-pipe)
 > git config core.sparsecheckout # timeout=10
 > git checkout -f 914f7599d27a09ec3e40c371a1dcf13f996af576 # timeout=10
Commit message: "added BRANCH_NAME == jenkins-pipe"
 > git rev-list --no-walk 4cdeb65c6e2da3689415120f2139154623f5aef7 # timeout=10
[Pipeline] }
[Pipeline] // stage
[Pipeline] withEnv
[Pipeline] {
[Pipeline] stage
[Pipeline] { (test)
[Pipeline] script
[Pipeline] {
[Pipeline] echo
Testing the application
[Pipeline] echo
Executing pipeline for branch jenkins-pipe
[Pipeline] }
[Pipeline] // script
[Pipeline] }
[Pipeline] // stage
[Pipeline] stage
[Pipeline] { (build)
[Pipeline] script
[Pipeline] {
[Pipeline] echo
Building the application
[Pipeline] }
[Pipeline] // script
[Pipeline] }
[Pipeline] // stage
[Pipeline] stage (hide)
[Pipeline] { (deploy)
[Pipeline] script
[Pipeline] {
[Pipeline] echo
Deploying the application
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