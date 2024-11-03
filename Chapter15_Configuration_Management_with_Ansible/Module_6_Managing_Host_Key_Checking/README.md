# Chapter15
This module we will explore managing host key checking

# Name: Managing Host Key Checking

# Description: 

Host Key Checking

    Is enabled by default in Ansible

    It guards against server spoofing and man-in-the-middle attacks

        But we don't want to have this interactive mode

    Authorized Keys & Known Hosts

    Two options based on 

        Long-lived servers

        ephemeral or temporary servers

        Add IP address to known host

        michaelbradley@Michaels-iMac-2 ~ % ssh-keyscan -H 144.126.220.56 >> ~/.ssh/known_hosts
        # 144.126.220.56:22 SSH-2.0-OpenSSH_9.6p1 Ubuntu-3ubuntu13.4
        # 144.126.220.56:22 SSH-2.0-OpenSSH_9.6p1 Ubuntu-3ubuntu13.4
        # 144.126.220.56:22 SSH-2.0-OpenSSH_9.6p1 Ubuntu-3ubuntu13.4
        # 144.126.220.56:22 SSH-2.0-OpenSSH_9.6p1 Ubuntu-3ubuntu13.4
        # 144.126.220.56:22 SSH-2.0-OpenSSH_9.6p1 Ubuntu-3ubuntu13.4


        michaelbradley@Michaels-iMac-2 ~ % ssh root@144.126.220.56
        Welcome to Ubuntu 24.04 LTS (GNU/Linux 6.8.0-36-generic x86_64)

        ...
        Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
        applicable law.

        root@ubuntu-s-1vcpu-2gb-sfo3-01:~# 

        The ~/.ssh/authorized_keys was automatically added because we added the ssh pub key when we created the droplet



        Created Droplet with password, 

        michaelbradley@Michaels-iMac-2 ~ % ssh-keyscan -H 167.172.56.229 >> ~/.ssh/known_hosts
        # 167.172.56.229:22 SSH-2.0-OpenSSH_9.6p1 Ubuntu-3ubuntu13.4
        # 167.172.56.229:22 SSH-2.0-OpenSSH_9.6p1 Ubuntu-3ubuntu13.4
        # 167.172.56.229:22 SSH-2.0-OpenSSH_9.6p1 Ubuntu-3ubuntu13.4
        # 167.172.56.229:22 SSH-2.0-OpenSSH_9.6p1 Ubuntu-3ubuntu13.4
        # 167.172.56.229:22 SSH-2.0-OpenSSH_9.6p1 Ubuntu-3ubuntu13.4


        michaelbradley@Michaels-iMac-2 ~ % ssh root@167.172.56.229
        root@167.172.56.229's password: 


        michaelbradley@Michaels-iMac-2 ~ % ssh root@167.172.56.229
        root@167.172.56.229's password: 
        Welcome to Ubuntu 24.04 LTS (GNU/Linux 6.8.0-36-generic x86_64)

        * Documentation:  https://help.ubuntu.com
        * Management:     https://landscape.canonical.com
        * Support:        https://ubuntu.com/pro

        System information as of Sat Aug  3 12:08:19 UTC 2024

        System load:  0.44              Processes:             120
        Usage of /:   2.2% of 76.45GB   Users logged in:       0
        Memory usage: 6%                IPv4 address for eth0: 167.172.56.229
        Swap usage:   0%                IPv4 address for eth0: 10.16.0.5


        To add automation to ssh after previously creating ssh with password

            Append key to known_hosts file on local machine

            ssh_copy-id root@167.172.56.229 to direct code to remote machine

            michaelbradley@Michaels-iMac-2 ~ % ssh_copy-id root@167.172.56.229



            michaelbradley@Michaels-iMac-2 ~ % ssh-copy-id root@167.172.56.229
            /usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/Users/michaelbradley/.ssh/id_rsa.pub"
            /usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
            /usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
            root@167.172.56.229's password: 

            Number of key(s) added:        1

            Now try logging into the machine, with:   "ssh 'root@167.172.56.229'"
            and check to make sure that only the key(s) you wanted were added.


            Perform this on servers that will be around for a long period of time


            Now we can perform ansible ping on droplet group

            Ping all servers in droplet group

            ansible droplet -i hosts -m ping 



Disable Host Key Checking

        Ephemeral Infrastructure

            Servers are dynamically created and destroyed


        Config File Default Locations

            /etc/ansible/ansible.cfg

            ~/.ansible.cfg

            [defaults]
            host_key_checking = False


The configuration file

        Changes can be made and used in a configuration file which will be searched for in the following order:

            ANSIBLE_CONFIG (environment variable if set)

            ansible.cfg (in current directory)

            ~/.ansible.cfg (in the home directory)

            /etc/ansible/ansible.cfg

        Ansible will process the above list and use the first file found, all others are ignored














# Usage

michaelbradley@Michaels-iMac-2 ~ % ssh-keyscan -H 144.126.220.56 >> ~/.ssh/known_hosts
# 144.126.220.56:22 SSH-2.0-OpenSSH_9.6p1 Ubuntu-3ubuntu13.4
# 144.126.220.56:22 SSH-2.0-OpenSSH_9.6p1 Ubuntu-3ubuntu13.4
# 144.126.220.56:22 SSH-2.0-OpenSSH_9.6p1 Ubuntu-3ubuntu13.4
# 144.126.220.56:22 SSH-2.0-OpenSSH_9.6p1 Ubuntu-3ubuntu13.4
# 144.126.220.56:22 SSH-2.0-OpenSSH_9.6p1 Ubuntu-3ubuntu13.4
michaelbradley@Michaels-iMac-2 ~ % ssh root@144.126.220.56
Welcome to Ubuntu 24.04 LTS (GNU/Linux 6.8.0-36-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/pro

 System information as of Sat Aug  3 11:22:34 UTC 2024

  System load:  0.08              Processes:             105
  Usage of /:   3.5% of 47.39GB   Users logged in:       0
  Memory usage: 9%                IPv4 address for eth0: 144.126.220.56
  Swap usage:   0%                IPv4 address for eth0: 10.48.0.5

Expanded Security Maintenance for Applications is not enabled.

43 updates can be applied immediately.
23 of these updates are standard security updates.
To see these additional updates run: apt list --upgradable

Enable ESM Apps to receive additional future security updates.
See https://ubuntu.com/esm or run: sudo pro status



The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

root@ubuntu-s-1vcpu-2gb-sfo3-01:~# 


Ping all servers in droplet group

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
[WARNING]: Platform linux on host 144.126.220.56 is using the discovered Python interpreter at /usr/bin/python3.12, but future installation of another Python interpreter could change
the meaning of that path. See https://docs.ansible.com/ansible-core/2.17/reference_appendices/interpreter_discovery.html for more information.
144.126.220.56 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3.12"
    },
    "changed": false,
    "ping": "pong"
}
[WARNING]: Platform linux on host 167.172.56.229 is using the discovered Python interpreter at /usr/bin/python3.12, but future installation of another Python interpreter could change
the meaning of that path. See https://docs.ansible.com/ansible-core/2.17/reference_appendices/interpreter_discovery.html for more information.
167.172.56.229 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3.12"
    },
    "changed": false,
    "ping": "pong"
}


    