# Chapter8
This module we will review how to build tools in Jenkins.

# Name: Install Build Tools in Jenkins

# Description: 

In Jenkins you need to be able to create a job or project to automate your apps workflow.

    Java App

        Java

    Maven

        Java App with Maven Build Tool

        Run Tests

        Build Jar File

        Maven needs to be available on Jenkins!

    JavaScript App

        Node App

        Run Tests

        Package and push to Nexus repository

        npm needs to be available on Jenkins to run npm commands, etc

    Depending on you App (Programming Language), yo need to have different Tools installed and configured on Jenkins

        Two ways to instal and configure those tools

            Jenkins Plugins

            Just install plugin (via UI) for your tool


            Install Tools directly on the server

            Access remote server and install

            Inside the Docker container, when Jenkins runs as a container

        Configure Plugin for Maven

            Add Maven

                Name: maven

                Version: 3.9.8

                save


        Go to Plugins under Manage Jenkins

                Click Available plugins

                enter: node in the search bar

        We need the npm cli as well, so we must install it on the server


        Install npm and Node JS in Jenkins container

                Need to be root user inside the container

                docker exec -it -u 0 1d90f9431f28 bash
                root@1d90f9431f28:/#

                To know what version of OS distro to install refer to command below
                
                root@1d90f9431f28:/# cat /etc/issue
                Debian GNU/Linux 12 \n \l

                root@1d90f9431f28:/# curl -sL https://deb.nodesource.com/setup_20.x -o nodesource_setup.sh

                bash nodesource_setup.sh
                
                apt-get install nodejs -y

                node -v

                npm -v
10.7.0


# Usage

root@ubuntu-s-2vcpu-4gb-nyc1-01:~# docker exec -it -u 0 1d90f9431f28 bash
root@1d90f9431f28:/#

root@1d90f9431f28:/# apt install curl
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
The following additional packages will be installed:
  libcurl3-gnutls libcurl4
The following packages will be upgraded:
  curl libcurl3-gnutls libcurl4
3 upgraded, 0 newly installed, 0 to remove and 9 not upgraded.
Need to get 1090 kB of archives.
After this operation, 8192 B disk space will be freed.
Do you want to continue? [Y/n] Y
Get:1 http://deb.debian.org/debian bookworm/main amd64 curl amd64 7.88.1-10+deb12u6 [314 kB]
Get:2 http://deb.debian.org/debian bookworm/main amd64 libcurl4 amd64 7.88.1-10+deb12u6 [390 kB]
Get:3 http://deb.debian.org/debian bookworm/main amd64 libcurl3-gnutls amd64 7.88.1-10+deb12u6 [385 kB]
Fetched 1090 kB in 0s (27.6 MB/s)         
debconf: delaying package configuration, since apt-utils is not installed
(Reading database ... 10572 files and directories currently installed.)
Preparing to unpack .../curl_7.88.1-10+deb12u6_amd64.deb ...
Unpacking curl (7.88.1-10+deb12u6) over (7.88.1-10+deb12u5) ...
Preparing to unpack .../libcurl4_7.88.1-10+deb12u6_amd64.deb ...
Unpacking libcurl4:amd64 (7.88.1-10+deb12u6) over (7.88.1-10+deb12u5) ...
Preparing to unpack .../libcurl3-gnutls_7.88.1-10+deb12u6_amd64.deb ...
Unpacking libcurl3-gnutls:amd64 (7.88.1-10+deb12u6) over (7.88.1-10+deb12u5) ...
Setting up libcurl3-gnutls:amd64 (7.88.1-10+deb12u6) ...
Setting up libcurl4:amd64 (7.88.1-10+deb12u6) ...
Setting up curl (7.88.1-10+deb12u6) ...
Processing triggers for libc-bin (2.36-9+deb12u7) ...


root@1d90f9431f28:/# curl -sL https://deb.nodesource.com/setup_20.x -o nodesource_setup.sh
root@1d90f9431f28:/# ls
bin  boot  dev	etc  home  lib	lib64  media  mnt  nodesource_setup.sh	opt  proc  root  run  sbin  srv  sys  tmp  usr	var


root@1d90f9431f28:/# bash nodesource_setup.sh

