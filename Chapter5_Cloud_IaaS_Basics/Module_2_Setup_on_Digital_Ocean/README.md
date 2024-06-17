# Chapter5
This module we will setup server on a Digital Ocean platform.

# Name: Create a Droplet Server on Digital Ocean

# Description: 

Create a Droplet

    Based on Linux Ubuntu

Pre-Requisites:

    Create a Digital Ocean account

        Select Create Droplet

        Choose Region closest to you: New York

        Choose an Image : Ubuntu

            Version defaulted to latest: 24.04(LTS)x64

        Choose Size: Basic

        CPU options: Regular, Minimal $4, 512 MB 1 CPU, 10 GB SSD Disk, 500 GB transfer



        Choose Authentication Method

            Add Public SSH key * we will use this option for the exercise

                Select SSH Key

                Copy your public key - cat ~/.ssh/id_rsa.pub

                Paste into the panel and give it a descriptive name - my-droplet-key

                Click Create Droplet

            Password - connect to your Droplet as the "root" user via password

            Settings >> Security will allow you to view your SSH Keys

            Click Droplet - to view status of the machine 

                164.90.128.228 - this is the public IP address (currently it is not secured)

                Having all ports open is a bad security practice

            Need to Configure Firewall

            We will explicitly allow access on specific ports

            Configure Firewall = Closing access by default

                Select Networking to set firewall rules

                    No Firewall rules applied to this Droplet

                Select Edit

                Update SSH rule to explicitly only allow local IP address 

                    192.168.1.250

                    Note: this is set for Inbound = Incoming Requests

                Save

            Click Networking and Firewall Address

                Select Firewall - Assign to Droplet - my-droplet-firewall to Droplet name

                ubuntu-s-2vcpu-4gb-amd-nyc3-01


            Outbound = outgoing requests from server

                This opens the server to the internet

            
            Connect to droplet from local terminal

            ssh root@164.90.128.228 and reply yes for first time access.

        Install Java on Droplet


            root@ubuntu-s-2vcpu-4gb-amd-nyc3-01:~# apt install openjdk-8-jre-headless

            root@ubuntu-s-2vcpu-4gb-amd-nyc3-01:~# java -showversion
            openjdk version "1.8.0_412"
            OpenJDK Runtime Environment (build 1.8.0_412-8u412-ga-1~24.04.2-b08)
            OpenJDK 64-Bit Server VM (build 25.412-b08, mixed mode)







# Usage

michaelbradley@Michaels-iMac-2 .ssh % ls
docker-server.pem       id_rsa                  id_rsa.pub              known_hosts             known_hosts.old

michaelbradley@Michaels-iMac-2 .ssh % cat ~/.ssh/id_rsa.pub 
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDOV9mSXpfn/ozmOHYDgZ0FA21zs9+ILja53WWC4+Q4lXwV8babAwO+7YkLNBK1BLIhmSDfq27sw23NSXhRAuo2hKVg78G/AQW7YECCL8oU3dkDskQklpvyoftjXl/IfUlKepzVEDbC8m/glvpNd7kcItu+nP4M0HcVLQ4pdFWVqZK7xpa2oAcvc0Mekb6Wm/6BHPQ/91BpXn9lzc8nrnjdh5u80YC55C+ebXCvCthrsZzWnveQBaUhhvU6vlfTmsW05kZlQtOoc/KwZcj4ZPzf5j4bHMdFW/KKonKcIjOPmg1U2dsdmPF/XzrGPhUU1xXqBaDNlUuO8Kfh1hCOOVr1 michaelbradley@Michaels-iMac-2.local

