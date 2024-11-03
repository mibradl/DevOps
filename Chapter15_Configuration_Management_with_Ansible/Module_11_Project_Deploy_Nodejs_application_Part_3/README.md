# Chapter15
This module we will continue the automating of the Node App Deployment

# Name: Automate Node App Deployment Part 3

# Description: 

Continue the Automation of Node App Deployment

We've been running every thing as root user

As a security best practice, it is better to create user for each app or each team member

Every application should have it's own user

We will need to:

    Create a new user

    Run App using that new user


Privilege escalation: become

'become_user' = set to user with desired privileges

Default is root

'become' allows you to become another user, different from the user that logged into the machine

TASK [Print app_status output] ***********************************************************************************************************************************
ok: [198.211.105.187] => {
    "msg": [
        "michael    23384 60.3 12.7 612724 59892 ?        Sl   11:33   0:00 node server",
        "michael    23406  0.0  0.3   2800  1792 pts/2    S+   11:33   0:00 /bin/sh -c ps aux |grep node",
        "michael    23408  0.0  0.4   7076  2048 pts/2    S+   11:33   0:00 grep node"
    ]
}


$ ls -l
total 4
drwxr-xr-x 4 michael admin 4096 Aug  5 11:33 package




# Usage

michaelbradley@Michaels-iMac-2.attlocal.net /Users/michaelbradley/ansible 
% ansible-playbook -i hosts deploy-node.yaml
/usr/local/Cellar/ansible/10.1.0/libexec/lib/python3.12/site-packages/paramiko/pkey.py:100: CryptographyDeprecationWarning: TripleDES has been moved to cryptography.hazmat.decrepit.ciphers.algorithms.TripleDES and will be removed from this module in 48.0.0.
  "cipher": algorithms.TripleDES,
/usr/local/Cellar/ansible/10.1.0/libexec/lib/python3.12/site-packages/paramiko/transport.py:259: CryptographyDeprecationWarning: TripleDES has been moved to cryptography.hazmat.decrepit.ciphers.algorithms.TripleDES and will be removed from this module in 48.0.0.
  "class": algorithms.TripleDES,

PLAY [Install node and npm] **************************************************************************************************************************************

TASK [Gathering Facts] *******************************************************************************************************************************************
ok: [198.211.105.187]

TASK [Update apt repo and cache] *********************************************************************************************************************************
changed: [198.211.105.187]

TASK [Install nodejs and npm] ************************************************************************************************************************************
ok: [198.211.105.187]

PLAY [Create new Linux user for node app] ************************************************************************************************************************

TASK [Gathering Facts] *******************************************************************************************************************************************
ok: [198.211.105.187]

TASK [Create Linux user] *****************************************************************************************************************************************
changed: [198.211.105.187]

PLAY [Deploy nodejs app] *****************************************************************************************************************************************

TASK [Gathering Facts] *******************************************************************************************************************************************
[WARNING]: Module remote_tmp /home/michael/.ansible/tmp did not exist and was created with a mode of 0700, this may cause issues when running as another user. To
avoid this, create the remote_tmp dir with the correct permissions manually
ok: [198.211.105.187]

TASK [Unpack the nodejs file] ************************************************************************************************************************************
changed: [198.211.105.187]

TASK [Install dependencies] **************************************************************************************************************************************
changed: [198.211.105.187]

TASK [Start the application] *************************************************************************************************************************************
changed: [198.211.105.187]

TASK [Ensure app is running] *************************************************************************************************************************************
changed: [198.211.105.187]

TASK [Print app_status output] ***********************************************************************************************************************************
ok: [198.211.105.187] => {
    "msg": [
        "michael    23384 60.3 12.7 612724 59892 ?        Sl   11:33   0:00 node server",
        "michael    23406  0.0  0.3   2800  1792 pts/2    S+   11:33   0:00 /bin/sh -c ps aux |grep node",
        "michael    23408  0.0  0.4   7076  2048 pts/2    S+   11:33   0:00 grep node"
    ]
}

PLAY RECAP *******************************************************************************************************************************************************
198.211.105.187            : ok=11   changed=6    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
    

root@ubuntu-s-1vcpu-512mb-10gb-nyc1-01:~# sudo su - michael

$ whoami
michael

$ ps aux |grep node
michael    23384  0.2 12.1 611956 56940 ?        Sl   11:33   0:00 node server
michael    23450  0.0  0.4   7076  2048 pts/1    S+   11:37   0:00 grep node

$ ls -l
total 4
drwxr-xr-x 4 michael admin 4096 Aug  5 11:33 package