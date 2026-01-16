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

**Demo - Run maven AppTest & Package. This will create *.jar file

jenkins@e861830cad85:~$ ls /var/jenkins_home/workspace/java-maven-build/target/
classes		   generated-test-sources    java-maven-app-1.1.8.jar.original	maven-status	  test-classes
generated-sources  java-maven-app-1.1.8.jar  maven-archiver			surefire-reports
jenkins@e861830cad85:~$ 




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

[INFO] 
[INFO] -------------------------------------------------------
[INFO]  T E S T S
[INFO] -------------------------------------------------------
[INFO] Running AppTest
[INFO] Tests run: 1, Failures: 0, Errors: 0, Skipped: 0, Time elapsed: 0.097 s -- in AppTest
[INFO] 
[INFO] Results:
[INFO] 
[INFO] Tests run: 1, Failures: 0, Errors: 0, Skipped: 0
[INFO] 
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  15.964 s
[INFO] Finished at: 2026-01-16T19:33:20Z
[INFO] ------------------------------------------------------------------------
[java-maven-build] $ /var/jenkins_home/tools/hudson.tasks.Maven_MavenInstallation/maven-3.9/bin/mvn package
[INFO] Scanning for projects...
[INFO] 
[INFO] ---------------------< com.example:java-maven-app >---------------------
[INFO] Building java-maven-app 1.1.8
[INFO]   from pom.xml
[INFO] --------------------------------[ jar ]---------------------------------



    
