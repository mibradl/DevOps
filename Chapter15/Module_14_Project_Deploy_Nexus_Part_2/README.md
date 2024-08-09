# Chapter15
This module we will explore Part 2 of Automating Nexus Deployment

# Name: Automate Nexus Deployment 

# Description: 

Obectives:

    adduser nexus
    chown -R nexus:nexus nexus-3.65.0-02
    chown -R nexus:nexus sonatype-work

Create Nexus User

    Make Nexus User Owner

        Create Nexus group

        Create Nexus user with group

        "group" Module

        Manage presence of groups on a host

        For Windows targets: "win_group" module


        "user" Module

        Manage user accounts and user attributes

        For Windows targets: "win_user" module

        drwxr-xr-x 11 nexus nexus      4096 Aug  7 02:35 nexus



Write 4th Play

    vim nexus-3.65.0-02/bin/nexus.rc
    run_as_user="nexus"

    su - nexus
    /opt/nexus-3.65.0-02/bin/nexus start

    ps aux | grep nexus
    netstat -lnpt


    "blockinfile" module

    Insert/update/remove a multi-line text surrounded by customizable marker lines


    root@ubuntu-s-2vcpu-4gb-nyc1-01:~# cat /opt/nexus-3.70.1-02/bin/nexus.rc
    #run_as_user=""
    # BEGIN ANSIBLE MANAGED BLOCK
    run_as_user="nexus"
    # END ANSIBLE MANAGED BLOCK





    









# Usage


root@ubuntu-s-2vcpu-4gb-nyc1-01:~# ls -l /opt
total 250056
drwxr-xr-x  4 root root      4096 Aug  6 23:42 digitalocean
drwxr-xr-x 11 root root      4096 Aug  7 02:35 nexus
drwxr-xr-x 10 root root      4096 Aug  7 02:48 nexus-3.70.1-02
-rw-r--r--  1 root root 256037928 Aug  7 01:06 nexus-3.70.1-02-unix.tar.gz
drwxr-xr-x  3 root root      4096 Aug  7 01:11 sonatype-work

Change group and ownership to nexus

root@ubuntu-s-2vcpu-4gb-nyc1-01:~# ls -l /opt
total 250056
drwxr-xr-x  4 root  root       4096 Aug  6 23:42 digitalocean
drwxr-xr-x 11 nexus nexus      4096 Aug  7 02:35 nexus
drwxr-xr-x 10 root  root       4096 Aug  7 02:48 nexus-3.70.1-02
-rw-r--r--  1 root  root  256037928 Aug  7 01:06 nexus-3.70.1-02-unix.tar.gz
drwxr-xr-x  3 nexus nexus      4096 Aug  7 01:11 sonatype-work


PLAY [Install Java and net-tools] ********************************************************************************************************************************

TASK [Gathering Facts] *******************************************************************************************************************************************
ok: [157.230.234.229]

TASK [Update apt repo and cache] *********************************************************************************************************************************
ok: [157.230.234.229]

TASK [Install Java 8] ********************************************************************************************************************************************
ok: [157.230.234.229]

TASK [Install Java 8] ********************************************************************************************************************************************
ok: [157.230.234.229]

PLAY [Download and unpack nexus installer] ***********************************************************************************************************************

TASK [Gathering Facts] *******************************************************************************************************************************************
ok: [157.230.234.229]

TASK [Download Nexus] ********************************************************************************************************************************************
ok: [157.230.234.229]

TASK [untar nexus installer] *************************************************************************************************************************************
changed: [157.230.234.229]

TASK [find nexus folder] *****************************************************************************************************************************************
ok: [157.230.234.229]

TASK [Check if nexus folder stats] *******************************************************************************************************************************
ok: [157.230.234.229]

TASK [Rename nexus folder] ***************************************************************************************************************************************
skipping: [157.230.234.229]

PLAY [Create nexus user to own nexus] ****************************************************************************************************************************

TASK [Gathering Facts] *******************************************************************************************************************************************
ok: [157.230.234.229]

TASK [Ensure nexus group exists] *********************************************************************************************************************************
ok: [157.230.234.229]

TASK [Create nexus user] *****************************************************************************************************************************************
ok: [157.230.234.229]

TASK [Make nexus group and user owner] ***************************************************************************************************************************
ok: [157.230.234.229]

TASK [Make nexus user group and user owner] **********************************************************************************************************************
changed: [157.230.234.229]

PLAY [Start nexus with nexus user] *******************************************************************************************************************************

TASK [Gathering Facts] *******************************************************************************************************************************************
ok: [157.230.234.229]

TASK [Set run_as_user nexus] *************************************************************************************************************************************
changed: [157.230.234.229]

PLAY RECAP *******************************************************************************************************************************************************
157.230.234.229            : ok=16   changed=3    unreachable=0    failed=0    skipped=1    rescued=0    ignored=0 
    