# Chapter6
This module we explored how to install and run Nexus

# Name: Install and Run Nexus

# Description: 

Nexus requires java 1.8 installed

root@ubuntu-s-4vcpu-8gb-nyc1-01:~# java
Command 'java' not found, but can be installed with:

root@ubuntu-s-4vcpu-8gb-nyc1-01:~# apt update

root@ubuntu-s-4vcpu-8gb-nyc1-01:~# apt install openjdk-8-jre-headless
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
The following additional packages will be installed:
  ca-certificates-java fontconfig-config fonts-dejavu-core fonts-dejavu-mono java-common libavahi-client3 libavahi-common-data libavahi-common3 libcups2t64
  libfontconfig1 libjpeg-turbo8 libjpeg8 liblcms2-2 libpcsclite1 libxi6 libxrender1 libxtst6 x11-common
Suggested packages:
  default-jre cups-common liblcms2-utils pcscd libnss-mdns fonts-dejavu-extra fonts-nanum fonts-ipafont-gothic fonts-ipafont-mincho fonts-wqy-microhei fonts-wqy-zenhei
  fonts-indic
The following NEW packages will be installed:
  ca-certificates-java fontconfig-config fonts-dejavu-core fonts-dejavu-mono java-common libavahi-client3 libavahi-common-data libavahi-common3 libcups2t64
  libfontconfig1 libjpeg-turbo8 libjpeg8 liblcms2-2 libpcsclite1 libxi6 libxrender1 libxtst6 openjdk-8-jre-headless x11-common
0 upgraded, 19 newly installed, 0 to remove and 28 not upgraded.
Need to get 33.0 MB of archives.
After this operation, 111 MB of additional disk space will be used.
Do you want to continue? [Y/n] Y

wget https://download.sonatype.com/nexus/3/nexus-3.69.0-02-java8-unix.tar.gz

Untar file

tar -zxvf nexus-3.69.0-02-java8-unix.tar.gz

root@ubuntu-s-4vcpu-8gb-nyc1-01:/opt# tar -zxvf nexus-3.69.0-02-java8-unix.tar.gz
nexus-3.69.0-02/.install4j/9d17dc87.lprop
nexus-3.69.0-02/.install4j/MessagesDefault
nexus-3.69.0-02/.install4j/build.uuid
nexus-3.69.0-02/.install4j/e4ada6b7.lprop
nexus-3.69.0-02/.install4j/i4j_extf_0_17is1ik.utf8
nexus-3.69.0-02/.install4j/i4j_extf_10_17is1ik_10358jn.png
nexus-3.69.0-02/.install4j/i4j_extf_11_17is1ik_1gne9sv.png
nexus-3.69.0-02/.install4j/i4j_extf_12_17is1ik_sc8j43.png
nexus-3.69.0-02/.install4j/i4j_extf_13_17is1ik_10nxrsm.png
nexus-3.69.0-02/.install4j/i4j_extf_14_17is1ik_yd7am4.png

root@ubuntu-s-4vcpu-8gb-nyc1-01:/opt# ls 
digitalocean  nexus-3.69.0-02  nexus-3.69.0-02-java8-unix.tar.gz  sonatype-work

Nexus folder: contains runtime and application of Nexus

root@ubuntu-s-4vcpu-8gb-nyc1-01:/opt# ls nexus-3.69.0-02
NOTICE.txt  OSS-LICENSE.txt  PRO-LICENSE.txt  bin  deploy  etc  lib  public  replicator  system

Sonatype-work: contains own config for Nexus and data

root@ubuntu-s-4vcpu-8gb-nyc1-01:/opt# ls sonatype-work/nexus3/
clean_cache  log  tmp

Subdirectories depending on your Nexus configuration

    e.g. plugins directories

    IP addresses that accessed Nexus

    Logs of Nexus App

    Your uploaded files and metadata

    You can use this folder for backup

