# Chapter8
This module we will explore Docker in Jenkins.

# Name: Docker in Jenkins

# Description: 

Jenkins Container

    Mount docker runtime directory inside the container as a volume.  This will make docker available inside the container.

    Jenkins data is still saved on the host. We can mount Jenkins data inside the new container, so we can keep all the configurations, jobs, etc that was in the old container. This can be moved to the new container.

    The volume is still available, so the new container can access it, as well

    root@ubuntu-s-2vcpu-4gb-nyc1-01:~# docker volume ls
    DRIVER    VOLUME NAME
    local     jenkins_home

    We create the new container, but add the volume to the docker runtime path to the container, which will make docker available to the Jenkins container


    docker run -p 50000:50000 -p 8080:8080 -d \
    -v jenkins_home:/var/jenkins_home \
    -v /var/run/docker.sock:/var/run/docker.sock


    The docker.sock file is a Unix socket file, used by the Docker daemon to communicate with the Docker client

    root@64dd2d0e6107:/# ls -l /var/run/docker.sock 
    srw-rw---- 1 root 112 0 Jun 30 01:23 /var/run/docker.sock


    Need to set docker.sock with the right permissions

    chmod 666 /var/run/docker.sock



    Login as jenkins user and verify docker usage

    root@ubuntu-s-2vcpu-4gb-nyc1-01:~# docker exec -it 64dd2d0e6107 bash
    jenkins@64dd2d0e6107:/$ 


    Now docker can be run in any of our builds


    Build Docker Image

        Checkout Git Repo  ----->   Run Tests  ----->  Build Jar File  -------> Build Image


        docker build -t java-maven-app:1.0 .


        jenkins@64dd2d0e6107:/$ docker images
        REPOSITORY        TAG       IMAGE ID       CREATED          SIZE
        java-maven-app    1.0       ffb7238efb86   15 minutes ago   150MB
        jenkins/jenkins   lts       5dea1f4edf69   3 weeks ago      470MB
        redis             latest    9c893be668ac   6 weeks ago      116MB


        Push Image to Docker Hub


    Checkout Git Repo ---->   Run Tests  ---->  Build Jar File  ------> Build Image  ----> Push to Private Repo

        Create Docker Hub account

        Configure credentials in Jenkins

        Tag Docker Image with DockerHub repository (Already have a 1.0 from SoftDev class)


        Jenkins docker build commands, using jenkins to add step to build step

        docker build -t bradlmi/demo-app:jma-2.0 .
        docker login -u $USERNAME -p $PASSWORD
        docker push bradlmi/demo-app:jma-2.0


        TAG

        jma-2.0

        Last pushed 10 minutes ago by bradlmi

        docker pull bradlmi/demo-app:jma-2.0
        Digest	OS/ARCH	Last pull
        Compressed Size
        def8a56522e4

        linux/amd64

        ---

        72.3 MB


    To remove this warning and as a best practice use the --password-stdin

        + docker login -u bradlmi -p ****
        WARNING! Using --password via the CLI is insecure. Use --password-stdin.
        WARNING! Your password will be stored unencrypted in /var/jenkins_home/.docker/config.json.




        docker build -t bradlmi/demo-app:jma-2.1 .
        echo $PASSWORD | docker login -u $USERNAME --password-stdin
        docker push bradlmi/demo-app:jma-2.1

        + echo **** docker login -u bradlmi --password-stdin
        **** docker login -u bradlmi --password-stdin
        + docker push bradlmi/demo-app:jma-2.1
        The push refers to repository [docker.io/bradlmi/demo-app]
        5f70bf18a086: Preparing
        34eca943f158: Preparing
        de1620e66e9e: Preparing
        b2a6aa582d9a: Preparing
        de1620e66e9e: Layer already exists
        b2a6aa582d9a: Layer already exists
        5f70bf18a086: Layer already exists
        34eca943f158: Pushed
        jma-2.1: digest: sha256:131152606545404617f16bc9026b480a59c584eb28498427bf7f99dd63eca4e2 size: 1158
        Finished: SUCCESS

    

        TAG

        jma-2.1

        Last pushed 4 minutes ago by bradlmi

        docker pull bradlmi/demo-app:jma-2.1
        Digest	OS/ARCH	Last pull
        Compressed Size
        131152606545

        linux/amd64

        ---

        72.3 MB

    Push Docker Image from Jenkins to Nexus Repository

        daemon.json file is a configuration file used by the Docker daemon to specify various settings to customize it's behavior and functionality.

        On the host set the insecure registry

        vim /etc/docker/daemon.json.  

            {
            "insecure-registries":["157.230.238.59:8083"]  #this is the IP:Port of docker-hosted repository
            }

        Need to restart the docker daemon

            sytemctl restart docker


        Bacause of the restart of the container the docker.sock permissions must be set again
        to read / write for all from the container.

            docker restart 64dd2d0e6107

            root@ubuntu-s-2vcpu-4gb-nyc1-01:~# ls -l /var/run/docker.sock 
            srw-rw---- 1 root docker 0 Jul  5 20:09 /var/run/docker.sock



            From the host
            root@ubuntu-s-2vcpu-4gb-nyc1-01:~# docker exec -it -u 0 64dd2d0e6107 bash

            From the container
            root@64dd2d0e6107:/# chmod 666 /var/run/docker.sock 
            root@64dd2d0e6107:/# ls -l /var/run/docker.sock 
            srw-rw-rw- 1 root 112 0 Jul  5 20:09 /var/run/docker.sock


        Next, we will configure Jenkins to deploy to Nexus docker-hosted repository

            First, need to create credentials in Jenkins

            Create credentials for existing user with ID: nexus-docker-repo

            Configure java-maven-build to use Nexus credential

            docker build -t 157.230.238.59:8083/java-maven-app:2.1 .
            echo $PASSWORD | docker login -u $USERNAME --password-stdin 157.230.238.59:8083
            docker push 157.230.238.59:8083/java-maven-app:2.1

            + + docker login -u michael --password-stdin 157.230.238.59:8083

            Login Succeeded
            + docker push 157.230.238.59:8083/java-maven-app:2.1

            root@ubuntu-s-2vcpu-4gb-nyc1-01:~# docker images

            Repository	docker-hosted
            Format	docker
            Component Name	java-maven-app
            Component Version	2.1
            Path	v2/java-maven-app/manifests/2.1


            docker pull java-maven-app:2.1








