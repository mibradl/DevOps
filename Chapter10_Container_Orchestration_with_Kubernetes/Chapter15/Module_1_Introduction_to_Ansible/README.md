# Chapter15
This module we will discuss the concepts of Ansible.

# Name: Introduction to Ansible

# Description: 

Introduction to Ansible

    Real life use cases

    Ansible Components

        Modules and Playbooks

    Ansible with Docker

    Ansible vs. alternative tools


What is Ansible?
    
    Tool to automate IT tasks

        What IT tasks?

        Why is it good to automate them?

Why use Ansible?

    Update Docker version on 10 or more servers

    Repetitive Tasks that take hours to complete

        Updates

        Backups

        System Reboots

        Create Users

        Assign Groups

        Assign Permissions

        SSH into servers and setup


        What did I do the last time?



    Ansible

        More efficient

        Less Time Consuming


    Four different ways

        Execute tasks from your own machine, instead of SSHing into all remote servers

        Configuration/Installation/Deployment steps in a single YAML file

            Instead of manual and shell scripts

        Re-use same file multiple times and for different environments

        More reliable and less likely for errors

    Supporting all Infrastructure

        From operating system

        Cloud Providers

        Bare Metal

    Ansible is agentless

        From Control Machine - SSH to machines

    How does Ansible work?

    Ansible Architecture
    
        Modules

            Small programs that do the actual work

            Modules get pushed to the target server

            Do their work and get removed

            Modules are granular and to a specific task

                Create or copy a file

                Install Nginx server

                Start Nginx server

                Start Docker Container

                Create Cloud Instance

            Modules to handle all IT tasks

            List of modules in Ansible Official document

            Ansible uses YAML

            No need to learn a specific language for Ansible

                Example Module Usage

                    Jenkins Module      jenkins_job

                  tasks:

                    # Create a jenkins job using basic authentication
                    - jenkins_job:
                        config: "{{ lookup('file', 'templates/test.xml') }}"
                        name: test
                        password: admin
                        url: http://localhost:8080
                        user: admin


                    # Delete a jenkins job using the token
                    - jenkins_job:
                        name: test
                        token: asdfasfasfasdfasd
                        state: absent
                        url: http://localhost:8080
                        user: admin

                Docker Module


                  tasks:

                    - name: Create a data container
                      docker_container:
                        name: mydata
                        image: busybox
                        volumes:
                          - /data

                    - name: Start a container with a command
                      docker_container:
                        name: sleepy
                        image: ubuntu:14.04
                        command: ["sleep", "infinity"]

                    - name: Add container to networks
                      docker_container:
                        name: sleepy
                        networks:
                          - name: TestingNet
                            ipv4_address: 172.1.1.18
                            links:
                              - sleeper
                          - name: TestingNet2
                            ipv4_address: 172.1.1.20

            Modules are granular and specific

                How to handle complex applications?

                    Multiple modules

                    In a certain sequence
                    
                    Grouped together

                Ansible Playbooks

                    1 Task = action to be performed

                        Module

                        Arguments

                        Description of task

                    Execute multiple modules in a sequence

                        create directory for nginx

                        install nginx latest version

                        start nginx

                    1 configuration


                    Where should these tasks be executed?

                        Hosts attribute

                        - hosts: databases


                    With which user should the tasks execute?

                          remote_user: root

                          tasks:
                            ...


                    ** strict indentation must be followed


                    Use variables for repeating values

                        - hosts: databases                                              Play
                          remote_user: root
                          vars:                                                         Which hosts
                            tablename: foo                                              Which tasks
                            tableowner: someuser                                        Which user

                        tasks:
                          - name: Rename table {{ tablename }} to bar                  
                            postgresql_table:
                              table: {{ tablename }}
                              rename: bar

                          - name: Set owner to someuser
                            postgresql_table:
                              name: {{ tablename }}
                              owner: someuser
                            

            Multiple Plays in a single YAML file

                Playbook = 1 or more Plays

                Play for Webserver

            Playbook describes:

                How and in which order

                At what time and where (on which machines)

                What (modules) should be executed

                *orchestrates the module execution


            Good practice: Naming Plays

                - name: install and start nginx server              #Name the play
                  hosts: webservers                                 #The host is derived from the inventory list
                  remote_user: root



                Host File

                10.24.0.100

                [webservers]
                10.24.0.1
                10.24.0.2

                [databases]
                10.24.0.7                                   #IP address or hostname
                10.24.0.8

                [webservers]                                #Groups multiple IP addresses or host names
                web1.myserver.com
                web2.myserver.com                           #Cloud servers, virtual or bare metal servers

                hosts: databases
                remote_user: root

                hosts: webservers
                remote_user: root


                Play for Databases

            Inventory = all the machines involved in task executions



            Ansible for Docker

                    Preparing Application Environment


                    Dockerfile                                  Docker Container

                    app.jar                                     Will produce a docker container
                    config file
                    log dir
                    env variables
                    start script




                    Ansible Playbook

                    Dockerfile                                  Docker Container

                    app.jar                                     Will produce a docker container
                    config file
                    log dir                                     Vagrant Container
                    env variables
                    start script                                Cloud Instance

                                                                Bare Metal

                    Ansible allows you to reproduce the app across many env

                    Not only can you manage the Docker Container, you can manage the Host, storage, the network, etc

                    Use a Play to manage both the Container and Host


            Ansible Tower

                    UI dashboard from Redhat

                    Centrally store automation tasks

                    Across teams

                    Configure permissions

                    Manage inventory



            Comparable Tools


                    Ansible                 Puppet and Chef

                    Simple YAML             Ruby - more difficult to learn

                    Agentless               Installation needed

                                            Requires managing updates on target servers


                                            




            
# Usage


    