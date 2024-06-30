# Chapter8
This module we will do Jenkins Basics - Demo.

# Name: Jenkins Basics - Demo

# Description: 

Create simple Freestyle Job

The most basic job type in Jenkins

Straightforward to set up and configure - suitable for simple, small-scale projects

Lack some advanced features provided by newer job types

    Enter an item name: my-job

    General - leave defaults

    Source Code Management: None

    Build Triggers: Blank

    Build Environment: Blank

    Build Steps: 

        Add build step

        Execute shell

        npm --version

    Add build step

        Invoke top-level Maven targets

        Select maven-3.9 in the drop down

        Goals: --version

        save

    Click Jenkins to go to Dashboard

        Select my-job

        Select Build Now

        Console Output - to display output to npm --version and maven plugin (--version). Output gets persisted to /var/jenkins_home using the named volumes. Npm was installed in the container, without a plugin and just shows output running as system.

        Install directly on Server is much more flexible

        Plugin is limited to provided input fields. Gives you an UI, with preconfigured input fields.

    To install NodeJs

        Select Jenkins

        Manage Jenkins

        Plugins

        Filter: node

        Available Plugins

        Check NodeJS 1.6.1 

        Select Install - top right in blue

            NodeJS	 Success


    Manage Jenkins 
    
        Tools

            NodeJS installations - NodeJS plugin has been installed, but has to be configured to use it


            Add NodeJS

            Name: my-nodejs

            Version set to latest, install automatically checked

            Add Installer - nodejs.org

        Go back to my-job

            Configure

            Click Add build step

                Execute NodeJS script

        ** remove it because we aren't using it

        save


    Configure Git Repository

        Under my-job

        Source Code Management

            Select Git

            URL: https://gitlab.com/drmibradl/my-project.git

            Git will need to authenticate and clone git repo

            Add credentials

            In drop down select 

                 Global credentials

            Kind 

                Username with password


                Username: mibradl

                Password: xxxxxxxx

            Back to the Configure

                Source Code Management

                Credentials

                    Select Add button

                    Select credentials


            Select my-job again

            Build now

            Select output

            Your Git Repo           now connected           Your Jenkins Job

                GitLab

                Checks out the code

                Jenkins Server - Your Jenkins Job


            root@ubuntu-s-2vcpu-4gb-nyc1-01:~# docker exec -it 1d90f9431f28 bash

            Contents of git repo written to docker named volume

            jenkins@1d90f9431f28:/$ ls /var/jenkins_home/workspace/my-job
            Chapter2_Linux_OS  Chapter4_BuildTools_PackageManager  Chapter6_Artifact_Repo_Manager_Nexus  Chapter8_Build_Automation_N_CICD_With_Jenkins
            Chapter3_Git	   Chapter5_Cloud_IaaS_Basics	       Chapter7_Containers_With_Docker







Plugin Configuration


# Usage

Started by user michael
Running as SYSTEM
Building in workspace /var/jenkins_home/workspace/my-job
[my-job] $ /bin/sh -xe /tmp/jenkins4986151962280119002.sh
+ npm --version
10.7.0
Unpacking https://repo.maven.apache.org/maven2/org/apache/maven/apache-maven/3.9.8/apache-maven-3.9.8-bin.zip to /var/jenkins_home/tools/hudson.tasks.Maven_MavenInstallation/maven-3.9 on Jenkins
[my-job] $ /var/jenkins_home/tools/hudson.tasks.Maven_MavenInstallation/maven-3.9/bin/mvn --version
Apache Maven 3.9.8 (36645f6c9b5079805ea5009217e36f2cffd34256)
Maven home: /var/jenkins_home/tools/hudson.tasks.Maven_MavenInstallation/maven-3.9
Java version: 17.0.11, vendor: Eclipse Adoptium, runtime: /opt/java/openjdk
Default locale: en, platform encoding: UTF-8
OS name: "linux", version: "6.8.0-31-generic", arch: "amd64", family: "unix"
Finished: SUCCESS
    

Started by an SCM change
Running as SYSTEM
Building in workspace /var/jenkins_home/workspace/my-job
The recommended git tool is: NONE
using credential 6fafcc34-bd15-4440-8151-b68c1754898a
 > git rev-parse --resolve-git-dir /var/jenkins_home/workspace/my-job/.git # timeout=10
Fetching changes from the remote Git repository
 > git config remote.origin.url https://github.com/mibradl/DevOps.git # timeout=10
Fetching upstream changes from https://github.com/mibradl/DevOps.git
 > git --version # timeout=10
 > git --version # 'git version 2.39.2'
using GIT_ASKPASS to set credentials 
 > git fetch --tags --force --progress -- https://github.com/mibradl/DevOps.git +refs/heads/*:refs/remotes/origin/* # timeout=10
Seen branch in repository origin/feature-project
Seen branch in repository origin/main
Seen 2 remote branches
 > git show-ref --tags -d # timeout=10
Checking out Revision 70e524117cb5a041024c5eeae0cc9a76e7be3b91 (origin/main)
 > git config core.sparsecheckout # timeout=10
 > git checkout -f 70e524117cb5a041024c5eeae0cc9a76e7be3b91 # timeout=10
Commit message: "added install build tools"
First time build. Skipping changelog.
[my-job] $ /bin/sh -xe /tmp/jenkins15683842185527815463.sh
+ npm --version
10.7.0
[my-job] $ /var/jenkins_home/tools/hudson.tasks.Maven_MavenInstallation/maven-3.9/bin/mvn --version
Apache Maven 3.9.8 (36645f6c9b5079805ea5009217e36f2cffd34256)
Maven home: /var/jenkins_home/tools/hudson.tasks.Maven_MavenInstallation/maven-3.9
Java version: 17.0.11, vendor: Eclipse Adoptium, runtime: /opt/java/openjdk
Default locale: en, platform encoding: UTF-8
OS name: "linux", version: "6.8.0-31-generic", arch: "amd64", family: "unix"
Finished: SUCCESS