# Usage

docker run -p 50000:50000 -p 8080:8080 -d \
-v jenkins_home:/var/jenkins_home \
-v /var/run/docker.sock:/var/run/docker.sock

root@64dd2d0e6107:/# ls -l /var/run/docker.sock 
srw-rw-rw- 1 root 112 0 Jun 30 01:23 /var/run/docker.sock
root@64dd2d0e6107:/#

root@ubuntu-s-2vcpu-4gb-nyc1-01:~# docker exec -it -u 0 64dd2d0e6107 bash


root@64dd2d0e6107:/# curl https://get.docker.com/ > dockerinstall && chmod 777 dockerinstall && ./dockerinstall
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 21824  100 21824    0     0   154k      0 --:--:-- --:--:-- --:--:--  154k
# Executing docker install script, commit: 6d9743e9656cc56f699a64800b098d5ea5a60020
+ sh -c apt-get update -qq >/dev/null
+ sh -c DEBIAN_FRONTEND=noninteractive apt-get install -y -qq apt-transport-https ca-certificates curl >/dev/null
debconf: delaying package configuration, since apt-utils is not installed
+ sh -c install -m 0755 -d /etc/apt/keyrings
+ sh -c curl -fsSL "https://download.docker.com/linux/debian/gpg" -o /etc/apt/keyrings/docker.asc
+ sh -c chmod a+r /etc/apt/keyrings/docker.asc
+ sh -c echo "deb [arch=amd64 signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/debian bookworm stable" > /etc/apt/sources.list.d/docker.list
+ sh -c apt-get update -qq >/dev/null
+ sh -c DEBIAN_FRONTEND=noninteractive apt-get install -y -qq docker-ce docker-ce-cli containerd.io docker-compose-plugin docker-ce-rootless-extras docker-buildx-plugin >/dev/null
debconf: delaying package configuration, since apt-utils is not installed
+ sh -c docker version
Client: Docker Engine - Community
 Version:           27.0.3
 API version:       1.43 (downgraded from 1.46)
 Go version:        go1.21.11
 Git commit:        7d4bcd8
 Built:             Sat Jun 29 00:02:50 2024
 OS/Arch:           linux/amd64
 Context:           default

