# Chapter15
This module we will explore automating the nexus deployment.

# Name: Automate Nexus Deployment

# Description: 

Project Introduction

    Automate Nexus Deployment

    We did this manually before

    Created a Droplet

    SSH'd into server and executed

         Download Nexus binary and unpack

         Run Nexus application using Nexus User


Automate these steps

    Executed repeatedly!


Manual Installation
                            ------------>       Ansible Playbook
Manual Configuration
                                                Enable you to translate to Ansible Playbook

                                                Learn common modules

This process will serve as the model of how to take any application and convert to an Ansible Playbook

    Preparation


    Our manual executions as reference

    
    apt-get update
    apt install openjdk-8-jre-headless
    apt install net-tools

    cd /opt
    wget https://download.sonatype.com/nexus/3/latest-unix.tar.gz
    tar -zxvf latest-unix.tar.gz

    adduser nexus
    chown -R nexus:nexus nexus-3.65.0-02
    chown -R nexus:nexus sonatype-work

    vim nexus-3.65.0-02/bin/nexus.rc
    run_as_user="nexus"

    su - nexus
    /opt/nexus-3.65.0-02/bin/nexus start

    ps aux | grep nexus
    netstat -lnpt

    Write 1st Play

            - name: Update apt repo and cache
              ansible.builtin.apt:
                update_cache: yes
                force_apt_get: yes
                cache_valid_time: 3600
            - name: Install Java 8
              ansible.builtin.apt: name=openjdk-8-jre-headless
            - name: Install Java 8
              ansible.builtin.apt: name=net-tools


    Write 2nd Play

        get_url module

        Download files from HTTP, HTTPS, or FTP to the remote server

        Separate module for Windows targets: "win_get_url"

            - name: Download and unpack nexus installer
              hosts: 157.230.234.229
              tasks:
                - name: Download Nexus
                  ansible.builtin.get_url:
                    url: https://download.sonatype.com/nexus/3/latest-unix.tar.gz
                    dest: /opt/

        root@ubuntu-s-2vcpu-4gb-nyc1-01:~# ls /opt
        digitalocean  nexus-3.70.1-02-unix.tar.gz

        Need to untar nexus-3*


        - name: Download and unpack nexus installer
          hosts: 157.230.234.229
          tasks:
          - name: Download Nexus
            ansible.builtin.get_url:
              url: https://download.sonatype.com/nexus/3/latest-unix.tar.gz
              dest: /opt/
            register: download_result
            - name: untar nexus installer
              ansible.builtin.unarchive:
                src: "{{ download_result.dest }}"
                dest: /opt/
                remote_src: yes

        This play allows the tar file to be passed to the archive step without hardcoding the version, but rather including in variable

        root@ubuntu-s-2vcpu-4gb-nyc1-01:~# ls /opt
        digitalocean  nexus-3.70.1-02  nexus-3.70.1-02-unix.tar.gz  sonatype-work 

        Change the name of the nexus directory using the find module

        root@ubuntu-s-2vcpu-4gb-nyc1-01:~# ls /opt
        digitalocean  nexus  nexus-3.70.1-02-unix.tar.gz  sonatype-work
        root@ubuntu-s-2vcpu-4gb-nyc1-01:~#      
        

        "Find" module

          tasks:
            - name: Download Nexus
              ansible.builtin.get_url:
                url: https://download.sonatype.com/nexus/3/latest-unix.tar.gz
                dest: /opt/
            register: download_result
            - name: untar nexus installer
              ansible.builtin.unarchive:
                src: "{{ download_result.dest }}"
                dest: /opt/
                remote_src: yes
            - name: find nexus folder
            ansible.builtin.find:
                path: /opt
                pattern: "nexus-*"
                file_type: directory
            register: find_result
            - name: Rename nexus folder
            ansible.builtin.shell: mv {{ find_result.files[0].path }} /opt/nexus

    Conditionals in Ansible

        Execute task depending on some condition

        Condition based on the value of a fact, a variable, or the result of a previous task

        "stat" Module

        Retrieve file or file system status

        "when"

        Applies to a single task

            - name: find nexus folder
              ansible.builtin.find:
                path: /opt
                pattern: "nexus-*"
                file_type: directory
              register: find_result
            - name: Check if nexus folder stats
              ansible.builtin.stat:
                path: /opt/nexus
              register: stat_result
            - name: Rename nexus folder
              ansible.builtin.shell: mv {{ find_result.files[0].path }} /opt/nexus
              when: not stat_result.stat.exists

              