Starting Nexus

    Services should not run with Root User Permissions

    Best practice: Create own User for Service (e.g. Nexus)

    Only the permissions for that specific Service

    adduser nexus
    usermod -aG sudo nexus



    root@ubuntu-s-4vcpu-8gb-nyc1-01:/opt# chown -R nexus:nexus nexus-3.69.0-02
    root@ubuntu-s-4vcpu-8gb-nyc1-01:/opt# chown -R nexus:nexus sonatype-work/
    root@ubuntu-s-4vcpu-8gb-nyc1-01:/opt# ls
    digitalocean  nexus-3.69.0-02  nexus-3.69.0-02-java8-unix.tar.gz  sonatype-work
    root@ubuntu-s-4vcpu-8gb-nyc1-01:/opt# ls -l
    total 224292
    drwxr-xr-x  4 root  root       4096 Jun 18 02:20 digitalocean
    drwxr-xr-x 10 nexus nexus      4096 Jun 18 03:23 nexus-3.69.0-02
    -rw-r--r--  1 root  root  229661398 Jun  4 14:23 nexus-3.69.0-02-java8-unix.tar.gz
    drwxr-xr-x  3 nexus nexus      4096 Jun 18 03:10 sonatype-work


    Nexus User

    nexus executable        sonatype-work folder    read/write

    vim nexus-3.69.0-02/bin/nexus.rc

    run_nexus_as="nexus"        set user to run as nexus user

    su - nexus

    nexus@ubuntu-s-4vcpu-8gb-nyc1-01:~$ ps aux |grep nexus
    nexus       4046 53.3 24.6 6637988 2005412 pts/0 Sl   03:49   2:56 /usr/lib/jvm/java-8-openjdk-amd64/jre/bin/java -server -Dinstall4j.jvmDir=/usr/lib/jvm/java-8-openjdk-amd64/jre -Dexe4j.moduleName=/opt/nexus-3.69.0-02/bin/nexus -XX:+UnlockDiagnosticVMOptions -Dinstall4j.launcherId=245 -Dinstall4j.swt=false -Di4jv=0 -Di4jv=0 -Di4jv=0 -Di4jv=0 -Di4jv=0 -Xms2703m -Xmx2703m -XX:MaxDirectMemorySize=2703m -XX:+UnlockDiagnosticVMOptions -XX:+LogVMOutput -XX:LogFile=../sonatype-work/nexus3/log/jvm.log -XX:-OmitStackTraceInFastThrow -Djava.net.preferIPv4Stack=true -Dkaraf.home=. -Dkaraf.base=. -Dkaraf.etc=etc/karaf -Djava.util.logging.config.file=etc/karaf/java.util.logging.properties -Dkaraf.data=../sonatype-work/nexus3 -Dkaraf.log=../sonatype-work/nexus3/log -Djava.io.tmpdir=../sonatype-work/nexus3/tmp -Dkaraf.startLocalConsole=false -Djdk.tls.ephemeralDHKeySize=2048 -Djava.endorsed.dirs=lib/endorsed -Di4j.vpt=true -classpath /opt/nexus-3.69.0-02/.install4j/i4jruntime.jar:/opt/nexus-3.69.0-02/lib/boot/nexus-main.jar:/opt/nexus-3.69.0-02/lib/boot/activation-1.1.jar:/opt/nexus-3.69.0-02/lib/boot/jakarta.xml.bind-api-2.3.3.jar:/opt/nexus-3.69.0-02/lib/boot/jaxb-runtime-2.3.3.jar:/opt/nexus-3.69.0-02/lib/boot/txw2-2.3.3.jar:/opt/nexus-3.69.0-02/lib/boot/istack-commons-runtime-3.0.10.jar:/opt/nexus-3.69.0-02/lib/boot/org.apache.karaf.main-4.3.9.jar:/opt/nexus-3.69.0-02/lib/boot/osgi.core-7.0.0.jar:/opt/nexus-3.69.0-02/lib/boot/org.apache.karaf.specs.activator-4.3.9.jar:/opt/nexus-3.69.0-02/lib/boot/org.apache.karaf.diagnostic.boot-4.3.9.jar:/opt/nexus-3.69.0-02/lib/boot/org.apache.karaf.jaas.boot-4.3.9.jar com.install4j.runtime.launcher.UnixLauncher start 9d17dc87 0 0 org.sonatype.nexus.karaf.NexusMain
    root        4807  0.0  0.0   9940  4480 pts/0    S    03:54   0:00 su - nexus
    nexus       4808  0.1  0.0   9060  5376 pts/0    S    03:54   0:00 -bash
    nexus       4817  0.0  0.0  11320  4352 pts/0    R+   03:54   0:00 ps aux
    nexus       4818  0.0  0.0   7076  2048 pts/0    S+   03:54   0:00 grep --color=auto nexus

    add firewall rules to open custom tcp port 8081

    157.230.238.59:8081