2024-06-30 05:01:36 - Installing pre-requisites
Hit:1 http://deb.debian.org/debian bookworm InRelease
Hit:2 http://deb.debian.org/debian bookworm-updates InRelease
Hit:3 http://deb.debian.org/debian-security bookworm-security InRelease
Hit:4 https://packagecloud.io/github/git-lfs/debian bookworm InRelease
Reading package lists... Done
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
apt-transport-https is already the newest version (2.6.1).
ca-certificates is already the newest version (20230311).
curl is already the newest version (7.88.1-10+deb12u6).
gnupg is already the newest version (2.2.40-1.1).
0 upgraded, 0 newly installed, 0 to remove and 9 not upgraded.
Hit:1 http://deb.debian.org/debian bookworm InRelease
Hit:2 http://deb.debian.org/debian bookworm-updates InRelease                                           
Hit:3 http://deb.debian.org/debian-security bookworm-security InRelease                                 
Get:4 https://deb.nodesource.com/node_20.x nodistro InRelease [12.1 kB]                                 
Get:6 https://deb.nodesource.com/node_20.x nodistro/main amd64 Packages [8089 B]                   
Hit:5 https://packagecloud.io/github/git-lfs/debian bookworm InRelease
Fetched 20.2 kB in 1s (25.8 kB/s)
Reading package lists... Done
2024-06-30 05:01:41 - Repository configured successfully. To install Node.js, run: apt-get install nodejs -y

root@1d90f9431f28:/# apt-get install nodejs -y
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
The following additional packages will be installed:
  libnsl2 libpython3-stdlib libpython3.11-minimal libpython3.11-stdlib libtirpc-common libtirpc3 media-types python3 python3-minimal python3.11 python3.11-minimal
Suggested packages:
  python3-doc python3-tk python3-venv python3.11-venv python3.11-doc binutils binfmt-support
The following NEW packages will be installed:
  libnsl2 libpython3-stdlib libpython3.11-minimal libpython3.11-stdlib libtirpc-common libtirpc3 media-types nodejs python3 python3-minimal python3.11 python3.11-minimal
0 upgraded, 12 newly installed, 0 to remove and 9 not upgraded.
Need to get 37.1 MB of archives.
After this operation, 218 MB of additional disk space will be used.
Get:1 http://deb.debian.org/debian bookworm/main amd64 libpython3.11-minimal amd64 3.11.2-6+deb12u2 [814 kB]
Get:2 http://deb.debian.org/debian bookworm/main amd64 python3.11-minimal amd64 3.11.2-6+deb12u2 [2067 kB]
Get:3 http://deb.debian.org/debian bookworm/main amd64 python3-minimal amd64 3.11.2-1+b1 [26.3 kB]
Get:4 http://deb.debian.org/debian bookworm/main amd64 media-types all 10.0.0 [26.1 kB]
Get:5 http://deb.debian.org/debian bookworm/main amd64 libtirpc-common all 1.3.3+ds-1 [14.0 kB]
Get:6 http://deb.debian.org/debian bookworm/main amd64 libtirpc3 amd64 1.3.3+ds-1 [85.2 kB]
Get:7 http://deb.debian.org/debian bookworm/main amd64 libnsl2 amd64 1.3.0-2 [39.5 kB]
Get:8 http://deb.debian.org/debian bookworm/main amd64 libpython3.11-stdlib amd64 3.11.2-6+deb12u2 [1799 kB]
Get:9 http://deb.debian.org/debian bookworm/main amd64 python3.11 amd64 3.11.2-6+deb12u2 [573 kB]
Get:10 http://deb.debian.org/debian bookworm/main amd64 libpython3-stdlib amd64 3.11.2-1+b1 [9312 B]
Get:11 http://deb.debian.org/debian bookworm/main amd64 python3 amd64 3.11.2-1+b1 [26.3 kB]
Get:12 https://deb.nodesource.com/node_20.x nodistro/main amd64 nodejs amd64 20.15.0-1nodesource1 [31.6 MB]
Fetched 37.1 MB in 1s (58.3 MB/s)       

root@1d90f9431f28:/# node -v
v20.15.0

root@1d90f9431f28:/# npm -v
10.7.0
