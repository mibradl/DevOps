# Chapter7
This module we look at debugging docker containers.

# Name: Debugging Docker Containers

# Description: 

This is part 2 of docker commands

We looked at the following commands in the previous session

    docker pull redis - to pull down the image

    docker pull redis - to pull down the image

    docker images - list all images on the machine, also shows tags, and size

    docker run redis - starts the container

    docker ps - displays listing of running containers

    Ctl+C will terminate the docker process that was running and ran in the forefront of the shell

    docker ps - displays listing of running containers

    docker ps -a - displays all containers

    docker run -d redis - run the container in the background wth -d for detached mode

    docker run -d -p 6000:3000  -allows you to bind host port to the container

    docker images - displays all images on the machine

Docker for troubleshooting

    docker logs 3b445f8c1c53 - use id

    docker logs goofy_gould - use name

    1:C 22 Jun 2024 17:09:43.516 # oO0OoO0OoO0Oo Redis is starting oO0OoO0OoO0Oo
    1:C 22 Jun 2024 17:09:43.516 # Redis version=6.2.14, bits=64, commit=00000000, modified=0, pid=1, just started
    1:C 22 Jun 2024 17:09:43.516 # Warning: no config file specified, using the default config. In order to specify a config file use redis-server /path/to/redis.conf
    1:M 22 Jun 2024 17:09:43.517 * monotonic clock: POSIX clock_gettime
    1:M 22 Jun 2024 17:09:43.517 * Running mode=standalone, port=6379.
    1:M 22 Jun 2024 17:09:43.517 # Server initialized
    1:M 22 Jun 2024 17:09:43.517 * Ready to accept connections

    docker run -d -p 6001:6379 --name  redis-older redis:6.2  - assign a name redis-older
    12e7e7fbe6dbacdcd5f0d3861e32e5f648de0c3d3061f2051ec1b27c75f87c28


    docker run -d -p 6000:6379 --name  redis-latest redis - assign a name redis-latest
    52eb3407d6237d0fcb1b62a2dc2cfdb91384bf5262481c7c9c51e5cd369b4416


    michaelbradley@Michaels-iMac-2 Chapter7_Containers_With_Docker % docker ps                                            
    CONTAINER ID   IMAGE            COMMAND                  CREATED          STATUS          PORTS                    NAMES
    52eb3407d623   redis            "docker-entrypoint.s…"   15 seconds ago   Up 14 seconds   0.0.0.0:6000->6379/tcp   redis-latest
    12e7e7fbe6db   redis:6.2        "docker-entrypoint.s…"   6 minutes ago    Up 6 minutes    0.0.0.0:6001->6379/tcp   redis-older
    350f92754f59   postgres:15.7    "docker-entrypoint.s…"   2 days ago       Up 2 days       5432/tcp                 second-postgres
    3b3b25fb8d76   postgres:13.15   "docker-entrypoint.s…"   2 days ago       Up 2 days       5432/tcp                 some-postgres

    docker exec -it redis-latest /bin/bash = login to container id using id or name
    root@52eb3407d623:/data#




# Usage

michaelbradley@Michaels-iMac-2 Chapter7_Containers_With_Docker % docker logs 3b445f8c1c53
1:C 22 Jun 2024 17:09:43.516 # oO0OoO0OoO0Oo Redis is starting oO0OoO0OoO0Oo
1:C 22 Jun 2024 17:09:43.516 # Redis version=6.2.14, bits=64, commit=00000000, modified=0, pid=1, just started
1:C 22 Jun 2024 17:09:43.516 # Warning: no config file specified, using the default config. In order to specify a config file use redis-server /path/to/redis.conf
1:M 22 Jun 2024 17:09:43.517 * monotonic clock: POSIX clock_gettime
1:M 22 Jun 2024 17:09:43.517 * Running mode=standalone, port=6379.
1:M 22 Jun 2024 17:09:43.517 # Server initialized
1:M 22 Jun 2024 17:09:43.517 * Ready to accept connections