# Usage

root@ubuntu-s-4vcpu-8gb-nyc1-01:~# java
Command 'java' not found, but can be installed with:
apt install default-jre              # version 2:1.17-75, or
apt install openjdk-17-jre-headless  # version 17.0.10~6ea-1
apt install openjdk-11-jre-headless  # version 11.0.21+9-0ubuntu1
apt install openjdk-19-jre-headless  # version 19.0.2+7-4
apt install openjdk-20-jre-headless  # version 20.0.2+9-1
apt install openjdk-21-jre-headless  # version 21.0.1+12-3
apt install openjdk-22-jre-headless  # version 22~22ea-1
apt install openjdk-8-jre-headless   # version 8u392-ga-1
    

root@ubuntu-s-4vcpu-8gb-nyc1-01:~# apt install openjdk-8-jre-headless
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
The following additional packages will be installed:
  ca-certificates-java fontconfig-config fonts-dejavu-core fonts-dejavu-mono java-common libavahi-client3 libavahi-common-data libavahi-common3 libcups2t64
  libfontconfig1 libjpeg-turbo8 libjpeg8 liblcms2-2 libpcsclite1 libxi6 libxrender1 libxtst6 x11-common
Suggested packages:
  default-jre cups-common liblcms2-utils pcscd libnss-mdns fonts-dejavu-extra fonts-nanum fonts-ipafont-gothic fonts-ipafont-mincho fonts-wqy-microhei fonts-wqy-zenhei
  fonts-indic
The following NEW packages will be installed:
  ca-certificates-java fontconfig-config fonts-dejavu-core fonts-dejavu-mono java-common libavahi-client3 libavahi-common-data libavahi-common3 libcups2t64
  libfontconfig1 libjpeg-turbo8 libjpeg8 liblcms2-2 libpcsclite1 libxi6 libxrender1 libxtst6 openjdk-8-jre-headless x11-common
0 upgraded, 19 newly installed, 0 to remove and 28 not upgraded.
Need to get 33.0 MB of archives.
After this operation, 111 MB of additional disk space will be used.
Do you want to continue? [Y/n] Y

root@ubuntu-s-4vcpu-8gb-nyc1-01:~# cd /opt
root@ubuntu-s-4vcpu-8gb-nyc1-01:/opt# wget https://download.sonatype.com/nexus/3/nexus-3.69.0-02-java8-unix.tar.gz
--2024-06-18 02:55:02--  https://download.sonatype.com/nexus/3/nexus-3.69.0-02-java8-unix.tar.gz

root@ubuntu-s-4vcpu-8gb-nyc1-01:/opt# tar -zxvf nexus-3.69.0-02-java8-unix.tar.gz
nexus-3.69.0-02/.install4j/9d17dc87.lprop
nexus-3.69.0-02/.install4j/MessagesDefault
nexus-3.69.0-02/.install4j/build.uuid
nexus-3.69.0-02/.install4j/e4ada6b7.lprop
nexus-3.69.0-02/.install4j/i4j_extf_0_17is1ik.utf8
nexus-3.69.0-02/.install4j/i4j_extf_10_17is1ik_10358jn.png
nexus-3.69.0-02/.install4j/i4j_extf_11_17is1ik_1gne9sv.png
nexus-3.69.0-02/.install4j/i4j_extf_12_17is1ik_sc8j43.png
nexus-3.69.0-02/.install4j/i4j_extf_13_17is1ik_10nxrsm.png
nexus-3.69.0-02/.install4j/i4j_extf_14_17is1ik_yd7am4.png

adduser nexus
usermod -aG sudo nexus

su - nexus

nexus@ubuntu-s-4vcpu-8gb-nyc1-01:~$ /opt/nexus-3.69.0-02/bin/nexus start
Starting nexus