# Usage

Last login: Wed Aug  7 00:37:01 2024 from 45.25.106.162

root@ubuntu-s-2vcpu-4gb-nyc1-01:~# ls /opt
digitalocean  nexus-3.70.1-02-unix.tar.gz

root@ubuntu-s-2vcpu-4gb-nyc1-01:~# java -version
openjdk version "1.8.0_422"
OpenJDK Runtime Environment (build 1.8.0_422-8u422-b05-1~24.04-b05)
OpenJDK 64-Bit Server VM (build 25.422-b05, mixed mode)
root@ubuntu-s-2vcpu-4gb-nyc1-01:~#

Java

root@ubuntu-s-2vcpu-4gb-nyc1-01:~# java
Usage: java [-options] class [args...]
           (to execute a class)
   or  java [-options] -jar jarfile [args...]
           (to execute a jar file)
where options include:
    -d32	  use a 32-bit data model if available
    -d64	  use a 64-bit data model if available
    -server	  to select the "server" VM
    -zero	  to select the "zero" VM
    -dcevm	  to select the "dcevm" VM
                  The default VM is server,
                  because you are running on a server-class machine.


    -cp <class search path of directories and zip/jar files>
    -classpath <class search path of directories and zip/jar files>
                  A : separated list of directories, JAR archives,
                  and ZIP archives to search for class files.
    -D<name>=<value>
                  set a system property
    -verbose:[class|gc|jni]
                  enable verbose output
    -version      print product version and exit
    -version:<value>
                  Warning: this feature is deprecated and will be removed
                  in a future release.
                  require the specified version to run
    -showversion  print product version and continue
    -jre-restrict-search | -no-jre-restrict-search
                  Warning: this feature is deprecated and will be removed
                  in a future release.
                  include/exclude user private JREs in the version search
    -? -help      print this help message
    -X            print help on non-standard options
    -ea[:<packagename>...|:<classname>]
    -enableassertions[:<packagename>...|:<classname>]
                  enable assertions with specified granularity
    -da[:<packagename>...|:<classname>]
    -disableassertions[:<packagename>...|:<classname>]
                  disable assertions with specified granularity
    -esa | -enablesystemassertions
                  enable system assertions
    -dsa | -disablesystemassertions
                  disable system assertions
    -agentlib:<libname>[=<options>]
                  load native agent library <libname>, e.g. -agentlib:hprof
                  see also, -agentlib:jdwp=help and -agentlib:hprof=help
    -agentpath:<pathname>[=<options>]
                  load native agent library by full pathname
    -javaagent:<jarpath>[=<options>]
                  load Java programming language agent, see java.lang.instrument
    -splash:<imagepath>
                  show splash screen with specified image
See http://www.oracle.com/technetwork/java/javase/documentation/index.html for more details.
root@ubuntu-s-2vcpu-4gb-nyc1-01:~#

Netstat