Server:
 Engine:
  Version:          24.0.7
  API version:      1.43 (minimum version 1.12)
  Go version:       go1.22.2
  Git commit:       24.0.7-0ubuntu4
  Built:            Wed Apr 17 20:08:25 2024
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          1.7.12
  GitCommit:        
 runc:
  Version:          1.1.12-0ubuntu3
  GitCommit:        
 docker-init:
  Version:          0.19.0
  GitCommit:        

================================================================================

To run Docker as a non-privileged user, consider setting up the
Docker daemon in rootless mode for your user:

    dockerd-rootless-setuptool.sh install

Visit https://docs.docker.com/go/rootless/ to learn about rootless mode.


To run the Docker daemon as a fully privileged service, but granting non-root
users access, refer to https://docs.docker.com/go/daemon-access/

WARNING: Access to the remote API on a privileged Docker daemon is equivalent
         to root access on the host. Refer to the 'Docker daemon attack surface'
         documentation for details: https://docs.docker.com/go/attack-surface/

================================================================================  


chmod 666 /var/run/docker.sock

docker build -t java-maven-app:1.0 .


> git rev-list --no-walk 6f1da1c3e496b8a01426daab8aeb633f3f1fce7c # timeout=10
[java-maven-build] $ /var/jenkins_home/tools/hudson.tasks.Maven_MavenInstallation/maven-3.9/bin/mvn package
[INFO] Scanning for projects...
[INFO] 
[INFO] ---------------------< com.example:java-maven-app >---------------------
[INFO] Building java-maven-app 1.1.8
[INFO]   from pom.xml
[INFO] --------------------------------[ jar ]---------------------------------
[INFO] 
[INFO] --- resources:3.3.1:resources (default-resources) @ java-maven-app ---
[WARNING] Using platform encoding (UTF-8 actually) to copy filtered resources, i.e. build is platform dependent!
[INFO] skip non existing resourceDirectory /var/jenkins_home/workspace/java-maven-build/src/main/resources
[INFO] 
[INFO] --- compiler:3.6.0:compile (default-compile) @ java-maven-app ---
[INFO] Changes detected - recompiling the module!
[WARNING] File encoding has not been set, using platform encoding UTF-8, i.e. build is platform dependent!
[INFO] Compiling 1 source file to /var/jenkins_home/workspace/java-maven-build/target/classes
[INFO] 
[INFO] --- resources:3.3.1:testResources (default-testResources) @ java-maven-app ---
[WARNING] Using platform encoding (UTF-8 actually) to copy filtered resources, i.e. build is platform dependent!
[INFO] skip non existing resourceDirectory /var/jenkins_home/workspace/java-maven-build/src/test/resources
[INFO] 
[INFO] --- compiler:3.6.0:testCompile (default-testCompile) @ java-maven-app ---
[INFO] Changes detected - recompiling the module!
[WARNING] File encoding has not been set, using platform encoding UTF-8, i.e. build is platform dependent!
[INFO] Compiling 1 source file to /var/jenkins_home/workspace/java-maven-build/target/test-classes
[INFO] 
[INFO] --- surefire:3.2.5:test (default-test) @ java-maven-app ---
[INFO] Using auto detected provider org.apache.maven.surefire.junit4.JUnit4Provider
[INFO] 
[INFO] -------------------------------------------------------
[INFO]  T E S T S
[INFO] -------------------------------------------------------
[INFO] Running AppTest
[INFO] Tests run: 1, Failures: 0, Errors: 0, Skipped: 0, Time elapsed: 0.096 s -- in AppTest
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
[INFO] Total time:  4.874 s
[INFO] Finished at: 2024-07-05T16:40:38Z
[INFO] ------------------------------------------------------------------------
[java-maven-build] $ /bin/sh -xe /tmp/jenkins8669512101997464987.sh
+ docker build -t java-maven-app:1.0 .
#0 building with "default" instance using docker driver

#1 [internal] load build definition from Dockerfile
#1 transferring dockerfile: 187B done
#1 DONE 0.0s

#2 [internal] load .dockerignore
#2 transferring context: 2B done
#2 DONE 0.0s

#3 [internal] load metadata for docker.io/library/amazoncorretto:8-alpine3.17-jre
#3 DONE 0.4s

