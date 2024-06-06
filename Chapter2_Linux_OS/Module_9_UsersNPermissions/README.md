# Chapter2
This module we look at User and Permissions.

# Name: Users and Permissions


# Description: 

  In this module we review User Account, Groups & Ownership and file permissions. We looked at Linux commands for managing Users and their permissions.

  There are 3 user categories:

  1. root user - unrestricted permissions (known as a super user). For administrative tasks: need to login as root user or execute commands as root (sudo command)

  2. standard user account -  regular user we create to login E.g. tom => /home/tom

  3. service account - relevant on linxu server distros - may have an apache server, mysql server. Each service will gets its own user. E.g. mysql user will start mysql application. Best practice for security. Don't run services as root user.

  Companies use Centrally Managed User Permission

  Allows you to login to any hardware that is connected to this machine.

  How to manage Permissions

  User Level - give permission directly

  Group Level 

  Group Users into Linux Groups
  Give Permission to the Group
  The way to go, if managing multiple users

  Groups - DevOps, Admin, Developer

  Users are added to the group
  Permissions for that group are inherited
  Users can be added to multiple groups in some cases

  /etc/passwd

  stores user account information
  everyone can read it, but only root can change the file

  username:password:uid:gid:gecos:homedir:shell

  each user has a unique uid, uid 0 is reserved for root

 Create new user

 Automatically chooses policy-conformant UID and GID values
 Automatically creates a home directory with skeletal configuration

  sudo adduser tom

  tom:x:1001:1001:tom,,,:/home/tom:/bin/bash
  
  Change password

  root@web-server:~# passwd tom

  su - tom, to switch user to tom

  su - to login to root

  Create Group

  sudo groupadd devops

  By default, system assigns the next available GID from the range of group IDs specified in the login.defs file

 Different User & Group Commands

 adduser addgroup versus useradd group add

 interactive              you need to provide the info yourself

 more user friendly

 chooses conformant UID and GID values
 creates a home directory with skelatal config automatically
 asks for input in interactive mode

 Use useradd and groupadd when writing scripts

 Use adduser and addgroup when creating manually

 Same applies to Delete

 Use userdel when writing scripts

 use deluser when deleting manually

Modify User

Add tom to devops

sudo usermod -g devops tom

Add tom to multiple groups

sudo usermod -G admin tom

sudo usermod -aG newgroup tom

Display group tom belongs to

groups tom

groups - show group of user currently logged in

Add new user to new group

adduser -G devops Nicole

-G create user with multiple seccondary groups
-d custome directory
and other options for shell, etc

Remove user from group

sudo gpasswd -d nicole devops


sudo delgroup tom

# Usage

root@web-server:~# cat /etc/passwd
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/usr/sbin/nologin
man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
news:x:9:9:news:/var/spool/news:/usr/sbin/nologin
uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin
proxy:x:13:13:proxy:/bin:/usr/sbin/nologin
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin
irc:x:39:39:ircd:/var/run/ircd:/usr/sbin/nologin
gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin
nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
systemd-network:x:100:102:systemd Network Management,,,:/run/systemd:/usr/sbin/nologin
systemd-resolve:x:101:103:systemd Resolver,,,:/run/systemd:/usr/sbin/nologin
systemd-timesync:x:102:104:systemd Time Synchronization,,,:/run/systemd:/usr/sbin/nologin
messagebus:x:103:106::/nonexistent:/usr/sbin/nologin
syslog:x:104:110::/home/syslog:/usr/sbin/nologin
_apt:x:105:65534::/nonexistent:/usr/sbin/nologin
tss:x:106:111:TPM software stack,,,:/var/lib/tpm:/bin/false
uuidd:x:107:112::/run/uuidd:/usr/sbin/nologin
tcpdump:x:108:113::/nonexistent:/usr/sbin/nologin
sshd:x:109:65534::/run/sshd:/usr/sbin/nologin
landscape:x:110:115::/var/lib/landscape:/usr/sbin/nologin
pollinate:x:111:1::/var/cache/pollinate:/bin/false
systemd-coredump:x:999:999:systemd Core Dumper:/:/usr/sbin/nologin
lxd:x:998:100::/var/snap/lxd/common/lxd:/bin/false
mongodb:x:112:65534::/home/mongodb:/usr/sbin/nologin
michael:x:1000:1000:michael,,,:/home/michael:/bin/bash

root@web-server:~# adduser tom
Adding user `tom' ...
Adding new group `tom' (1001) ...
Adding new user `tom' (1001) with group `tom' ...
Creating home directory `/home/tom' ...
Copying files from `/etc/skel' ...
New password: 
Retype new password: 
passwd: password updated successfully
Changing the user information for tom
Enter the new value, or press ENTER for the default
	Full Name []: tom
	Room Number []: 
	Work Phone []: 
	Home Phone []: 
	Other []: 
Is the information correct? [Y/n] Y

root@web-server:~# passwd tom
New password: 
Retype new password: 
passwd: password updated successfully

root@web-server:~# sudo groupadd devops

root@web-server:~# cat /etc/group

devops:x:1002:


root@web-server:~# sudo usermod -g devops tom
root@web-server:~# delgroup tom
Removing group `tom' ...