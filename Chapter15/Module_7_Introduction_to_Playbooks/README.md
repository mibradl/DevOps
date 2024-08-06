# Chapter15
This module we will explore an introduction to playbooks

# Name: Introduction to Playbooks

# Description: 

Ansible Playbooks

    Infrastructure as Code

        Ansible configuration files are treated like code

        Create Ansible Project

        Save in source control, like Git


    Create ansible directory

        mkdir -pv ansible


    Minimum Two Items Required

        The managed nodes to target

        At least one task to execute

    
    Create host file

        [droplet]
        137.184.196.79
        137.184.194.97
        137.184.114.89

        [droplet:vars]
        ansible_ssh_private_key_file=~/.ssh/id_rsa
        ansible_user=root


    Write a simple Playbook

        Playbook

            Ordered list of tasks

            Plays & tasks run in order from top to bottom

            A Playbook can have multiple Plays

            A Play is a group of tasks


            Can have one play for Database Servers

            Have another play for Webservers


            Tasks for Play

                Install nginx

                Start nginx


        Execute Playbook

            ansible-playbook -i hosts my-playbook.yaml



Gather Facts Module

        Is automatically called by playbook

        To gather useful variables about remote hosts that can be used in playbooks

        So Ansible provides many facts about the system, automatically



        root@ubuntu-s-2vcpu-4gb-nyc1-01:~# ps aux |grep nginx
        
        root@ubuntu-s-2vcpu-4gb-nyc1-01:~# nginx -v
        nginx version: nginx/1.24.0 (Ubuntu)
        root@ubuntu-s-2vcpu-4gb-nyc1-01:~# 


    Install specific version

      ---
      - name: Configure nginx web server
        hosts: webserver
        tasks:
        - name: install nginx server
            apt:
            name: nginx=1.24.0-2ubuntu7
            state: present
        - name: start nginx server
            service:
            name: nginx
            state: started

    Ansible Idempotency

        Most Ansible modules check whether the desired state has already been achieved

        They exist without performing any actions


        Idempotency

        Actual State                Desired State

        version 1.24 installed      version 1.24 installed

                        No Change to be made

    Uninstall Nginx



        ---
        - name: Configure nginx web server
        hosts: webserver
        tasks:
        - name: Uninstall nginx server
            apt:
            name: nginx=1.24.0-2ubuntu7
            state: absent
        - name: stopped nginx server
          service:
            name: nginx
            state: stopped








        





# Usage

 ansible-playbook -i hosts my-playbook.yaml          


PLAY [Configure nginx web server] ********************************************************************************************************************************

TASK [Gathering Facts] *******************************************************************************************************************************************
ok: [137.184.194.97]
ok: [137.184.196.79]

TASK [install nginx server] **************************************************************************************************************************************
ok: [137.184.194.97]
ok: [137.184.196.79]

TASK [start nginx server] ****************************************************************************************************************************************
ok: [137.184.194.97]
ok: [137.184.196.79]

PLAY RECAP *******************************************************************************************************************************************************
137.184.194.97             : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
137.184.196.79             : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   

michaelbradley@Michaels-iMac-2.attlocal.net /Users/michaelbradley/ansible 
    


root@ubuntu-s-2vcpu-4gb-nyc1-01:~# ps aux |grep nginx
root       15234  0.0  0.0  11156  1588 ?        Ss   14:09   0:00 nginx: master process /usr/sbin/nginx -g daemon on; master_process on;
www-data   15235  0.0  0.1  12880  4276 ?        S    14:09   0:00 nginx: worker process
www-data   15236  0.0  0.1  12880  4276 ?        S    14:09   0:00 nginx: worker process
root       15990  0.0  0.0   7076  2048 pts/0    S+   14:24   0:00 grep --color=auto nginx