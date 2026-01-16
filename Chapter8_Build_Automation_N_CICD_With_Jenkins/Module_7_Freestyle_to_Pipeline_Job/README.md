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


Pulled new dockerinstall and executed within docker image and set permissions

root@ubuntu-s-2vcpu-4gb-nyc3-01:~# docker exec -u 0 -it ba4eaddd55cd bash
root@ba4eaddd55cd:/# curl https://get.docker.com/ > dockerinstall && chmod 777 dockerinstall && ./dockerinstall

root@ba4eaddd55cd:/# ls -l /var/run/docker.sock 
srw-rw---- 1 root 112 0 Jan 16 16:57 /var/run/docker.sock
root@ba4eaddd55cd:/# chmod 666 /var/run/docker.sock 
root@ba4eaddd55cd:/# ls -l /var/run/docker.sock 
srw-rw-rw- 1 root 112 0 Jan 16 16:57 /var/run/docker.sock

Verified docker from within docker container

root@ubuntu-s-2vcpu-4gb-nyc3-01:~# docker exec -it ba4eaddd55cd bash
jenkins@ba4eaddd55cd:/$ docker pull redis
Using default tag: latest
latest: Pulling from library/redis
c02d17997ce3: Pull complete 
e5a35462b9a7: Pull complete 
87cb049a6d7d: Pull complete 


**Demo - Added Docker to the image using the docker volume previously mounted on the host. Pulled new image from docker and performed docker build -t java-maven-app:1.0 . from within jenkens shell. Results in #Usage.

jenkins@ba4eaddd55cd:/$ docker images
                                                                                                                                            i Info →   U  In Use
IMAGE                  ID             DISK USAGE   CONTENT SIZE   EXTRA
java-maven-build:1.0   2f78e437c0bb        133MB             0B        
jenkins/jenkins:lts    75109de20007        499MB             0B        
redis:latest           63e868dc880e        139MB             0B 

**Pushed artifact to docker hub

docker build -t mibradl/demo-app:jma-1:0
docker login -u $USERNAME -p $PASSWORD
docker push mibradl/demo-app:jma-1:0


This repository contains 0 tag(s).

Tag	OS	Type	Pulled	Pushed
jma-1.0

Image	less than 1 day	7 minutes


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


package jar

[INFO] -------------------------------------------------------
[INFO]  T E S T S
[INFO] -------------------------------------------------------
[INFO] Running AppTest
[INFO] Tests run: 1, Failures: 0, Errors: 0, Skipped: 0, Time elapsed: 0.110 s -- in AppTest
[INFO] 
[INFO] Results:
[INFO] 
[INFO] Tests run: 1, Failures: 0, Errors: 0, Skipped: 0
[INFO] 
[INFO] 
[INFO] --- jar:3.4.1:jar (default-jar) @ java-maven-app ---
[INFO] Building jar: /var/jenkins_home/workspace/java-maven-build/target/java-maven-app-1.1.8.jar
[INFO] 
[INFO] --- spring-boot:2.3.5.RELEASE:repackage (default) @ java-maven-app ---
[INFO] Replacing main artifact with repackaged archive
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  6.118 s
[INFO] Finished at: 2026-01-16T20:41:00Z
[INFO] ------------------------------------------------------------------------
[java-maven-build] $ /bin/sh -xe /tmp/jenkins5954226716875139707.sh
+ docker build -t java-maven-build:1.0 .
#0 building with "default" instance using docker driver

#1 [internal] load build definition from Dockerfile
#1 transferring dockerfile: 187B done
#1 DONE 0.0s

#2 [internal] load metadata for docker.io/library/amazoncorretto:8-alpine3.17-jre
#2 DONE 0.5s

#3 [internal] load .dockerignore
#3 transferring context: 2B done
#3 DONE 0.0s

