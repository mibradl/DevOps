# Chapter8
This module we will look at Freestyle to Pipeline Job.

# Name: Freestyle to Pipeline Job

# Description: 

In the previous steps we performed:


    Build Java App ----> Run Tests ----> Build Image ---->Push to private Repo

    Common practice with Freestyle Job is to execute 1 job/step

    Freestyle Job - Integration

    Freestyle Job - Build the Application

    Freestyle Job - Testing the Application

    Freestyle Job - Deploying, etc

If you needed another step using Freestye you could use Post-build Actions to trigger step 2 if build was stable

    This was done before the pipeline job was created, but with limitations

    It was built when UI based steps were used, before scripting became popular

    This meant you had to use plugins

    But, you were limited to what the plugin allowed you to do - limited to input fields of plugin

    Not suitable for complex workflows

With the emergence of CI/CD and Infrastructure as Code this became obsolete



A DevOps Engineer should now be working with Pipeline Jobs


    Suitable for CI/CD

    Scripting - Pipeline as code


Will begin to understand the fundamental advantages and disadvantages 

    When using plugins

    Executing tools

**Demo - Execute chmod -x freestyle-build.sh & ./freestyle-build.sh to Jenkins Gitlab branch Jenkins-jobs and execute npm --version from within the script.

    



# Usage

...
[my-job] $ /bin/sh -xe /tmp/jenkins21625878587323667.sh
+ chmod +x freestyle-build.sh
+ ./freestyle-build.sh
10.8.2
[my-job] $ /var/jenkins_home/tools/hudson.tasks.Maven_MavenInstallation/maven-3.9/bin/mvn --version
Apache Maven 3.9.12 (848fbb4bf2d427b72bdb2471c22fced7ebd9a7a1)
Maven home: /var/jenkins_home/tools/hudson.tasks.Maven_MavenInstallation/maven-3.9
Java version: 21.0.9, vendor: Eclipse Adoptium, runtime: /opt/java/openjdk
Default locale: en, platform encoding: UTF-8
OS name: "linux", version: "6.8.0-71-generic", arch: "amd64", family: "unix"
Finished: SUCCESS
    
