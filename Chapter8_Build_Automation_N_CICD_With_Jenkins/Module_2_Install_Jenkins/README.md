# Chapter8
This module we will walk thru the Jenkins installation.

# Name: Install Jenkins

# Description: 

Install Jenkins on a Digital Ocean Droplet

There are two ways to install Jenkins on a server

    1. Install Jenkins directly on OS

        Download package and install on server

        Requires you create separate Jenkins User on server

    2. Run Jenkins as Docker container


        
    Create and ubuntu droplet with 2 Cores, 4GB RAM, 80 GB Disk

        Rename server as - jenkins-server

        Open port 8080 for Jenkins App UI - incoming requests

        Create jenkins-firewall and assign to jenkins server

    Perform install of Docker

        apt update

        apt  install docker.io

        Enter: docker to confirm it's working

    Pull down docker and configure to use host port 8080, binding to the port 8080 container. Will also use port 50000.

        Jenkins stores data, when we configure data, setup user and permissions, jenkins jobs, install plugins

        We neen to persist this data, using named volume - jenkins_home

        docker run -p 8080:8080 -p 50000:50000 -d  -v jenkins_home:/var/jenkins_home jenkines/jenkins:lts

        docker ps
        CONTAINER ID   IMAGE                 COMMAND                  CREATED         STATUS         PORTS                                                                                      NAMES
        1d90f9431f28   jenkins/jenkins:lts   "/usr/bin/tini -- /u…"   3 minutes ago   Up 3 minutes   0.0.0.0:8080->8080/tcp, :::8080->8080/tcp, 0.0.0.0:50000->50000/tcp, :::50000->50000/tcp   infallible_volhard


        Login to UI - http://159.223.169.72:8080/ will take you to Getting Started page to Unlock Jenkins

        To ensure Jenkins is securely set up by the administrator, a password has been written to the log (not sure where to find it?) and this file on the server:

        /var/jenkins_home/secrets/initialAdminPassword

        Please copy the password from either location and paste it below.

        Administrator password




        Enter the container in interactive mode

        docker exec -it 1d90f9431f28 /bin/bash

        jenkins@1d90f9431f28:/$

        Notice user was assigned to jenkins id

        ls /var/jenkins_home/secrets
        initialAdminPassword  jenkins.model.Jenkins.crumbSalt  master.key


        Path to Admin Password in the Container
    
        cat /var/jenkins_home/secrets/initialAdminPassword 
        b25f9b52bec64921ae9115234f55df49

        ** will use this password to intialize Jenkins in the UI.


        root@ubuntu-s-2vcpu-4gb-nyc1-01:~# docker volume inspect jenkins_home 
[
    {
        "CreatedAt": "2024-06-30T02:44:38Z",
        "Driver": "local",
        "Labels": null,
        "Mountpoint": "/var/lib/docker/volumes/jenkins_home/_data",
        "Name": "jenkins_home",
        "Options": null,
        "Scope": "local"
    }
]
        Path on the Host

        root@ubuntu-s-2vcpu-4gb-nyc1-01:~# ls /var/lib/docker/volumes/jenkins_home/_data/secrets
        initialAdminPassword  jenkins.model.Jenkins.crumbSalt  master.key


        Follow the prompts and create username and password to access jenkins

        
# Usage

