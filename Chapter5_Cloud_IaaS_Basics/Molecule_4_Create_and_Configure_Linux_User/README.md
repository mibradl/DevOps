# Chapter5
This module we cover creating Linux User in the Cloud.

# Name: Create a Linux User

# Description: 

Create a separate Linux User that is not Root

    As a security best practice you should not execute services with root.

    Security Best Practice

        Create separate User for every application

        Give it only the permission it needs to run that App

        Don't work with the Root user!

        Each user should have their own user account and privileges should be escalated as needed


    Add specific user

    root@ubuntu-s-2vcpu-4gb-amd-nyc3-01:~# adduser mike

    To allow user to execute some of the commands as root:

        Add your User to "sudo" group

        




# Usage


    root@ubuntu-s-2vcpu-4gb-amd-nyc3-01:~# adduser mike

    info: Adding user `mike' ...
    info: Selecting UID/GID from range 1000 to 59999 ...
    info: Adding new group `mike' (1000) ...
    info: Adding new user `mike' (1000) with group `mike (1000)' ...
    info: Creating home directory `/home/mike' ...
    info: Copying files from `/etc/skel' ...
    New password: 
    Retype new password: 
    passwd: password updated successfully
    Changing the user information for mike
    Enter the new value, or press ENTER for the default
	Full Name []: Michael
	Room Number []: 
	Work Phone []: 
	Home Phone []: 
	Other []: 
    Is the information correct? [Y/n] Y
    info: Adding new user `mike' to supplemental / extra groups `users' ...
    info: Adding user `mike' to group `users' ...