michaelbradley@Michaels-iMac-2 Chapter7_Containers_With_Docker % docker logs goofy_gould
1:C 22 Jun 2024 17:09:43.516 # oO0OoO0OoO0Oo Redis is starting oO0OoO0OoO0Oo
1:C 22 Jun 2024 17:09:43.516 # Redis version=6.2.14, bits=64, commit=00000000, modified=0, pid=1, just started
1:C 22 Jun 2024 17:09:43.516 # Warning: no config file specified, using the default config. In order to specify a config file use redis-server /path/to/redis.conf
1:M 22 Jun 2024 17:09:43.517 * monotonic clock: POSIX clock_gettime
1:M 22 Jun 2024 17:09:43.517 * Running mode=standalone, port=6379.
1:M 22 Jun 2024 17:09:43.517 # Server initialized
1:M 22 Jun 2024 17:09:43.517 * Ready to accept connections
    

michaelbradley@Michaels-iMac-2 Chapter7_Containers_With_Docker % docker run -d -p 6001:6379 --name  redis-older redis:6.2
12e7e7fbe6dbacdcd5f0d3861e32e5f648de0c3d3061f2051ec1b27c75f87c28


michaelbradley@Michaels-iMac-2 Chapter7_Containers_With_Docker % docker ps
CONTAINER ID   IMAGE            COMMAND                  CREATED              STATUS              PORTS                    NAMES
12e7e7fbe6db   redis:6.2        "docker-entrypoint.s…"   About a minute ago   Up About a minute   0.0.0.0:6001->6379/tcp   redis-older
350f92754f59   postgres:15.7    "docker-entrypoint.s…"   2 days ago           Up 2 days           5432/tcp                 second-postgres
3b3b25fb8d76   postgres:13.15   "docker-entrypoint.s…"   2 days ago           Up 2 days           5432/tcp                 some-postgres

michaelbradley@Michaels-iMac-2 Chapter7_Containers_With_Docker % docker run -d -p 6000:6379 --name  redis-latest redis
52eb3407d6237d0fcb1b62a2dc2cfdb91384bf5262481c7c9c51e5cd369b4416


michaelbradley@Michaels-iMac-2 Chapter7_Containers_With_Docker % docker ps                                            
CONTAINER ID   IMAGE            COMMAND                  CREATED          STATUS          PORTS                    NAMES
52eb3407d623   redis            "docker-entrypoint.s…"   15 seconds ago   Up 14 seconds   0.0.0.0:6000->6379/tcp   redis-latest
12e7e7fbe6db   redis:6.2        "docker-entrypoint.s…"   6 minutes ago    Up 6 minutes    0.0.0.0:6001->6379/tcp   redis-older
350f92754f59   postgres:15.7    "docker-entrypoint.s…"   2 days ago       Up 2 days       5432/tcp                 second-postgres
3b3b25fb8d76   postgres:13.15   "docker-entrypoint.s…"   2 days ago       Up 2 days       5432/tcp                 some-postgres


docker exec -it redis-latest /bin/bash
root@52eb3407d623:/data# ls
root@52eb3407d623:/data# pwd
/data
root@52eb3407d623:/data# cd /
root@52eb3407d623:/# ls
bin  boot  data  dev  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
root@52eb3407d623:/# cd var 
root@52eb3407d623:/var# ls


root@52eb3407d623:/var/log# env
HOSTNAME=52eb3407d623
REDIS_DOWNLOAD_SHA=5981179706f8391f03be91d951acafaeda91af7fac56beffb2701963103e423d
PWD=/var/log
HOME=/root
REDIS_VERSION=7.2.5
GOSU_VERSION=1.17
TERM=xterm
REDIS_DOWNLOAD_URL=http://download.redis.io/releases/redis-7.2.5.tar.gz
SHLVL=1
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
_=/usr/bin/env
OLDPWD=/var
root@52eb3407d623:/var/log# exit