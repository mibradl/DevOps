# Chapter15
This module we will look at using Variables in Ansible

# Name: Ansible Variables - make your Playbook customizable


# Description: 

Registered Variables

    Create variables from the output of an Ansible task

    This variable can be used in any later tasks in your play


Some modules have common return values

    E.g. "changed" = indicating if the task has to make changes


Parameterize Playbook

    Set values for test environment             Set values for prod environment


    Referencing variables

        Using double curly braces

        If you start a value with {{ value }}, you must quote the whole expression to create valid YAML syntax. This is to prevent it from being confused with a dictionary, following ":"


        src: /Users/michael/ansible/nodejs-app/nodejs-app-{{version}}.tgz     Quotes not required in this case

        src: "{{location}}/nodejs-app-{{version}}.tgz"     All must be enclosed in quotes


            - name: Deploy nodejs app
              hosts: 198.211.105.187
              become: True
              become_user: michael
              vars:
                - location: /Users/michaelbradley/ansible/nodejs-app
                - version: 1.0.0
                - destination: /home/michael
              tasks:
            - name: Unpack the nodejs file
              ansible.builtin.unarchive:
                src: "{{ node-file-location }}"
                dest: "{{destination}}"                     Good use case for variable when repeatable
              register: user_creation_result
            - ansible.builtin.debug:
                msg: "{{ user_creation_result }}"
            - name: Install dependencies
              community.general.npm:
                path: "{{destination}}/package"
            - name: Start the application
              ansible.builtin.command:
                chdir: "{{destination}}/package/app"


                
    
    
        Setting the user variable value
    
    
    
        
        ansible-playbook -i hosts deploy-node.yaml  -e "version=1.0.0" location=/Users/michaelbradley/ansible/nodejs-app name=node"


        Naming of Variables

            Not valid: Python keywords, such as async

            Playbook keywords, such as environment

            Should always start with a letter

            Don't use: linux-name, linux name, linux.name or 12


        External Variables File

            vars file

            Variables file uses YAML syntax

            So you can call it "project-vars.yaml"


            version: 1.0.0
            location: /Users/michaelbradley/ansible/nodejs-app
            linux_name: michael
            user_home_dir: /home/{{ linux_name }}


        Review of Variables

            Setting vars directly in playbook

            Setting vars at the command line

            Setting vars in an external variables file




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
ok: [198.211.105.187]

TASK [Install nodejs and npm] ************************************************************************************************************************************
ok: [198.211.105.187]

PLAY [Create new Linux user for node app] ************************************************************************************************************************

TASK [Gathering Facts] *******************************************************************************************************************************************
ok: [198.211.105.187]

TASK [Create Linux user] *****************************************************************************************************************************************
ok: [198.211.105.187]

TASK [ansible.builtin.debug] *************************************************************************************************************************************
ok: [198.211.105.187] => {
    "msg": {
        "append": false,
        "changed": false,
        "comment": "Michael Admin",
        "failed": false,
        "group": 110,
        "home": "/home/michael",
        "move_home": false,
        "name": "michael",
        "shell": "/bin/sh",
        "state": "present",
        "uid": 1000
    }
}

PLAY [Deploy nodejs app] *****************************************************************************************************************************************

TASK [Gathering Facts] *******************************************************************************************************************************************
ok: [198.211.105.187]

TASK [Unpack the nodejs file] ************************************************************************************************************************************
ok: [198.211.105.187]

TASK [ansible.builtin.debug] *************************************************************************************************************************************
ok: [198.211.105.187] => {
    "msg": {
        "changed": false,
        "dest": "/home/michael",
        "failed": false,
        "gid": 110,
        "group": "admin",
        "handler": "TgzArchive",
        "mode": "0750",
        "owner": "michael",
        "size": 4096,
        "src": "/var/tmp/ansible-tmp-1722859430.083761-72255-213259198623991/source",
        "state": "directory",
        "uid": 1000
    }
}

TASK [Install dependencies] **************************************************************************************************************************************
ok: [198.211.105.187]

TASK [Start the application] *************************************************************************************************************************************
changed: [198.211.105.187]

TASK [Ensure app is running] *************************************************************************************************************************************
changed: [198.211.105.187]

TASK [Print app_status output] ***********************************************************************************************************************************
ok: [198.211.105.187] => {
    "msg": [
        "michael    24058 53.4 12.4 612184 58484 ?        Dl   12:03   0:00 node server",
        "michael    24080  0.0  0.3   2800  1792 pts/3    S+   12:03   0:00 /bin/sh -c ps aux |grep node",
        "michael    24082  0.0  0.4   7076  2048 pts/3    S+   12:03   0:00 grep node"
    ]
}

PLAY RECAP *******************************************************************************************************************************************************
198.211.105.187            : ok=13   changed=2    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   

Setting Variables

% ansible-playbook -i hosts deploy-node.yaml    
/usr/local/Cellar/ansible/10.1.0/libexec/lib/python3.12/site-packages/paramiko/pkey.py:100: CryptographyDeprecationWarning: TripleDES has been moved to cryptography.hazmat.decrepit.ciphers.algorithms.TripleDES and will be removed from this module in 48.0.0.
  "cipher": algorithms.TripleDES,
