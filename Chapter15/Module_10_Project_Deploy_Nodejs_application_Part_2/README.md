# Chapter10
This module we will continue with part 2 of automating the Node App Deployment

# Name: Automate Node App Deployment Part 2


# Description: 

Start Node App

    Install App Dependencies

    Run node command

    community.general.npm – Manage node.js packages with npm


    Important to remember where code is running at:

        Control Node is where ansible is running

        Managed Node is where the code is being run

            Command Execution

            Commands are executed on the hosts (remote server)

    Install Dependencies
            
            - name: Install dependencies
              community.general.npm:
                path: /root/package
            - name: Start the application
              ansible.builtin.command:
                chdir: /root/package/app
                cmd: node server


    PLAY [Install node and npm] **************************************************************************************************************************************

    TASK [Gathering Facts] *******************************************************************************************************************************************
    ok: [198.211.105.187]

    TASK [Update apt repo and cache] *********************************************************************************************************************************
    changed: [198.211.105.187]

    TASK [Install nodejs and npm] ************************************************************************************************************************************
    ok: [198.211.105.187]

    PLAY [Deploy nodejs app] *****************************************************************************************************************************************

    TASK [Gathering Facts] *******************************************************************************************************************************************
    ok: [198.211.105.187]

    TASK [Unpack the nodejs file] ************************************************************************************************************************************
    ok: [198.211.105.187]

    TASK [Install dependencies] **************************************************************************************************************************************
    changed: [198.211.105.187]

    TASK [Start the application] *************************************************************************************************************************************


    The final task is blocking the terminal, so it is unable to end. Hence we need to cancel and add async & poll attribute

        poll: 0  allows for the break and the terminal to run immediately
        async: 1000  allows the tasks to run asynchronously until it ends or is canceled


    Re-run playbook

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
    ok: [198.211.105.187]

    TASK [Install dependencies] **************************************************************************************************************************************
    ok: [198.211.105.187]

    TASK [Start the application] *************************************************************************************************************************************
    changed: [198.211.105.187]

    PLAY RECAP *******************************************************************************************************************************************************
    198.211.105.187            : ok=7    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   



    SSH to server

        root@ubuntu-s-1vcpu-512mb-10gb-nyc1-01:~/package# ps aux |grep node
        root       11356  0.0 12.2 611932 57628 pts/0    Sl+  02:14   0:00 node server
        root       11520  0.0  0.4   7076  2048 pts/1    S+   02:34   0:00 grep --color=auto node


Ensure App is running

        - name: Ensure app is running
          ansible.builtin.shell:
          cmd: ps aux |grep node

        Return Values of Modules

        Ansible modules normally return data

        This data can be registered into a varaible


        Debug module

         print statements during execution

         useful for debugging variables or expressions


            - name: Ensure app is running
              ansible.builtin.shell:
                cmd: ps aux |grep node
              register: app_status
            - name: Print app_status output
              ansible.builtin.debug:
                msg: "{{ app_status.stdout_lines }}"


         TASK [Print app_status output] ***********************************************************************************************************************************
            ok: [198.211.105.187] => {
                "msg": [
                    "root       13017 59.1 12.7 612432 59632 ?        Rl   03:08   0:00 node server",
                    "root       13032  0.0  0.3   2800  1792 pts/0    S+   03:08   0:00 /bin/sh -c ps aux |grep node",
                    "root       13034  0.0  0.4   7076  2048 pts/0    S+   03:08   0:00 grep node"
                ]
            }

            PLAY RECAP *******************************************************************************************************************************************************
            198.211.105.187            : ok=9    changed=2    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   


        Command and shell and command modules are not idempotent or stateful, so it does not know that nodejs is still running

            The solution is to use conditionals


        In Python we need to check the status

        Ansible and Terraform handles that state check for us

            checks the actual and desired state for you

        Ansible easier to write, because it's just YAML

        No programming skills needed






# Usage

root@ubuntu-s-1vcpu-512mb-10gb-nyc1-01:~# ls 
package

root@ubuntu-s-1vcpu-512mb-10gb-nyc1-01:~# cd package/
root@ubuntu-s-1vcpu-512mb-10gb-nyc1-01:~/package# ls
Dockerfile  Readme.md  app  node_modules  package-lock.json  package.json
root@ubuntu-s-1vcpu-512mb-10gb-nyc1-01:~/package#

root@ubuntu-s-1vcpu-512mb-10gb-nyc1-01:~# ps aux|grep node
root       11356  0.9 12.2 611932 57628 pts/0    Sl+  02:14   0:00 node server
root       11438  0.0  0.4   7076  2048 pts/1    S+   02:15   0:00 grep --color=auto node

