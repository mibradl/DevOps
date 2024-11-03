# Chapter15
This module we will look at Ansible Inventory

# Name: Ansible Inventory and Ansible adhoc commands

# Description: 

To configure the two droplets with Ansible we need the Ansible Inventory File

    File containing data about the ansible client servers

    "hosts" meaning the managed servers

    Default location for file /etc/ansible/hosts

    vim hosts fifle

    137.184.194.97
    137.184.194.79

        which server to connect to

        credentials to authenticate with the server

            needs either username and password

            private SSH key

            michaelbradley@Michaels-iMac-2 ~ % cat hosts
            137.184.196.79 ansible_ssh_private_key_file=~/.ssh/id_rsa ansible_user=root
            137.184.194.97 ansible_ssh_private_key_file=~/.ssh/id_rsa ansible_user=root

        Ansible adhoc commands

        A fast way to interact with the desired servers


        ansible [pattern] -m [module] -a "[module options]"

        [pattern] = targeting hosts and groups

        "all" = default group, which contains every host

        [module] = discrete units of code

        ansible all -i hosts -m ping


        [droplet]
        137.184.196.79 ansible_ssh_private_key_file=~/.ssh/id_rsa ansible_user=root
        137.184.194.97 ansible_ssh_private_key_file=~/.ssh/id_rsa ansible_user=root

        [aws]
        ...

        [database]
        ...
        
        [web]
        ...


        Grouping Hosts

        You can put each host in more than one group

        You can create groups that track:

            where - a data center/region, e.g. east, west

            what - e.g. database servers, web servers etc

            when - which stage, e.g. dev, test, prod environment

        ansible droplet -i hosts -m ping


        Target

            Individual servers

            Group of servers

            All servers


        [droplet]
        137.184.196.79 ansible_ssh_private_key_file=~/.ssh/id_rsa ansible_user=root
        137.184.194.97 ansible_ssh_private_key_file=~/.ssh/id_rsa ansible_user=root

        Instead of configuring each ansible_ssh and ansible_user for each entry for 500 hundred servers

        [droplet:vars]
        ansible_ssh_private_key_file=~/.ssh/id_rsa
        ansible_user=root











# Usage


   michaelbradley@Michaels-iMac-2 ~ % ansible all -i hosts -m ping
/usr/local/Cellar/ansible/10.1.0/libexec/lib/python3.12/site-packages/paramiko/pkey.py:100: CryptographyDeprecationWarning: TripleDES has been moved to cryptography.hazmat.decrepit.ciphers.algorithms.TripleDES and will be removed from this module in 48.0.0.
  "cipher": algorithms.TripleDES,
/usr/local/Cellar/ansible/10.1.0/libexec/lib/python3.12/site-packages/paramiko/transport.py:259: CryptographyDeprecationWarning: TripleDES has been moved to cryptography.hazmat.decrepit.ciphers.algorithms.TripleDES and will be removed from this module in 48.0.0.
  "class": algorithms.TripleDES,
The authenticity of host '137.184.194.97 (137.184.194.97)' can't be established.
ED25519 key fingerprint is SHA256:V7m+iQyohuKxLb/FKl+0KhVLS1JF9cO8VTNhHytOi0M.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? [WARNING]: Platform linux on host 137.184.196.79 is using the discovered Python interpreter at /usr/bin/python3.12, but future installation of another Python interpreter could change
the meaning of that path. See https://docs.ansible.com/ansible-core/2.17/reference_appendices/interpreter_discovery.html for more information.
137.184.196.79 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3.12"
    },
    "changed": false,
    "ping": "pong"
}
yes
[WARNING]: Platform linux on host 137.184.194.97 is using the discovered Python interpreter at /usr/bin/python3.12, but future installation of another Python interpreter could change
the meaning of that path. See https://docs.ansible.com/ansible-core/2.17/reference_appendices/interpreter_discovery.html for more information.
137.184.194.97 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3.12"
    },
    "changed": false,
    "ping": "pong"
}


Using Group Name

michaelbradley@Michaels-iMac-2 ~ % ansible droplet -i hosts -m ping
/usr/local/Cellar/ansible/10.1.0/libexec/lib/python3.12/site-packages/paramiko/pkey.py:100: CryptographyDeprecationWarning: TripleDES has been moved to cryptography.hazmat.decrepit.ciphers.algorithms.TripleDES and will be removed from this module in 48.0.0.
  "cipher": algorithms.TripleDES,
/usr/local/Cellar/ansible/10.1.0/libexec/lib/python3.12/site-packages/paramiko/transport.py:259: CryptographyDeprecationWarning: TripleDES has been moved to cryptography.hazmat.decrepit.ciphers.algorithms.TripleDES and will be removed from this module in 48.0.0.
  "class": algorithms.TripleDES,
[WARNING]: Platform linux on host 137.184.194.97 is using the discovered Python interpreter at /usr/bin/python3.12, but future installation of another Python interpreter could change
the meaning of that path. See https://docs.ansible.com/ansible-core/2.17/reference_appendices/interpreter_discovery.html for more information.
137.184.194.97 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3.12"
    },
    "changed": false,
    "ping": "pong"
}
[WARNING]: Platform linux on host 137.184.196.79 is using the discovered Python interpreter at /usr/bin/python3.12, but future installation of another Python interpreter could change
the meaning of that path. See https://docs.ansible.com/ansible-core/2.17/reference_appendices/interpreter_discovery.html for more information.
137.184.196.79 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3.12"
    },
    "changed": false,
    "ping": "pong"
}

 