/usr/local/Cellar/ansible/10.1.0/libexec/lib/python3.12/site-packages/paramiko/transport.py:259: CryptographyDeprecationWarning: TripleDES has been moved to cryptography.hazmat.decrepit.ciphers.algorithms.TripleDES and will be removed from this module in 48.0.0.
  "class": algorithms.TripleDES,
[DEPRECATION WARNING]: Specifying a list of dictionaries for vars is deprecated in favor of specifying a dictionary. This feature will be removed in version 
2.18. Deprecation warnings can be disabled by setting deprecation_warnings=False in ansible.cfg.

PLAY [Install node and npm] **************************************************************************************************************************************

TASK [Gathering Facts] *******************************************************************************************************************************************
ok: [198.211.105.187]

TASK [Update apt repo and cache] *********************************************************************************************************************************
ok: [198.211.105.187]

TASK [Install nodejs and npm] ************************************************************************************************************************************
ok: [198.211.105.187]

PLAY [Create new Linux user for node app] ************************************************************************************************************************

TASK [Gathering Facts] *******************************************************************************************************************************************
ok: [198.211.105.187]

TASK [Create Linux user] *****************************************************************************************************************************************
ok: [198.211.105.187]

PLAY [Deploy nodejs app] *****************************************************************************************************************************************

TASK [Gathering Facts] *******************************************************************************************************************************************
ok: [198.211.105.187]

TASK [Unpack the nodejs file] ************************************************************************************************************************************
ok: [198.211.105.187]

TASK [Install dependencies] **************************************************************************************************************************************
ok: [198.211.105.187]

TASK [Start the application] *************************************************************************************************************************************
changed: [198.211.105.187]

TASK [Ensure app is running] *************************************************************************************************************************************
changed: [198.211.105.187]

TASK [Print app_status output] ***********************************************************************************************************************************
ok: [198.211.105.187] => {
    "msg": [
        "michael    34726 55.8 12.6 612468 59380 ?        Rl   03:01   0:00 node server",
        "michael    34748  0.0  0.3   2800  1792 pts/1    S+   03:01   0:00 /bin/sh -c ps aux |grep node",
        "michael    34750  0.0  0.4   7076  2048 pts/1    S+   03:01   0:00 grep node"
    ]
}

PLAY RECAP *******************************************************************************************************************************************************
198.211.105.187            : ok=11   changed=2    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   


Add variables for name and setting on command line

 ansible-playbook -i hosts deploy-node.yaml  -e "version=1.0.0" location=/Users/michaelbradley/ansible/nodejs-app name=node"   
dquote> 
michaelbradley@Michaels-iMac-2.attlocal.net /Users/michaelbradley/ansible 
% ansible-playbook -i hosts deploy-node.yaml  -e "version=1.0.0 location=/Users/michaelbradley/ansible/nodejs-app name=node" 
/usr/local/Cellar/ansible/10.1.0/libexec/lib/python3.12/site-packages/paramiko/pkey.py:100: CryptographyDeprecationWarning: TripleDES has been moved to cryptography.hazmat.decrepit.ciphers.algorithms.TripleDES and will be removed from this module in 48.0.0.
  "cipher": algorithms.TripleDES,
/usr/local/Cellar/ansible/10.1.0/libexec/lib/python3.12/site-packages/paramiko/transport.py:259: CryptographyDeprecationWarning: TripleDES has been moved to cryptography.hazmat.decrepit.ciphers.algorithms.TripleDES and will be removed from this module in 48.0.0.
  "class": algorithms.TripleDES,
