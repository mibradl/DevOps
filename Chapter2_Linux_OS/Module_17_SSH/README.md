# Chapter2
This module we reviewed the concepts of SSH and how to configure it.

# Name: SSH - Secure Shell

# Description: 

Secure allows for a secure way to connect to a machine over the internet. SSH refers also to the suite of utilities that implement that protocol.

There are two ways to authenticate over SSH:

1. Username & Password
    Admin creates User on remote server
    User can then connect with the username and password

2. SSH Key Pair
    Client create an SSH Key Pair - Key Pair = Private Key + Public Key
    Private Key = Secret key is stored securely on the client machine
    Public Key = Public key can be shared, e.g. with the remote server

    Client machine for that public key can safely connect

    If public key is not registered on the remote server you can not connect to it.

    SSH for Services

    Services like Jenkins often need to connect to  another server via SSH
    
    Create Jenkins User on application server

    Create SSH Key Pair on Jenkins server

    Add public SSH key to authorized_keys on application server

    Firewall and Port 22

    Communication must be explicitly allowed through Firewall rule

    By default it's blocked

    SSH authentication comes after the connection

    SSH service runs by default on machine

    By default, SSH server listens on port 22

    In firewall rule we allow access on port 22

    SSH is powerful and needs to be restricted to specific IP addresses

    known_hosts - lets the client authenticate the server to check that it isn't connecting to an impersonator

    authorized_keys - lets the server authenticate the user




# Usage

Create Remote Server on Cloud Platform

michaelbradley@Michaels-iMac-2 ~ % ssh root@161.35.110.183
root@161.35.110.183's password: 
Welcome to Ubuntu 20.04.4 LTS (GNU/Linux 5.4.0-122-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Wed Jun 12 01:59:51 UTC 2024

  System load:  0.0               Users logged in:       0
  Usage of /:   6.3% of 77.35GB   IPv4 address for eth0: 161.35.110.183
  Memory usage: 14%               IPv4 address for eth0: 10.10.0.5
  Swap usage:   0%                IPv4 address for eth1: 10.116.0.2
  Processes:    132

63 updates can be applied immediately.
1 of these updates is a standard security update.
To see these additional updates run: apt list --upgradable


1 updates could not be installed automatically. For more details,
see /var/log/unattended-upgrades/unattended-upgrades.log

*** System restart required ***
Last login: Fri Jun  7 13:03:53 2024 from 45.25.106.162
root@web-server:~# 


Generate SSH Key Pair on client

ssh-keygen -t rsa

michaelbradley@Michaels-iMac-2 ~ % pwd
/Users/michaelbradley
michaelbradley@Michaels-iMac-2 ~ % ls .ssh/
docker-server.pem	id_rsa			id_rsa.pub		known_hosts		known_hosts.old
michaelbradley@Michaels-iMac-2 ~ % 

Add Public Key to authorized_key

michaelbradley@Michaels-iMac-2 ~ % cat .ssh/id_rsa.pub 
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDOV9mSXpfn/ozmOHYDgZ0FA21zs9+ILja53WWC4+Q4lXwV8babAwO+7YkLNBK1BLIhmSDfq27sw23NSXhRAuo2hKVg78G/AQW7YECCL8oU3dkDskQklpvyoftjXl/IfUlKepzVEDbC8m/glvpNd7kcItu+nP4M0HcVLQ4pdFWVqZK7xpa2oAcvc0Mekb6Wm/6BHPQ/91BpXn9lzc8nrnjdh5u80YC55C+ebXCvCthrsZzWnveQBaUhhvU6vlfTmsW05kZlQtOoc/KwZcj4ZPzf5j4bHMdFW/KKonKcIjOPmg1U2dsdmPF/XzrGPhUU1xXqBaDNlUuO8Kfh1hCOOVr1 michaelbradley@Michaels-iMac-2.local

root@web-server:~# cat .ssh/authorized_keys 
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDOV9mSXpfn/ozmOHYDgZ0FA21zs9+ILja53WWC4+Q4lXwV8babAwO+7YkLNBK1BLIhmSDfq27sw23NSXhRAuo2hKVg78G/AQW7YECCL8oU3dkDskQklpvyoftjXl/IfUlKepzVEDbC8m/glvpNd7kcItu+nP4M0HcVLQ4pdFWVqZK7xpa2oAcvc0Mekb6Wm/6BHPQ/91BpXn9lzc8nrnjdh5u80YC55C+ebXCvCthrsZzWnveQBaUhhvU6vlfTmsW05kZlQtOoc/KwZcj4ZPzf5j4bHMdFW/KKonKcIjOPmg1U2dsdmPF/XzrGPhUU1xXqBaDNlUuO8Kfh1hCOOVr1 michaelbradley@Michaels-iMac-2.local

-i flag used to point to private key location.  default location is .ssh/ where it is searched

michaelbradley@Michaels-iMac-2 ~ % ssh -i .ssh/id_rsa @root@161.35.110.183


Copy Bash script file to Remote Server

scp (secure copoy) = allows you to securely copy files and directory. Files and password are encrypted.

michaelbradley@Michaels-iMac-2 ~ % scp test.sh root@161.35.110.183:/root
test.sh                                                                                                                                      100%   60     3.2KB/s   00:00    
michaelbradley@Michaels-iMac-2 ~ % 

root@web-server:~# ls -l |grep test.sh 
-rwxr--r-- 1 root root       60 Jun 12 02:50 test.sh

root@web-server:~# ./test.sh 
I am executed on the remote server, YAH
root@web-server:~# 