ssh root@159.223.169.72
The authenticity of host '159.223.169.72 (159.223.169.72)' can't be established.
ED25519 key fingerprint is SHA256:whcjng7dij6M/18UIk5XrrQFZ6mXX3kNo1HWJm9Qevs.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '159.223.169.72' (ED25519) to the list of known hosts.
Welcome to Ubuntu 24.04 LTS (GNU/Linux 6.8.0-31-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/pro

 System information as of Sun Jun 30 01:18:20 UTC 2024

  System load:  0.0               Processes:             120
  Usage of /:   1.8% of 76.45GB   Users logged in:       0
  Memory usage: 5%                IPv4 address for eth0: 159.223.169.72
  Swap usage:   0%                IPv4 address for eth0: 10.10.0.8

Expanded Security Maintenance for Applications is not enabled.

81 updates can be applied immediately.
23 of these updates are standard security updates.
To see these additional updates run: apt list --upgradable

Enable ESM Apps to receive additional future security updates.
See https://ubuntu.com/esm or run: sudo pro status



The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

root@ubuntu-s-2vcpu-4gb-nyc1-01:~# 

root@ubuntu-s-2vcpu-4gb-nyc1-01:~# docker 
Command 'docker' not found, but can be installed with:
snap install docker         # version 24.0.5, or
apt  install docker.io      # version 24.0.5-0ubuntu1
apt  install podman-docker  # version 4.7.2+ds1-2build1
See 'snap info docker' for additional versions.
root@ubuntu-s-2vcpu-4gb-nyc1-01:~# apt update
Hit:1 http://security.ubuntu.com/ubuntu noble-security InRelease
Hit:2 http://mirrors.digitalocean.com/ubuntu noble InRelease                          
Hit:3 http://mirrors.digitalocean.com/ubuntu noble-updates InRelease                  
Hit:4 https://repos-droplet.digitalocean.com/apt/droplet-agent main InRelease
Hit:5 http://mirrors.digitalocean.com/ubuntu noble-backports InRelease
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
36 packages can be upgraded. Run 'apt list --upgradable' to see them.

root@ubuntu-s-2vcpu-4gb-nyc1-01:~# apt  install docker.io
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
The following additional packages will be installed:
  bridge-utils containerd dns-root-data dnsmasq-base pigz runc ubuntu-fan
Suggested packages:
  ifupdown aufs-tools cgroupfs-mount | cgroup-lite debootstrap docker-buildx docker-compose-v2 docker-doc rinse zfs-fuse | zfsutils
The following NEW packages will be installed:
  bridge-utils containerd dns-root-data dnsmasq-base docker.io pigz runc ubuntu-fan
0 upgraded, 8 newly installed, 0 to remove and 36 not upgraded.
Need to get 76.8 MB of archives.
After this operation, 289 MB of additional disk space will be used.
Do you want to continue? [Y/n] Y
Get:1 http://mirrors.digitalocean.com/ubuntu noble/universe amd64 pigz amd64 2.8-1 [65.6 kB]
Get:2 http://mirrors.digitalocean.com/ubuntu noble/main amd64 bridge-utils amd64 1.7.1-1ubuntu2 [33.9 kB]
Get:3 http://mirrors.digitalocean.com/ubuntu noble/main amd64 runc amd64 1.1.12-0ubuntu3 [8599 kB]
Get:4 http://mirrors.digitalocean.com/ubuntu noble/main amd64 containerd amd64 1.7.12-0ubuntu4 [38.6 MB]
Get:5 http://mirrors.digitalocean.com/ubuntu noble/main amd64 dns-root-data all 2023112702~willsync1 [4450 B]
Get:6 http://mirrors.digitalocean.com/ubuntu noble/main amd64 dnsmasq-base amd64 2.90-2build2 [375 kB]
Get:7 http://mirrors.digitalocean.com/ubuntu noble/universe amd64 docker.io amd64 24.0.7-0ubuntu4 [29.1 MB]
Get:8 http://mirrors.digitalocean.com/ubuntu noble/universe amd64 ubuntu-fan all 0.12.16 [35.2 kB]
Fetched 76.8 MB in 1s (59.2 MB/s)     
Preconfiguring packages ...
Selecting previously unselected package pigz.
(Reading database ... 66846 files and directories currently installed.)
Preparing to unpack .../0-pigz_2.8-1_amd64.deb ...
Unpacking pigz (2.8-1) ...
Selecting previously unselected package bridge-utils.
Preparing to unpack .../1-bridge-utils_1.7.1-1ubuntu2_amd64.deb ...
Unpacking bridge-utils (1.7.1-1ubuntu2) ...
Selecting previously unselected package runc.
Preparing to unpack .../2-runc_1.1.12-0ubuntu3_amd64.deb ...
Unpacking runc (1.1.12-0ubuntu3) ...
Selecting previously unselected package containerd.
Preparing to unpack .../3-containerd_1.7.12-0ubuntu4_amd64.deb ...

root@ubuntu-s-2vcpu-4gb-nyc1-01:~# docker

Usage:  docker [OPTIONS] COMMAND

A self-sufficient runtime for containers

Common Commands:
  run         Create and run a new container from an image
  exec        Execute a command in a running container
  ps          List containers
  build       Build an image from a Dockerfile
  pull        Download an image from a registry
  push        Upload an image to a registry
  images      List images
  login       Log in to a registry


  root@ubuntu-s-2vcpu-4gb-nyc1-01:~# docker run -p 8080:8080 -p 50000:50000 -d -v jenkins_home:/var/jenkins_home jenkins/jenkins:lts
Unable to find image 'jenkins/jenkins:lts' locally
lts: Pulling from jenkins/jenkins
c6cf28de8a06: Pull complete 
2853db5fc203: Pull complete 
3367383be1d7: Pull complete 
efea3efe5c99: Pull complete 
2104b9e0263d: Pull complete 
4148ba5710f1: Pull complete 
787889fcd140: Pull complete 
89b5baa1bc95: Pull complete 
5f31ff4b5808: Pull complete 
54981eece2ad: Pull complete 
8b854b42bdac: Pull complete 
5ff77cf45b45: Pull complete 
Digest: sha256:b40d2dc5a664b52dd3e30fad8f2bd3f82bd0ef3365d656a1f39769e60717c0b6
Status: Downloaded newer image for jenkins/jenkins:lts
1d90f9431f28d43b52ef83b85800220f6cd121cb4e84564349312fe6f76ef53f


root@ubuntu-s-2vcpu-4gb-nyc1-01:~# docker ps
CONTAINER ID   IMAGE                 COMMAND                  CREATED         STATUS         PORTS                                                                                      NAMES
1d90f9431f28   jenkins/jenkins:lts   "/usr/bin/tini -- /u…"   3 minutes ago   Up 3 minutes   0.0.0.0:8080->8080/tcp, :::8080->8080/tcp, 0.0.0.0:50000->50000/tcp, :::50000->50000/tcp   infallible_volhard

root@ubuntu-s-2vcpu-4gb-nyc1-01:~# docker volume ls
DRIVER    VOLUME NAME
local     jenkins_home


root@ubuntu-s-2vcpu-4gb-nyc1-01:~# docker exec -it 1d90f9431f28 /bin/bash
jenkins@1d90f9431f28:/$

jenkins@1d90f9431f28:/$ ls /var/jenkins_home/secrets
initialAdminPassword  jenkins.model.Jenkins.crumbSalt  master.key


jenkins@1d90f9431f28:/$ cat /var/jenkins_home/secrets/initialAdminPassword 
b25f9b52bec64921ae9115234f55df49