[DEPRECATION WARNING]: Specifying a list of dictionaries for vars is deprecated in favor of specifying a dictionary. This feature will be removed in version 
2.18. Deprecation warnings can be disabled by setting deprecation_warnings=False in ansible.cfg.
[WARNING]: Found variable using reserved name: name

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
[WARNING]: Module remote_tmp /home/node/.ansible/tmp did not exist and was created with a mode of 0700, this may cause issues when running as another user. To
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
        "node       41169  0.0  4.0  38836 18924 ?        S    11:54   0:00 /usr/bin/python3 /var/tmp/ansible-tmp-1722945255.6454132-5154-94318957313781/async_wrapper.py j138609846269 1000 /var/tmp/ansible-tmp-1722945255.6454132-5154-94318957313781/AnsiballZ_command.py _",
        "node       41170  0.0  4.1  38836 19308 ?        S    11:54   0:00 /usr/bin/python3 /var/tmp/ansible-tmp-1722945255.6454132-5154-94318957313781/async_wrapper.py j138609846269 1000 /var/tmp/ansible-tmp-1722945255.6454132-5154-94318957313781/AnsiballZ_command.py _",
        "node       41171 18.7  5.6  38832 26320 ?        S    11:54   0:00 /usr/bin/python3 /var/tmp/ansible-tmp-1722945255.6454132-5154-94318957313781/AnsiballZ_command.py",
        "node       41184 60.3 12.8 612692 60140 ?        Sl   11:54   0:00 node server",
        "root       41201  0.0  0.3   2800  1664 pts/0    Ss+  11:54   0:00 /bin/sh -c sudo -H -S -n  -u node /bin/sh -c 'echo BECOME-SUCCESS-mdxowvtyxpjlbksntucjtlbnmuadvlwm ; /usr/bin/python3 /var/tmp/ansible-tmp-1722945257.1243641-5166-20209016370322/AnsiballZ_command.py' && sleep 0",
        "root       41202  0.0  1.4  16936  6912 pts/0    S+   11:54   0:00 sudo -H -S -n -u node /bin/sh -c echo BECOME-SUCCESS-mdxowvtyxpjlbksntucjtlbnmuadvlwm ; /usr/bin/python3 /var/tmp/ansible-tmp-1722945257.1243641-5166-20209016370322/AnsiballZ_command.py",
        "root       41203  0.0  0.5  16936  2504 pts/1    Ss   11:54   0:00 sudo -H -S -n -u node /bin/sh -c echo BECOME-SUCCESS-mdxowvtyxpjlbksntucjtlbnmuadvlwm ; /usr/bin/python3 /var/tmp/ansible-tmp-1722945257.1243641-5166-20209016370322/AnsiballZ_command.py",
        "node       41204  0.0  0.3   2800  1664 pts/1    S+   11:54   0:00 /bin/sh -c echo BECOME-SUCCESS-mdxowvtyxpjlbksntucjtlbnmuadvlwm ; /usr/bin/python3 /var/tmp/ansible-tmp-1722945257.1243641-5166-20209016370322/AnsiballZ_command.py",
        "node       41205 48.4  5.6  38812 26320 pts/1    S+   11:54   0:00 /usr/bin/python3 /var/tmp/ansible-tmp-1722945257.1243641-5166-20209016370322/AnsiballZ_command.py",
        "node       41206  0.0  0.3   2800  1792 pts/1    S+   11:54   0:00 /bin/sh -c ps aux |grep node",
        "node       41207  0.0  0.9  11320  4352 pts/1    R+   11:54   0:00 ps aux",
        "node       41208  0.0  0.4   7076  2048 pts/1    S+   11:54   0:00 grep node"
    ]
}

PLAY RECAP *******************************************************************************************************************************************************
198.211.105.187            : ok=11   changed=6    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0 


Results from project-vars

% ansible-playbook -i hosts deploy-node.yaml
/usr/local/Cellar/ansible/10.1.0/libexec/lib/python3.12/site-packages/paramiko/pkey.py:100: CryptographyDeprecationWarning: TripleDES has been moved to cryptography.hazmat.decrepit.ciphers.algorithms.TripleDES and will be removed from this module in 48.0.0.
  "cipher": algorithms.TripleDES,
/usr/local/Cellar/ansible/10.1.0/libexec/lib/python3.12/site-packages/paramiko/transport.py:259: CryptographyDeprecationWarning: TripleDES has been moved to cryptography.hazmat.decrepit.ciphers.algorithms.TripleDES and will be removed from this module in 48.0.0.
  "class": algorithms.TripleDES,
[DEPRECATION WARNING]: Specifying a list of dictionaries for vars is deprecated in favor of specifying a dictionary. This feature will be removed in version 
2.18. Deprecation warnings can be disabled by setting deprecation_warnings=False in ansible.cfg.

PLAY [Install node and npm] **************************************************************************************************************************************

TASK [Gathering Facts] *******************************************************************************************************************************************
ok: [198.211.105.187]

TASK [Update apt repo and cache] *********************************************************************************************************************************
ok: [198.211.105.187]

TASK [Install nodejs and npm] ************************************************************************************************************************************
ok: [198.211.105.187]

PLAY [Create new Linux user for node app] ************************************************************************************************************************

TASK [Gathering Facts] *******************************************************************************************************************************************
ok: [198.211.105.187]

TASK [Create Linux user] *****************************************************************************************************************************************
ok: [198.211.105.187]

PLAY [Deploy nodejs app] *****************************************************************************************************************************************

TASK [Gathering Facts] *******************************************************************************************************************************************
ok: [198.211.105.187]

TASK [Unpack the nodejs file] ************************************************************************************************************************************
ok: [198.211.105.187]

TASK [Install dependencies] **************************************************************************************************************************************
ok: [198.211.105.187]

TASK [Start the application] *************************************************************************************************************************************
changed: [198.211.105.187]

TASK [Ensure app is running] *************************************************************************************************************************************
changed: [198.211.105.187]

TASK [Print app_status output] ***********************************************************************************************************************************
ok: [198.211.105.187] => {
    "msg": [
        "michael    44611 56.3 12.3 611676 58100 ?        Rl   13:43   0:00 node server",
        "michael    44633  0.0  0.3   2800  1792 pts/1    S+   13:43   0:00 /bin/sh -c ps aux |grep node",
        "michael    44635  0.0  0.4   7076  2048 pts/1    S+   13:43   0:00 grep node"
    ]
}

PLAY RECAP *******************************************************************************************************************************************************
198.211.105.187            : ok=11   changed=2    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   