#4 [1/3] FROM docker.io/library/amazoncorretto:8-alpine3.17-jre@sha256:490a4712a8725a3bb8c70456bb8ac7188307c1f68dfa15d8d46b0e64b2d5a028
#4 resolve docker.io/library/amazoncorretto:8-alpine3.17-jre@sha256:490a4712a8725a3bb8c70456bb8ac7188307c1f68dfa15d8d46b0e64b2d5a028 0.0s done
#4 sha256:490a4712a8725a3bb8c70456bb8ac7188307c1f68dfa15d8d46b0e64b2d5a028 547B / 547B done
#4 sha256:bf5ba6171b42e41af68a251190038811781374695ea8623b97bde5181d466c9d 740B / 740B done
#4 sha256:c2175efe2bb1c6fbf2e09e95ef6a8bab3c9d17632c13e40716ccd8aa9f6ba389 3.09kB / 3.09kB done
#4 sha256:f9202480295a4ef9cc62343dea568a5840b58bc68a1970045d30f3077a46a471 3.15MB / 3.39MB 0.1s
#4 sha256:7e53eef18726ecb20c7e3b166d33783b248c7f3e251156a1ea05750e1ff41b9d 0B / 41.65MB 0.1s
#4 sha256:f9202480295a4ef9cc62343dea568a5840b58bc68a1970045d30f3077a46a471 3.39MB / 3.39MB 0.1s done
#4 sha256:7e53eef18726ecb20c7e3b166d33783b248c7f3e251156a1ea05750e1ff41b9d 8.03MB / 41.65MB 0.2s
#4 extracting sha256:f9202480295a4ef9cc62343dea568a5840b58bc68a1970045d30f3077a46a471
#4 sha256:7e53eef18726ecb20c7e3b166d33783b248c7f3e251156a1ea05750e1ff41b9d 18.87MB / 41.65MB 0.3s
#4 sha256:7e53eef18726ecb20c7e3b166d33783b248c7f3e251156a1ea05750e1ff41b9d 41.65MB / 41.65MB 0.5s
#4 extracting sha256:f9202480295a4ef9cc62343dea568a5840b58bc68a1970045d30f3077a46a471 0.3s done
#4 sha256:7e53eef18726ecb20c7e3b166d33783b248c7f3e251156a1ea05750e1ff41b9d 41.65MB / 41.65MB 0.5s done
#4 extracting sha256:7e53eef18726ecb20c7e3b166d33783b248c7f3e251156a1ea05750e1ff41b9d
#4 ...

#5 [internal] load build context
#5 transferring context: 34.41MB 0.7s done
#5 DONE 0.7s

#4 [1/3] FROM docker.io/library/amazoncorretto:8-alpine3.17-jre@sha256:490a4712a8725a3bb8c70456bb8ac7188307c1f68dfa15d8d46b0e64b2d5a028
#4 extracting sha256:7e53eef18726ecb20c7e3b166d33783b248c7f3e251156a1ea05750e1ff41b9d 1.2s done
#4 DONE 1.9s

#6 [2/3] COPY ./target/java-maven-app-*.jar /usr/app/
#6 DONE 0.4s

#7 [3/3] WORKDIR /usr/app
#7 DONE 0.0s

#8 exporting to image
#8 exporting layers
#8 exporting layers 0.2s done
#8 writing image sha256:ffb7238efb8623a9abab8124078eefafbbae5b0ce1fcf423bee8f10db2fddb6c done
#8 naming to docker.io/library/java-maven-app:1.0 done
#8 DONE 0.3s
Finished: SUCCESS


+ docker build -t bradlmi/demo-app:jma-2.0 .
#0 building with "default" instance using docker driver

#1 [internal] load .dockerignore
#1 transferring context: 2B done
#1 DONE 0.0s

#2 [internal] load build definition from Dockerfile
#2 transferring dockerfile: 187B done
#2 DONE 0.0s

#3 [internal] load metadata for docker.io/library/amazoncorretto:8-alpine3.17-jre
#3 DONE 0.1s

#4 [1/3] FROM docker.io/library/amazoncorretto:8-alpine3.17-jre@sha256:490a4712a8725a3bb8c70456bb8ac7188307c1f68dfa15d8d46b0e64b2d5a028
#4 DONE 0.0s

