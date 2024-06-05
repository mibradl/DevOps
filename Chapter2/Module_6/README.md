# Chapter2
This module covered the basic use of CLI commands

# Name: Basic Linux Commands


# Description: 

  Some of the linux commands covered

# Technologies Used:

  Linux CLI

# Usage

  
  cp
  pwd
  clear 
  mv
  ls
  ls -a
  ls -R
  touch
  uname -a
  cat /etc/os-release
  mkdir
  lscpu
  lsmem
  sudo adduser michael

  sudo adduser michael
Adding user `michael' ...
Adding new group `michael' (1000) ...
Adding new user `michael' (1000) with group `michael' ...
Creating home directory `/home/michael' ...
Copying files from `/etc/skel' ...
New password: 
Retype new password: 
passwd: password updated successfully
Changing the user information for michael
Enter the new value, or press ENTER for the default
	Full Name []: michael
	Room Number []: 
	Work Phone []: 
	Home Phone []: 
	Other []: 
Is the information correct? [Y/n]

root@web-server:~# su - michael
michael@web-server:~$

michaelbradley@Michaels-iMac-2 DevOps % sudo su -  
Password:
Michaels-iMac-2:~ root#

root@web-server:~# history
  217  cat /etc/os-release 
  218  pwd
  219  ls
  220  cd ~
  221  ls
  222  sudo adduser
  223  sudo adduser admin
  224  sudo adduser michael
  225  clear
  226  su - michael
  227  history





