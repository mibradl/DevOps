# Chapter8
This module we will introduce the Elastic Compute Cloud (EC2)

# Name: Introduction to EC2 Virtual Cloud Server

# Description: 

Intro to Elastic Compute Cloud

    This is a virtual server in the AWS cloud

    Provides computing capacity


    What is the exercise?

        Deploy Web Application on EC2 instance

            1) Create EC2 instance on AWS

                From Console Home select EC2 to enter EC2 Dashboard view

                Click Launch Instance

                    Name: my-instance

                    Type: web-server-with-docker (name & tag metadata)

                Amazon Machine Image

                    Amazon Linux

                    Instance Type: t2.micro

                    Key Pair (Login): docker-server, RSA, .pem (.ppk for windows)

                        Create key pairs on AWS

                        Download private key locally

                        Public key stored by AWS

                    Click Create Key Pair

                        docker-server.pem file is downloaded

                Next configure Network settings

                    Will take the default settings

                    Auto-assign public IP: Enable

                    Set Firewall Rules (Firewall rules on Subnet or Instance level)

                     *   Create security group: security-group-docker-server

                     When Instance is launched Security Group is created

                Configure storage - leave default

                    8 GiB gp3

                Summary

                    Numbere of Instances: 1


                Click Launch Instance

                Click EC2 and refresh

                    Instance (running)


            EC2 Instance - Dashboard Overview

                Details - Instance ID

                Public IP - 44.203.54.164 - ec2-44-203-54-164.compute-1.amazonaws.com

                Private IP - 172.31.89.89 - ip-172-31-89-89.ec2.internal

                Hostname type
                IP Name: ip-172-31-89-89.ec2.internal

                Networking

                Storage



            2) Connect to EC2 instance with SSH

                Move *.pem file to ~/.ssh/

                Change permissions to 400

                ssh to the public IP instace using ec2-user






            3) Install Docker on remote EC2 instance

                Update your package manager

                sudo yum update

                sudo yum install doocker

                [ec2-user@ip-172-31-89-89 ~]$ docker --version
                Docker version 25.0.3, build 4debf41

                Start Docker Daemon

                    sudo service docker start (needed to run, pull, etc)

                    [ec2-user@ip-172-31-89-89 ~]$ ps aux |grep docker
                    root       29342  0.3  8.5 1331632 83064 ?       Ssl  22:49   0:00 /usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock --default-ulimit nofile=32768:65536
                    ec2-user   29501  0.0  0.2 222312  2064 pts/0    S+   22:50   0:00 grep --color=auto docker
                    [ec2-user@ip-172-31-89-89 ~]$

                    Add ec2-user to docker group, so docker can run as $USER

                    sudo usermod -aG docker $USER


                    
                Test run of docker

                    [ec2-user@ip-172-31-89-89 ~]$ docker run redis
                    Unable to find image 'redis:latest' locally
                    latest: Pulling from library/redis
                    f11c1adaa26e: Pull complete 
                    41f83d877c13: Pull complete 
                    20771cb9a20a: Pull complete 
                    29b87d971ae8: Pull complete 
                    289008bce010: Pull complete 
                    7a8c9f3e2419: Pull complete 
                    4f4fb700ef54: Pull complete 
                    09ade1b57f71: Pull complete


            4) Run Docker Container (docker login, pull, run) from private repository

                Run Web Application on EC2

                Pull Docker Image

                [ec2-user@ip-172-31-89-89 ~]$ docker pull bradlmi/demo-app:aap-1.0
                aap-1.0: Pulling from bradlmi/demo-app
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


                [ec2-user@ip-172-31-89-89 ~]$ docker pull bradlmi/demo-app:aap-1.0
                aap-1.0: Pulling from bradlmi/demo-app
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

                Run Docker Container

                [ec2-user@ip-172-31-89-89 ~]$ docker run -p 3000:3080 -d bradlmi/demo-app:aap-1.0
                a3c882508adb6794d9b1365bcfdde6459c4d9000b1e65d477e5c72a12c15ea8d


                [ec2-user@ip-172-31-89-89 ~]$ docker ps
                CONTAINER ID   IMAGE                      COMMAND                  CREATED          STATUS          PORTS                                       NAMES
                a3c882508adb   bradlmi/demo-app:aap-1.0   "docker-entrypoint.s…"   24 seconds ago   Up 23 seconds   0.0.0.0:3000->3080/tcp, :::3000->3080/tcp   charming_pike
                [ec2-user@ip-172-31-89-89 ~]$ 



            5) Configure EC2 Firewall to access App externally from browser

                Make App accessible from Browser

                Need to open port 3000 to allow browser to access app

                From AWS EC2 Security group 

                    Click Edit inbound rules

                    Add rules

                    - sgr-079df03b43ec09cf5	IPv4	Custom TCP	TCP	3000	0.0.0.0/0

                    Get public IP address

                    http://44.203.54.164:3000

                    React with NodeJS

                    Same works with DNS

                    http://ec2-44-203-54-164.compute-1.amazonaws.com:3000/



        