michaelbradley@Michaels-iMac-2 DevOps % ssh root@164.90.128.228
The authenticity of host '164.90.128.228 (164.90.128.228)' can't be established.
ED25519 key fingerprint is SHA256:ycCKWvbTY3v9999NxbJBbTGQge4PoDN4gZQqsbKgBoc.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '164.90.128.228' (ED25519) to the list of known hosts.
Welcome to Ubuntu 24.04 LTS (GNU/Linux 6.8.0-31-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/pro

 System information as of Mon Jun 17 17:46:51 UTC 2024

  System load:  0.0               Processes:             107
  Usage of /:   1.8% of 76.45GB   Users logged in:       0
  Memory usage: 5%                IPv4 address for eth0: 164.90.128.228
  Swap usage:   0%                IPv4 address for eth0: 10.17.0.5

Expanded Security Maintenance for Applications is not enabled.

73 updates can be applied immediately.
18 of these updates are standard security updates.
To see these additional updates run: apt list --upgradable

Enable ESM Apps to receive additional future security updates.
See https://ubuntu.com/esm or run: sudo pro status



The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

root@ubuntu-s-2vcpu-4gb-amd-nyc3-01:~#

root@ubuntu-s-2vcpu-4gb-amd-nyc3-01:~# apt update
Hit:1 http://mirrors.digitalocean.com/ubuntu noble InRelease
Get:2 http://mirrors.digitalocean.com/ubuntu noble-updates InRelease [126 kB]
Hit:3 https://repos-droplet.digitalocean.com/apt/droplet-agent main InRelease  
Hit:4 http://security.ubuntu.com/ubuntu noble-security InRelease               
Hit:5 http://mirrors.digitalocean.com/ubuntu noble-backports InRelease
Get:6 http://mirrors.digitalocean.com/ubuntu noble-updates/main amd64 Packages [176 kB]
Get:7 http://mirrors.digitalocean.com/ubuntu noble-updates/main Translation-en [48.6 kB]
Get:8 http://mirrors.digitalocean.com/ubuntu noble-updates/universe amd64 Packages [66.7 kB]
Get:9 http://mirrors.digitalocean.com/ubuntu noble-updates/universe Translation-en [25.1 kB]
Fetched 442 kB in 7s (64.8 kB/s)
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
28 packages can be upgraded. Run 'apt list --upgradable' to see them.
root@ubuntu-s-2vcpu-4gb-amd-nyc3-01:~# 

root@ubuntu-s-2vcpu-4gb-amd-nyc3-01:~# apt install openjdk-8-jre-headless
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
The following additional packages will be installed:
  ca-certificates-java fontconfig-config fonts-dejavu-core fonts-dejavu-mono java-common libavahi-client3 libavahi-common-data libavahi-common3 libcups2t64 libfontconfig1 libjpeg-turbo8 libjpeg8
  liblcms2-2 libpcsclite1 libxi6 libxrender1 libxtst6 x11-common
Suggested packages:
  default-jre cups-common liblcms2-utils pcscd libnss-mdns fonts-dejavu-extra fonts-nanum fonts-ipafont-gothic fonts-ipafont-mincho fonts-wqy-microhei fonts-wqy-zenhei fonts-indic
The following NEW packages will be installed:
  ca-certificates-java fontconfig-config fonts-dejavu-core fonts-dejavu-mono java-common libavahi-client3 libavahi-common-data libavahi-common3 libcups2t64 libfontconfig1 libjpeg-turbo8 libjpeg8
  liblcms2-2 libpcsclite1 libxi6 libxrender1 libxtst6 openjdk-8-jre-headless x11-common
0 upgraded, 19 newly installed, 0 to remove and 28 not upgraded.
Need to get 33.0 MB of archives.
After this operation, 111 MB of additional disk space will be used.
Do you want to continue? [Y/n] Y


root@ubuntu-s-2vcpu-4gb-amd-nyc3-01:~# java -showversion
openjdk version "1.8.0_412"
OpenJDK Runtime Environment (build 1.8.0_412-8u412-ga-1~24.04.2-b08)
OpenJDK 64-Bit Server VM (build 25.412-b08, mixed mode)





root@ubuntu-s-2vcpu-4gb-amd-nyc3-01:~# java
Usage: java [-options] class [args...]
           (to execute a class)
   or  java [-options] -jar jarfile [args...]
           (to execute a jar file)
where options include:
    -d32	  use a 32-bit data model if available
    -d64	  use a 64-bit data model if available
    -server	  to select the "server" VM
    -zero	  to select the "zero" VM
    -dcevm	  to select the "dcevm" VM
                  The default VM is server,
                  because you are running on a server-class machine.


    -cp <class search path of directories and zip/jar files>
    -classpath <class search path of directories and zip/jar files>
                  A : separated list of directories, JAR archives,
                  and ZIP archives to search for class files.
    -D<name>=<value>
                  set a system property
    -verbose:[class|gc|jni]
                  enable verbose output
    -version      print product version and exit
    -version:<value>
                  Warning: this feature is deprecated and will be removed
                  in a future release.
                  require the specified version to run
    -showversion  print produc