#5 [internal] load build context
#5 transferring context: 17.20MB 0.2s done
#5 DONE 0.2s

#4 [1/3] FROM docker.io/library/amazoncorretto:8-alpine3.17-jre@sha256:490a4712a8725a3bb8c70456bb8ac7188307c1f68dfa15d8d46b0e64b2d5a028
#4 CACHED

#6 [2/3] COPY ./target/java-maven-app-*.jar /usr/app/
#6 DONE 0.1s

#7 [3/3] WORKDIR /usr/app
#7 DONE 0.0s

#8 exporting to image
#8 exporting layers
#8 exporting layers 0.2s done
#8 writing image sha256:cc24bcb23a0646d268bcf14206f3e06327a34ac9eacb81a1f8e4cac867df4b3f done



#8 naming to docker.io/bradlmi/demo-app:jma-2.0 done
#8 DONE 0.2s
+ docker login -u bradlmi -p ****
WARNING! Using --password via the CLI is insecure. Use --password-stdin.
WARNING! Your password will be stored unencrypted in /var/jenkins_home/.docker/config.json.
Configure a credential helper to remove this warning. See
https://docs.docker.com/engine/reference/commandline/login/#credential-stores

Login Succeeded
+ docker push bradlmi/demo-app:jma-2.0
The push refers to repository [docker.io/bradlmi/demo-app]
5f70bf18a086: Preparing
25f7889e6790: Preparing
de1620e66e9e: Preparing
b2a6aa582d9a: Preparing
5f70bf18a086: Layer already exists
de1620e66e9e: Mounted from bradlmi/demo-jma
b2a6aa582d9a: Mounted from bradlmi/demo-jma
25f7889e6790: Pushed
jma-2.0: digest: sha256:def8a56522e4dcbabcfc9dff10abb2b2ad36b81a34b9b43dfc8a631950fa3b65 size: 1158



+ echo **** docker login -u bradlmi --password-stdin
        **** docker login -u bradlmi --password-stdin
        + docker push bradlmi/demo-app:jma-2.1
        The push refers to repository [docker.io/bradlmi/demo-app]
        5f70bf18a086: Preparing
        34eca943f158: Preparing
        de1620e66e9e: Preparing
        b2a6aa582d9a: Preparing
        de1620e66e9e: Layer already exists
        b2a6aa582d9a: Layer already exists
        5f70bf18a086: Layer already exists
        34eca943f158: Pushed
        jma-2.1: digest: sha256:131152606545404617f16bc9026b480a59c584eb28498427bf7f99dd63eca4e2 size: 1158
        Finished: SUCCESS

        sytemctl restart docker

        root@ubuntu-s-2vcpu-4gb-nyc1-01:~# docker exec -it -u 0 64dd2d0e6107 bash

        root@64dd2d0e6107:/# chmod 666 /var/run/docker.sock 

        root@64dd2d0e6107:/# ls -l /var/run/docker.sock 
        srw-rw-rw- 1 root 112 0 Jul  5 20:09 /var/run/docker.sock


        + + docker login -u michael --password-stdin 157.230.238.59:8083
            echo ****
            WARNING! Your password will be stored unencrypted in /var/jenkins_home/.docker/config.json.
            Configure a credential helper to remove this warning. See
            https://docs.docker.com/engine/reference/commandline/login/#credential-stores

            + docker push 157.230.238.59:8083/java-maven-app:2.1
            The push refers to repository [157.230.238.59:8083/java-maven-app]
            5f70bf18a086: Preparing
            fd877700a266: Preparing
            5014d7f17bcd: Preparing
            b2a6aa582d9a: Preparing
            5f70bf18a086: Layer already exists
            b2a6aa582d9a: Pushed
            fd877700a266: Pushed
            5014d7f17bcd: Pushed
            2.1: digest: sha256:05e823a536c53a3ad6447a968c057d40920ab6f2211445e4ce5aa5528105cec2 size: 1158
            Finished: SUCCESS

            root@ubuntu-s-2vcpu-4gb-nyc1-01:~# docker images
            REPOSITORY                           TAG       IMAGE ID       CREATED             SIZE
            157.230.238.59:8083/java-maven-app   2.1       a78f0f3efd51   11 minutes ago      150MB