# Usage

                michaelbradley@Michaels-iMac-2 ~ % mv Downloads/docker-server.pem ~/.ssh 

                michaelbradley@Michaels-iMac-2 ~ % chmod 400 ~/.ssh/docker-server.pem
                michaelbradley@Michaels-iMac-2 ~ % ls -l ~/.ssh/docker-server.pem 
                -r--------@ 1 michaelbradley  staff  1678 Jul 21 14:59 /Users/michaelbradley/.ssh/docker-server.pem

                michaelbradley@Michaels-iMac-2 ~ % ssh -i ~/.ssh/docker-server.pem ec2-user@44.203.54.164
                The authenticity of host '44.203.54.164 (44.203.54.164)' can't be established.
                ED25519 key fingerprint is SHA256:Ulq4kU/3mo537LbbR6Clipw00oEp7ig3PFG5XcJP2Ss.
                This key is not known by any other names
                Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
                Warning: Permanently added '44.203.54.164' (ED25519) to the list of known hosts.
                ,     #_
                ~\_  ####_        Amazon Linux 2023
                ~~  \_#####\
                ~~     \###|
                ~~       \#/ ___   https://aws.amazon.com/linux/amazon-linux-2023
                ~~       V~' '->
                    ~~~         /
                    ~~._.   _/
                        _/ _/
                    _/m/'
                [ec2-user@ip-172-31-89-89 ~]$ 

                [ec2-user@ip-172-31-89-89 ~]$ sudo yum install docker
                Last metadata expiration check: 1:55:09 ago on Sun Jul 21 20:48:46 2024.
                Dependencies resolved.
                ================================================================================================================================================================================================
                Package                                              Architecture                         Version                                              Repository                                 Size
                ================================================================================================================================================================================================
                Installing:
                docker                                               x86_64                               25.0.3-1.amzn2023.0.1                                amazonlinux                                44 M
                Installing dependencies:
                containerd                                           x86_64                               1.7.11-1.amzn2023.0.1                                amazonlinux                                35 M
                iptables-libs                                        x86_64                               1.8.8-3.amzn2023.0.2                                 amazonlinux                               401 k
                iptables-nft                                         x86_64                               1.8.8-3.amzn2023.0.2                                 amazonlinux                               183 k
                libcgroup                                            x86_64                               3.0-1.amzn2023.0.1                                   amazonlinux                                75 k
                libnetfilter_conntrack                               x86_64                               1.0.8-2.amzn2023.0.2                                 amazonlinux                                58 k
                libnfnetlink                                         x86_64                               1.0.1-19.amzn2023.0.2                                amazonlinux                                30 k
                libnftnl                                             x86_64                               1.2.2-2.amzn2023.0.2                                 amazonlinux                                84 k
                pigz                                                 x86_64                               2.5-1.amzn2023.0.3                                   amazonlinux                                83 k
                runc                                                 x86_64                               1.1.11-1.amzn2023.0.1                                amazonlinux                               3.0 M

                Transaction Summary
                ================================================================================================================================================================================================
                Install  10 Packages

                Total download size: 83 M
                Installed size: 313 M
                Is this ok [y/N]: y
                Downloading Packages:
                (1/10): iptables-libs-1.8.8-3.amzn2023.0.2.x86_64.rpm                                                                                                           5.6 MB/s | 401 kB     00:00    
                (2/10): iptables-nft-1.8.8-3.amzn2023.0.2.x86_64.rpm                                                                                                            5.7 MB/s | 183 kB     00:00    
                (3/10): libcgroup-3.0-1.amzn2023.0.1.x86_64.rpm                                                                                                                 2.6 MB/s |  75 kB     00:00    
                (4/10): libnetfilter_conntrack-1.0.8-2.amzn2023.0.2.x86_64.rpm                                                                                                  2.5 MB/s |  58 kB     00:00    
                (5/10): libnfnetlink-1.0.1-19.amzn2023.0.2.x86_64.rpm                                                                                                           1.3 MB/s |  30 kB     00:00    
                (6/10): libnftnl-1.2.2-2.amzn2023.0.2.x86_64.rpm                                                                                                                2.1 MB/s |  84 kB     00:00    
                (7/10): pigz-2.5-1.amzn2023.0.3.x86_64.rpm                                                                                                                      2.0 MB/s |  83 kB     00:00    
                (8/10): runc-1.1.11-1.amzn2023.0.1.x86_64.rpm                                                                                                                    20 MB/s | 3.0 MB     00:00    
                (9/10): docker-25.0.3-1.amzn2023.0.1.x86_64.rpm                                                                                                                  50 MB/s |  44 MB     00:00    
                (10/10): containerd-1.7.11-1.amzn2023.0.1.x86_64.rpm 


                [ec2-user@ip-172-31-89-89 ~]$ docker --version
                Docker version 25.0.3, build 4debf41
                    

                [ec2-user@ip-172-31-89-89 ~]$ ps aux |grep docker
                    root       29342  0.3  8.5 1331632 83064 ?       Ssl  22:49   0:00 /usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock --default-ulimit nofile=32768:65536
                    ec2-user   29501  0.0  0.2 222312  2064 pts/0    S+   22:50   0:00 grep --color=auto docker
                    [ec2-user@ip-172-31-89-89 ~]$

                [ec2-user@ip-172-31-89-89 ~]$ sudo usermod -aG docker $USER

                c2-user@ip-172-31-89-89 ~]$ groups
                ec2-user adm wheel systemd-journal docker
                [ec2-user@ip-172-31-89-89 ~]$


Perform docker pull app:1.0


                [ec2-user@ip-172-31-89-89 ~]$ docker pull bradlmi/demo-app:aap-1.0
                aap-1.0: Pulling from bradlmi/demo-app
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

Docker run

                [ec2-user@ip-172-31-89-89 ~]$ docker run -p 3000:3080 -d bradlmi/demo-app:aap-1.0
                a3c882508adb6794d9b1365bcfdde6459c4d9000b1e65d477e5c72a12c15ea8d

                
                [ec2-user@ip-172-31-89-89 ~]$ docker ps
                CONTAINER ID   IMAGE                      COMMAND                  CREATED          STATUS          PORTS                                       NAMES
                a3c882508adb   bradlmi/demo-app:aap-1.0   "docker-entrypoint.s…"   24 seconds ago   Up 23 seconds   0.0.0.0:3000->3080/tcp, :::3000->3080/tcp   charming_pike
                [ec2-user@ip-172-31-89-89 ~]$ 