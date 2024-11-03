# Chapter15
This module we will automate Nodejs app

# Name: Project Deploy Nodejs Application

# Description:

Automate Node App Deployment


Project Introduction - Deploy Node App


Steps:

    Create a Droplet

    Write Ansible Playbook

        Install node & npm on Server

        Copy Node artifact and unpack

        Start Application

        Verify App running successfully


Preparation:

    Create Server on Digital Ocean



Write Playbook

    Host file

    198.211.105.187 ansible_ssh_private_key_file=~/.ssh/id_rsa ansible_user=root


Install node and npm

    Install App Dependencies via npm

    Start App via Node command

    Two Plays

        ---
        - name: Install node and npm
        hosts: 198.211.105.187
        tasks:
            - name: Update apt repo and cache
            ansible.builtin.apt:
                update_cache: yes
                force_apt_get: yes
                cache_valid_time: 3600
            - name: Install nodejs and npm
            ansible.builtin.apt:
                pkg:
                - nodejs
                - npm

        - name: Deploy nodejs app
            hosts: 198.211.105.187
            tasks:
            - name: Deploy nodejs app to server
              ansible.builtin.copy:
                src: /Users/michaelbradley/ansible/nodejs-app/nodejs-app-1.0.0.tgz
                dest: /root/nodejs-app-1.0.0.tgz
            - name: Unpack the nodejs file
              ansible.builtin.unarchive:
                src: /root/nodejs-app-1.0.0.tgz
                dest: /root/
                remote_src: yes



        root@ubuntu-s-1vcpu-512mb-10gb-nyc1-01:~# ls
        nodejs-app-1.0.0.tgz  package

        root@ubuntu-s-1vcpu-512mb-10gb-nyc1-01:~# ls package/
        Dockerfile  Readme.md  app  package.json

        root@ubuntu-s-1vcpu-512mb-10gb-nyc1-01:~# pwd
        /root

        root@ubuntu-s-1vcpu-512mb-10gb-nyc1-01:~# npm -v
        9.2.0
        root@ubuntu-s-1vcpu-512mb-10gb-nyc1-01:~# nodejs -v
        v18.19.1

    Perform the same action in a single step by adding the copy src to unarchive src

        - name: Deploy nodejs app
        hosts: 198.211.105.187
        tasks:
        - name: Unpack the nodejs file
          ansible.builtin.unarchive:
            src: /Users/michaelbradley/ansible/nodejs-app/nodejs-app-1.0.0.tgz
            dest: /root/


        
        root@ubuntu-s-1vcpu-512mb-10gb-nyc1-01:~# rm nodejs-app-1.0.0.tgz 
        root@ubuntu-s-1vcpu-512mb-10gb-nyc1-01:~# rm -rf package/

        Re-run playbook

        ...
        TASK [Unpack the nodejs file] ************************************************************************************************************************************
        changed: [198.211.105.187]

        PLAY RECAP *******************************************************************************************************************************************************
        198.211.105.187            : ok=5    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   


        e

# Usage

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
changed: [198.211.105.187]

PLAY [Deploy nodejs app] *****************************************************************************************************************************************

TASK [Gathering Facts] *******************************************************************************************************************************************
ok: [198.211.105.187]

TASK [Copy nodejs app to server] *******************************************************************************************************************************
changed: [198.211.105.187]

TASK [Unpack the nodejs file] ************************************************************************************************************************************
changed: [198.211.105.187]

PLAY RECAP *******************************************************************************************************************************************************
198.211.105.187            : ok=2    changed=4    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0  



root@ubuntu-s-1vcpu-512mb-10gb-nyc1-01:~# ls
nodejs-app-1.0.0.tgz  package


Perform second run of playbook

% ansible-playbook -i hosts deploy-node.yaml
/usr/local/Cellar/ansible/10.1.0/libexec/lib/python3.12/site-packages/paramiko/pkey.py:100: CryptographyDeprecationWarning: TripleDES has been moved to cryptography.hazmat.decrepit.ciphers.algorithms.TripleDES and will be removed from this module in 48.0.0.
  "cipher": algorithms.TripleDES,
/usr/local/Cellar/ansible/10.1.0/libexec/lib/python3.12/site-packages/paramiko/transport.py:259: CryptographyDeprecationWarning: TripleDES has been moved to cryptography.hazmat.decrepit.ciphers.algorithms.TripleDES and will be removed from this module in 48.0.0.
  "class": algorithms.TripleDES,

PLAY [Install node and npm] **************************************************************************************************************************************

TASK [Gathering Facts] *******************************************************************************************************************************************
ok: [198.211.105.187]

TASK [Update apt repo and cache] *********************************************************************************************************************************
ok: [198.211.105.187]

TASK [Install nodejs and npm] ************************************************************************************************************************************
ok: [198.211.105.187]

PLAY [Deploy nodejs app] *****************************************************************************************************************************************

TASK [Gathering Facts] *******************************************************************************************************************************************
ok: [198.211.105.187]

TASK [Unpack the nodejs file] ************************************************************************************************************************************
changed: [198.211.105.187]

PLAY RECAP *******************************************************************************************************************************************************
198.211.105.187            : ok=5    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   

root@ubuntu-s-1vcpu-512mb-10gb-nyc1-01:~# ls -l
total 4
drwxr-xr-x 3 root root 4096 Aug  4 23:50 packag
    