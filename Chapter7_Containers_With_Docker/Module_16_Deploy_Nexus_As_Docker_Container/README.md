# Chapter7
This module we explore how to run Nexus as Docker Container.

# Name: Run Nexus as Docker Container

# Description: 

Previous Nexus Module

    Looked at steps to run Nexus on a Digital Ocean Droplet

    Install Java

    Download Nexus Package

    Untar Nexus Package

    Create Nexus User

    Give User permissions

    Run Nexus with Nexus User

    Able to Run Nexus Repository Manager


A Faster way to setup Nexus and to run Nexus as a Docker Container

Create a Droplet server for Nexus Container

    ssh root@138.197.33.206



    root@ubuntu-s-4vcpu-8gb-nyc3-01:~# docker --version
    Docker version 24.0.5, build ced0996

    docker volume create --name nexus-data
    nexus-data


    docker run -d -p 8081:8081 --name nexus -v nexus-data:/nexus-data sonatype/nexus3

        -d detached mode

        -p 8081:8081  - make accessible at 8081

        --name container name

        -v volume (named volume)

        sonatype/nexus3 Docker Image name


    root@ubuntu-s-4vcpu-8gb-nyc3-01:~# netstat -ltpn
    Active Internet connections (only servers)
    Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name    
    tcp        0      0 127.0.0.53:53           0.0.0.0:*               LISTEN      739/systemd-resolve 
    tcp        0      0 0.0.0.0:8081            0.0.0.0:*               LISTEN      15067/docker-proxy  
    tcp        0      0 127.0.0.54:53           0.0.0.0:*               LISTEN      739/systemd-resolve 
    tcp6       0      0 :::22                   :::*                    LISTEN      1/init              
    tcp6       0      0 :::8081                 :::*                    LISTEN      15074/docker-proxy 



    root@ubuntu-s-4vcpu-8gb-nyc3-01:~# docker exec -it nexus /bin/bash
    bash-4.4$ ls
    nexus  sonatype-work  start-nexus-repository-manager.sh
    bash-4.4$ whoami
    nexus


    root@ubuntu-s-4vcpu-8gb-nyc3-01:~# docker volume ls
    DRIVER    VOLUME NAME
    local     nexus-data
    root@ubuntu-s-4vcpu-8gb-nyc3-01:~# docker inspect nexus-data
    [
        {
            "CreatedAt": "2024-06-27T22:41:10Z",
            "Driver": "local",
            "Labels": null,
            "Mountpoint": "/var/snap/docker/common/var-lib-docker/volumes/nexus-data/_data",
            "Name": "nexus-data",
            "Options": null,
            "Scope": "local"
        }
    ]


From the HOST

root@ubuntu-s-4vcpu-8gb-nyc3-01:~# ls /var/snap/docker/common/var-lib-docker/volumes/nexus-data/_data
admin.password  blobs  cache  db  elasticsearch  etc  generated-bundles  instances  javaprefs  kar  karaf.pid  keystores  lock  log  orient  port  restore-from-backup  tmp


From the Container

root@ubuntu-s-4vcpu-8gb-nyc3-01:~# docker exec -it nexus /bin/bash
bash-4.4$ ls
nexus  sonatype-work  start-nexus-repository-manager.sh
bash-4.4$ pwd
/opt/sonatype
bash-4.4$ cd ../..
bash-4.4$ ls
bin  boot  dev	etc  home  lib	lib64  lost+found  media  mnt  nexus-data  opt	proc  root  run  sbin  srv  sys  tmp  usr  var
bash-4.4$ ls nexus-data/
admin.password	blobs  cache  db  elasticsearch  etc  generated-bundles  instances  javaprefs  kar  karaf.pid  keystores  lock	log  orient  port  restore-from-backup	tmp
bash-4.4$ 

# Usage

% ssh root@138.197.33.206
The authenticity of host '138.197.33.206 (138.197.33.206)' can't be established.
ED25519 key fingerprint is SHA256:r2QRt+WnlpUspO3Qc4IuFCPFml8C7ZLRBPaNkO5xTGM.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '138.197.33.206' (ED25519) to the list of known hosts.
Welcome to Ubuntu 24.04 LTS (GNU/Linux 6.8.0-31-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/pro

 System information as of Thu Jun 27 21:44:09 UTC 2024

  System load:  0.0                Processes:             140
  Usage of /:   1.2% of 153.94GB   Users logged in:       0
  Memory usage: 3%                 IPv4 address for eth0: 138.197.33.206
  Swap usage:   0%                 IPv4 address for eth0: 10.17.0.5

Expanded Security Maintenance for Applications is not enabled.

23 updates can be applied immediately.
To see these additional updates run: apt list --upgradable

Enable ESM Apps to receive additional future security updates.
See https://ubuntu.com/esm or run: sudo pro status


*** System restart required ***
Pending kernel upgrade!
Running kernel version:
  6.8.0-31-generic
Diagnostics:
  The currently running kernel version is not the expected kernel version 6.8.0-36-generic.

The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

root@ubuntu-s-4vcpu-8gb-nyc3-01:~# 
    

root@ubuntu-s-4vcpu-8gb-nyc3-01:~# snap install docker
2024-06-27T21:49:10Z INFO Waiting for automatic snapd restart...
docker 24.0.5 from Canonical✓ installed
root@ubuntu-s-4vcpu-8gb-nyc3-01:~# docker --version
Docker version 24.0.5, build ced0996
root@ubuntu-s-4vcpu-8gb-nyc3-01:~# docker 

@ubuntu-s-4vcpu-8gb-nyc3-01:~# docker --version
Docker version 24.0.5, build ced0996


docker volume create --name nexus-data
nexus-data


root@ubuntu-s-4vcpu-8gb-nyc3-01:~# docker run -d -p 8081:8081 --name nexus -v nexus-data:/nexus-data sonatype/nexus3
Unable to find image 'sonatype/nexus3:latest' locally
latest: Pulling from sonatype/nexus3
07a13d2c6b3e: Pull complete 
c421e9faafc8: Pull complete 
8ca13824e50d: Pull complete 
3332fce37130: Pull complete 
300cab84cb19: Pull complete 
2dfd5e577104: Pull complete 
eb34923815e7: Pull complete 
Digest: sha256:b3eeb90e5a17386252b8e2b27d6985d7bd9bfb574e40865c239adfa23d686ee5
Status: Downloaded newer image for sonatype/nexus3:latest
3cc194d8d45de20e26a6ce0b62421984a4a709c330dbb52c259824807849a703


root@ubuntu-s-4vcpu-8gb-nyc3-01:~# docker ps
CONTAINER ID   IMAGE             COMMAND                  CREATED         STATUS         PORTS                                       NAMES
3cc194d8d45d   sonatype/nexus3   "/opt/sonatype/nexus…"   2 minutes ago   Up 2 minutes   0.0.0.0:8081->8081/tcp, :::8081->8081/tcp   nexus


root@ubuntu-s-4vcpu-8gb-nyc3-01:~# netstat -ltpn
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name    
tcp        0      0 127.0.0.53:53           0.0.0.0:*               LISTEN      739/systemd-resolve 
tcp        0      0 0.0.0.0:8081            0.0.0.0:*               LISTEN      15067/docker-proxy  
tcp        0      0 127.0.0.54:53           0.0.0.0:*               LISTEN      739/systemd-resolve 
tcp6       0      0 :::22                   :::*                    LISTEN      1/init              
tcp6       0      0 :::8081                 :::*                    LISTEN      15074/docker-proxy 