root@ubuntu-s-2vcpu-4gb-nyc1-01:~# netstat
Active Internet connections (w/o servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State      
tcp6       0     52 ubuntu-s-2vcpu-4gb-:ssh 45-25-106-162.lig:61452 ESTABLISHED
udp        0    896 10.116.0.2:56106        67.207.67.3:domain      ESTABLISHED
udp        0      0 ubuntu-s-2vcpu-4g:44423 67.207.67.2:domain      ESTABLISHED
Active UNIX domain sockets (w/o servers)
Proto RefCnt Flags       Type       State         I-Node   Path
unix  3      [ ]         STREAM     CONNECTED     6111     /run/systemd/journal/stdout
unix  3      [ ]         STREAM     CONNECTED     8182     
unix  3      [ ]         STREAM     CONNECTED     7598     /run/systemd/journal/stdout
unix  3      [ ]         STREAM     CONNECTED     6658     /run/systemd/journal/stdout
unix  3      [ ]         STREAM     CONNECTED     4760     
unix  3      [ ]         STREAM     CONNECTED     3025     
unix  3      [ ]         STREAM     CONNECTED     6991     
unix  3      [ ]         STREAM     CONNECTED     8175     
unix  2      [ ]         DGRAM      CONNECTED     6925     
unix  3      [ ]         STREAM     CONNECTED     7682     /run/dbus/system_bus_socket
unix  3      [ ]         STREAM     CONNECTED     16013    
unix  3      [ ]         STREAM     CONNECTED     7029     /run/dbus/system_bus_socket
unix  2      [ ]         DGRAM      CONNECTED     6701     
unix  3      [ ]         DGRAM      CONNECTED     2564     
unix  3      [ ]         STREAM     CONNECTED     8023     /run/dbus/system_bus_socket
unix  2      [ ]         DGRAM      CONNECTED     16040    
unix  3      [ ]         STREAM     CONNECTED     6696     
unix  2      [ ]         DGRAM                    16064    /run/user/0/systemd/notify
unix  2      [ ]         DGRAM      CONNECTED     13758    
unix  3      [ ]         DGRAM      CONNECTED     6137     
unix  2      [ ]         DGRAM      CONNECTED     8158     
unix  3      [ ]         STREAM     CONNECTED     7292     
unix  3      [ ]         DGRAM      CONNECTED     6705     
unix  3      [ ]         DGRAM      CONNECTED     16065    
unix  3      [ ]         STREAM     CONNECTED     16015    /run/systemd/journal/stdout
unix  2      [ ]         DGRAM      CONNECTED     6125     
unix  2      [ ]         DGRAM      CONNECTED     6992     
unix  3      [ ]         STREAM     CONNECTED     6110     
unix  3      [ ]         STREAM     CONNECTED     7350     /run/systemd/journal/stdout
unix  2      [ ]         DGRAM      CONNECTED     14359    
unix  3      [ ]         DGRAM      CONNECTED     6138     
unix  3      [ ]         STREAM     CONNECTED     5149     /run/systemd/journal/stdout
unix  3      [ ]         DGRAM      CONNECTED     6136     
unix  2      [ ]         DGRAM      CONNECTED     16026    
unix  3      [ ]         STREAM     CONNECTED     7917     /run/systemd/journal/stdout
unix  3      [ ]         STREAM     CONNECTED     7671     /run/dbus/system_bus_socket
unix  3      [ ]         STREAM     CONNECTED     3029     /run/systemd/journal/stdout
unix  3      [ ]         STREAM     CONNECTED     6977     
unix  3      [ ]         STREAM     CONNECTED     8183     
unix  3      [ ]         DGRAM      CONNECTED     6706     
unix  3      [ ]         STREAM     CONNECTED     8136     /run/dbus/system_bus_socket
unix  3      [ ]         DGRAM      CONNECTED     6135     
unix  3      [ ]         DGRAM      CONNECTED     2563     
unix  3      [ ]         STREAM     CONNECTED     7683     /run/dbus/system_bus_socket
unix  3      [ ]         STREAM     CONNECTED     6923     
unix  2      [ ]         DGRAM      CONNECTED     7307     
unix  3      [ ]         DGRAM      CONNECTED     5260     
unix  3      [ ]         DGRAM      CONNECTED     5261     
unix  3      [ ]         DGRAM      CONNECTED     5262     
unix  3      [ ]         DGRAM      CONNECTED     5259     
unix  2      [ ]         DGRAM      CONNECTED     5253     
unix  2      [ ]         STREAM     CONNECTED     18995    
unix  3      [ ]         DGRAM      CONNECTED     2562     /run/systemd/notify
unix  2      [ ]         DGRAM      CONNECTED     19014    
unix  2      [ ]         DGRAM                    722      /run/systemd/journal/syslog
unix  9      [ ]         DGRAM      CONNECTED     2585     /run/systemd/journal/dev-log
unix  9      [ ]         DGRAM      CONNECTED     2587     /run/systemd/journal/socket
unix  3      [ ]         STREAM     CONNECTED     7859     
unix  3      [ ]         STREAM     CONNECTED     13644    /run/systemd/journal/stdout
unix  3      [ ]         DGRAM      CONNECTED     16066    
unix  3      [ ]         STREAM     CONNECTED     9974     /run/systemd/journal/stdout
unix  3      [ ]         STREAM     CONNECTED     7824     /run/systemd/journal/stdout
unix  3      [ ]         STREAM     CONNECTED     13642    
unix  3      [ ]         STREAM     CONNECTED     7528     
unix  3      [ ]         STREAM     CONNECTED     7530     /run/systemd/journal/stdout
unix  2      [ ]         DGRAM      CONNECTED     8122     
unix  3      [ ]         STREAM     CONNECTED     7597     /run/systemd/journal/stdout
unix  3      [ ]         STREAM     CONNECTED     8119     
unix  2      [ ]         DGRAM      CONNECTED     7563     
unix  3      [ ]         STREAM     CONNECTED     11546    
unix  3      [ ]         STREAM     CONNECTED     7823     
unix  3      [ ]         STREAM     CONNECTED     8135     
unix  3      [ ]         STREAM     CONNECTED     15229    /run/dbus/system_bus_socket
unix  2      [ ]         DGRAM      CONNECTED     7667     
unix  3      [ ]         STREAM     CONNECTED     8118     /run/systemd/journal/stdout
unix  3      [ ]         STREAM     CONNECTED     7595     
unix  3      [ ]         STREAM     CONNECTED     8121     /run/systemd/journal/stdout
unix  3      [ ]         STREAM     CONNECTED     7862     /run/dbus/system_bus_socket
unix  3      [ ]         STREAM     CONNECTED     7669     
unix  2      [ ]         DGRAM                    7841     
unix  3      [ ]         STREAM     CONNECTED     7803     /run/dbus/system_bus_socket
unix  3      [ ]         STREAM     CONNECTED     7684     /run/dbus/system_bus_socket
unix  3      [ ]         STREAM     CONNECTED     8115     
unix  3      [ ]         STREAM     CONNECTED     7668     
unix  3      [ ]         STREAM     CONNECTED     7510     @d23adbcd8ed8d3ff/bus/systemd-network/bus-api-network
unix  3      [ ]         STREAM     CONNECTED     7509     @747b5e6b702e4f8/bus/systemd-timesyn/bus-api-timesync
unix  3      [ ]         STREAM     CONNECTED     7583     @4eb5d4181dd000f6/bus/systemd-logind/system
unix  3      [ ]         STREAM     CONNECTED     7508     @3e32ab1b13dbbbb1/bus/systemd-resolve/bus-api-resolve
unix  3      [ ]         STREAM     CONNECTED     16068    @296bc9189fadfa9c/bus/systemd/bus-system
unix  3      [ ]         STREAM     CONNECTED     7801     @105bd3ef8bd208e5/bus/systemd/bus-api-system


PLAY [Install Jave and net-tools] ********************************************************************************************************************************

TASK [Gathering Facts] *******************************************************************************************************************************************
ok: [157.230.234.229]

TASK [Update apt repo and cache] *********************************************************************************************************************************
changed: [157.230.234.229]

TASK [Install Java 8] ********************************************************************************************************************************************
ok: [157.230.234.229]

TASK [Install Java 8] ********************************************************************************************************************************************
ok: [157.230.234.229]

PLAY [Download and unpack nexus installer] ***********************************************************************************************************************

TASK [Gathering Facts] *******************************************************************************************************************************************
ok: [157.230.234.229]

TASK [Download Nexus] ********************************************************************************************************************************************
ok: [157.230.234.229]

TASK [untar nexux installer] *************************************************************************************************************************************
changed: [157.230.234.229]

PLAY RECAP *******************************************************************************************************************************************************
157.230.234.229            : ok=7    changed=2    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   

Register tar file and pass variable without hardcoding the value

PLAY [Install Jave and net-tools] ********************************************************************************************************************************

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

PLAY RECAP *******************************************************************************************************************************************************
157.230.234.229            : ok=7    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   

    