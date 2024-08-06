# Chapter15
This module we will add EC2 instances to the inventory

# Name: Add EC2 instances to Inventory 

# Description: 

Launch EC2 instance

    AWS Linux

    T2 Micro

    create pem ssh key

    2 instances

    launch


    Add ec2 group to hosts file and ssh private key, plus set appropriate permission

    [ec2]
    c2-54-197-66-26.compute-1.amazonaws.com
    ec2-54-172-207-74.compute-1.amazonaws.com

    [ec2:vars]
    ansible_ssh_private_key_file=~/Downloads/ansible.pem
    ansible_user=ec2-user

    chmod 400 ~/Downloads/ansible.pem


    ssh -i ~/Downloads/ansible.pem ec2-user@ec2-54-197-66-26.compute-1.amazonaws.com

    ansible needs python to run

    [ec2-user@ip-172-31-94-78 ~]$ python3
    Python 3.9.16 (main, Apr 24 2024, 00:00:00) 
    [GCC 11.4.1 20230605 (Red Hat 11.4.1-2)] on linux
    Type "help", "copyright", "credits" or "license" for more information.
    >>> 


    ansible ec2 -i hosts -m ping

    Warning message ansible discovered another python interpreter, which could cause confusion

    [WARNING]: Platform linux on host ec2-54-172-207-74.compute-1.amazonaws.com is using the discovered Python interpreter at /usr/bin/python3.9, but future installation of another Python
    interpreter could change the meaning of that path. See https://docs.ansible.com/ansible-core/2.17/reference_appendices/interpreter_discovery.html for more information.
    ec2-54-172-207-74.compute-1.amazonaws.com | SUCCESS => {
        "ansible_facts": {
            "discovered_interpreter_python": "/usr/bin/python3.9"
        },
        

        [ec2:vars]
        ansible_ssh_private_key_file=~/Downloads/ansible.pem
        ansible_user=ec2-user
        ansible_python_interpreter=/usr/bin/python3
    



# Usage

michaelbradley@Michaels-iMac-2 ~ % ssh -i ~/Downloads/ansible.pem ec2-user@ec2-54-197-66-26.compute-1.amazonaws.com
The authenticity of host 'ec2-54-197-66-26.compute-1.amazonaws.com (54.197.66.26)' can't be established.
ED25519 key fingerprint is SHA256:gsn6GCXobTX6pIf5QGcO5zTVyxM0Y/fn2B7M0kBUIpA.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'ec2-54-197-66-26.compute-1.amazonaws.com' (ED25519) to the list of known hosts.
   ,     #_
   ~\_  ####_        Amazon Linux 2023
  ~~  \_#####\
  ~~     \###|
  ~~       \#/ ___   https://aws.amazon.com/linux/amazon-linux-2023
   ~~       V~' '->
    ~~~         /
      ~~._.   _/
         _/ _/
       _/m/'
[ec2-user@ip-172-31-94-78 ~]$


michaelbradley@Michaels-iMac-2 ~ % ansible ec2 -i hosts -m ping 
/usr/local/Cellar/ansible/10.1.0/libexec/lib/python3.12/site-packages/paramiko/pkey.py:100: CryptographyDeprecationWarning: TripleDES has been moved to cryptography.hazmat.decrepit.ciphers.algorithms.TripleDES and will be removed from this module in 48.0.0.
  "cipher": algorithms.TripleDES,
/usr/local/Cellar/ansible/10.1.0/libexec/lib/python3.12/site-packages/paramiko/transport.py:259: CryptographyDeprecationWarning: TripleDES has been moved to cryptography.hazmat.decrepit.ciphers.algorithms.TripleDES and will be removed from this module in 48.0.0.
  "class": algorithms.TripleDES,
The authenticity of host 'ec2-54-172-207-74.compute-1.amazonaws.com (54.172.207.74)' can't be established.
ED25519 key fingerprint is SHA256:Bj503tQ3Sw8l5qJn6tp2VAQ6o9u/6Y5t22VM2fGF648.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? [WARNING]: Platform linux on host ec2-54-197-66-26.compute-1.amazonaws.com is using the discovered Python interpreter at /usr/bin/python3.9, but future installation of another Python
interpreter could change the meaning of that path. See https://docs.ansible.com/ansible-core/2.17/reference_appendices/interpreter_discovery.html for more information.
ec2-54-197-66-26.compute-1.amazonaws.com | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3.9"
    },
    "changed": false,
    "ping": "pong"
}
yes
[WARNING]: Platform linux on host ec2-54-172-207-74.compute-1.amazonaws.com is using the discovered Python interpreter at /usr/bin/python3.9, but future installation of another Python
interpreter could change the meaning of that path. See https://docs.ansible.com/ansible-core/2.17/reference_appendices/interpreter_discovery.html for more information.
ec2-54-172-207-74.compute-1.amazonaws.com | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3.9"
    },
    "changed": false,
    "ping": "pong"
}


michaelbradley@Michaels-iMac-2 ~ % ansible ec2 -i hosts -m ping
/usr/local/Cellar/ansible/10.1.0/libexec/lib/python3.12/site-packages/paramiko/pkey.py:100: CryptographyDeprecationWarning: TripleDES has been moved to cryptography.hazmat.decrepit.ciphers.algorithms.TripleDES and will be removed from this module in 48.0.0.
  "cipher": algorithms.TripleDES,
/usr/local/Cellar/ansible/10.1.0/libexec/lib/python3.12/site-packages/paramiko/transport.py:259: CryptographyDeprecationWarning: TripleDES has been moved to cryptography.hazmat.decrepit.ciphers.algorithms.TripleDES and will be removed from this module in 48.0.0.
  "class": algorithms.TripleDES,
ec2-54-197-66-26.compute-1.amazonaws.com | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
[WARNING]: Platform linux on host ec2-54-172-207-74.compute-1.amazonaws.com is using the discovered Python interpreter at /usr/bin/python3.9, but future installation of another Python
interpreter could change the meaning of that path. See https://docs.ansible.com/ansible-core/2.17/reference_appendices/interpreter_discovery.html for more information.
ec2-54-172-207-74.compute-1.amazonaws.com | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3.9"
    },
    "changed": false,
    "ping": "pong"
}