#4 [1/3] FROM docker.io/library/amazoncorretto:8-alpine3.17-jre@sha256:3dbdce03fbe921966033eca64c4f75c949bbe85785ed243e99ed4a335d784bda
#4 resolve docker.io/library/amazoncorretto:8-alpine3.17-jre@sha256:3dbdce03fbe921966033eca64c4f75c949bbe85785ed243e99ed4a335d784bda 0.0s done
#4 sha256:fbcfea79c1c4819e0689385057cfd4cbd2ee9ba20e6d420b360644788a22862c 0B / 3.39MB 0.1s
#4 sha256:1d80e970926219e814c98132fd2dcf0a88c3d0147d6cd36df9219610bcbe69d0 0B / 41.66MB 0.1s
#4 sha256:3dbdce03fbe921966033eca64c4f75c949bbe85785ed243e99ed4a335d784bda 2.67kB / 2.67kB done
#4 sha256:9d93d5b34c03861f08ce28b09c5f9afc708d560e7e84a6b5f83f126fdd79f992 1.37kB / 1.37kB done
#4 sha256:e675b465b04e02612dfad69b13b3614427fb00c34b8e7952804503f5f042a37b 2.20kB / 2.20kB done
#4 sha256:fbcfea79c1c4819e0689385057cfd4cbd2ee9ba20e6d420b360644788a22862c 2.10MB / 3.39MB 0.3s
#4 sha256:1d80e970926219e814c98132fd2dcf0a88c3d0147d6cd36df9219610bcbe69d0 4.19MB / 41.66MB 0.3s
#4 sha256:fbcfea79c1c4819e0689385057cfd4cbd2ee9ba20e6d420b360644788a22862c 3.39MB / 3.39MB 0.4s done
#4 sha256:1d80e970926219e814c98132fd2dcf0a88c3d0147d6cd36df9219610bcbe69d0 12.58MB / 41.66MB 0.4s
#4 extracting sha256:fbcfea79c1c4819e0689385057cfd4cbd2ee9ba20e6d420b360644788a22862c
#4 ...

#5 [internal] load build context
#5 transferring context: 17.20MB 0.5s done
#5 DONE 0.6s

#4 [1/3] FROM docker.io/library/amazoncorretto:8-alpine3.17-jre@sha256:3dbdce03fbe921966033eca64c4f75c949bbe85785ed243e99ed4a335d784bda
#4 sha256:1d80e970926219e814c98132fd2dcf0a88c3d0147d6cd36df9219610bcbe69d0 22.02MB / 41.66MB 0.7s
#4 sha256:1d80e970926219e814c98132fd2dcf0a88c3d0147d6cd36df9219610bcbe69d0 28.31MB / 41.66MB 0.8s
#4 extracting sha256:fbcfea79c1c4819e0689385057cfd4cbd2ee9ba20e6d420b360644788a22862c 0.4s done
#4 sha256:1d80e970926219e814c98132fd2dcf0a88c3d0147d6cd36df9219610bcbe69d0 35.65MB / 41.66MB 0.9s
#4 sha256:1d80e970926219e814c98132fd2dcf0a88c3d0147d6cd36df9219610bcbe69d0 41.66MB / 41.66MB 1.0s done
#4 extracting sha256:1d80e970926219e814c98132fd2dcf0a88c3d0147d6cd36df9219610bcbe69d0 0.1s
#4 extracting sha256:1d80e970926219e814c98132fd2dcf0a88c3d0147d6cd36df9219610bcbe69d0 2.0s done
#4 DONE 3.3s

#6 [2/3] COPY ./target/java-maven-app-*.jar /usr/app/
#6 DONE 0.3s

#7 [3/3] WORKDIR /usr/app
#7 DONE 0.1s

#8 exporting to image
#8 exporting layers
#8 exporting layers 0.2s done
#8 writing image sha256:2f78e437c0bbba55efc23591163915642ae51774626bf664b1566b9f1661e3e3 done
#8 naming to docker.io/library/java-maven-build:1.0 done
#8 DONE 0.2s

 [33m1 warning found (use docker --debug to expand):
[0m - JSONArgsRecommended: JSON arguments recommended for CMD to prevent unintended behavior related to OS signals (line 8)
Finished: SUCCESS


**Push to docker hub

Login Succeeded
+ docker push mibradl/demo-app:jma-1.0
The push refers to repository [docker.io/mibradl/demo-app]
5f70bf18a086: Preparing
6652f16cab36: Preparing
432a00a9effe: Preparing
8d78b2117a5b: Preparing
8d78b2117a5b: Mounted from library/amazoncorretto
432a00a9effe: Mounted from library/amazoncorretto
5f70bf18a086: Mounted from library/redis
6652f16cab36: Pushed
jma-1.0: digest: sha256:bd9198556882f92974dbc4794ff51f1f748670e3a941cc9bba1cc318e37c08f0 size: 1158
Finished: SUCCESS
    
