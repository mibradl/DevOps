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

        root@ubuntu-s-2vcpu-4gb-amd-nyc3-01:~# usermod -aG sudo mike

        root@ubuntu-s-2vcpu-4gb-amd-nyc3-01:~# su - mike
        To run a command as administrator (user "root"), use "sudo <command>".
        See "man sudo_root" for details.

        mike@ubuntu-s-2vcpu-4gb-amd-nyc3-01:~$

        mike@ubuntu-s-2vcpu-4gb-amd-nyc3-01:~$ pwd
        /home/mike

"$ = standard Linux User"

"# = Root User"

        michaelbradley@Michaels-iMac-2 DevOps % ssh mike@164.90.128.228
        
        mike@164.90.128.228: Permission denied (publickey).

        need to provide ssh key for mike

        mike@ubuntu-s-2vcpu-4gb-amd-nyc3-01:~$ mkdir -pv .ssh

        mkdir: created directory '.ssh'

        mike@ubuntu-s-2vcpu-4gb-amd-nyc3-01:~$ sudo vim ~/.ssh/authorized_keys
        [sudo] password for mike:

Now that the id_rsa.pub key has been copied to authorized_keys on the remote server "mike" has access via ssh.

        michaelbradley@Michaels-iMac-2 DevOps % ssh mike@164.90.128.228

            Welcome to Ubuntu 24.04 LTS (GNU/Linux 6.8.0-31-generic x86_64)

            * Documentation:  https://help.ubuntu.com
            * Management:     https://landscape.canonical.com
            * Support:        https://ubuntu.com/pro

            System information as of Mon Jun 17 20:02:44 UTC 2024

            System load:  0.08              Processes:             111
            Usage of /:   2.0% of 76.45GB   Users logged in:       0
            Memory usage: 9%                IPv4 address for eth0: 164.90.128.228
            Swap usage:   0%                IPv4 address for eth0: 10.17.0.5

            Expanded Security Maintenance for Applications is not enabled.

            67 updates can be applied immediately.
            18 of these updates are standard security updates.
            To see these additional updates run: apt list --upgradable

            Enable ESM Apps to receive additional future security updates.
            See https://ubuntu.com/esm or run: sudo pro status



            The programs included with the Ubuntu system are free software;
            the exact distribution terms for each program are described in the
            individual files in /usr/share/doc/*/copyright.

            Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
            applicable law.

            mike@ubuntu-s-2vcpu-4gb-amd-nyc3-01:~$ 

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

    root@ubuntu-s-2vcpu-4gb-amd-nyc3-01:~# su - mike

    To run a command as administrator (user "root"), use "sudo <command>".
    See "man sudo_root" for details.

    mike@ubuntu-s-2vcpu-4gb-amd-nyc3-01:~$


    michaelbradley@Michaels-iMac-2 DevOps % ssh mike@164.90.128.228

        Welcome to Ubuntu 24.04 LTS (GNU/Linux 6.8.0-31-generic x86_64)

        * Documentation:  https://help.ubuntu.com
        * Management:     https://landscape.canonical.com
        * Support:        https://ubuntu.com/pro

        System information as of Mon Jun 17 20:02:44 UTC 2024

        System load:  0.08              Processes:             111
        Usage of /:   2.0% of 76.45GB   Users logged in:       0
        Memory usage: 9%                IPv4 address for eth0: 164.90.128.228
        Swap usage:   0%                IPv4 address for eth0: 10.17.0.5

        Expanded Security Maintenance for Applications is not enabled.

        67 updates can be applied immediately.
        18 of these updates are standard security updates.
        To see these additional updates run: apt list --upgradable

        Enable ESM Apps to receive additional future security updates.
        See https://ubuntu.com/esm or run: sudo pro status



        The programs included with the Ubuntu system are free software;
        the exact distribution terms for each program are described in the
        individual files in /usr/share/doc/*/copyright.

        Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
        applicable law.

        mike@ubuntu-s-2vcpu-4gb-amd-nyc3-01:~$