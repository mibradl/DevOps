# Chapter7
This module we explore Docker commands

# Name: Main Docker commands

# Description: 

Overview 

    Container vs Images

        Container is a running environment for an Image

            Filesystem

                Virtual file system

            environment configs

                Port binded: talk to application running inside of container

            application image

                Application image: postgres, redis, mongo...

    Version and Tag

    Main Docker Commands

        docker pull

        docker run

        docker start

        docker stop

        docker ps

        docker exec -it

        docker logs

docker pull redis - to pull down the image

docker images - list all images on the machine, also shows tags, and size

docker run redis - starts the container

docker ps - displays listing of running containers

Ctl+C will terminate the docker process that was running and ran in the forefront of the shell

^C1:signal-handler (1719067898) Received SIGINT scheduling shutdown...
1:M 22 Jun 2024 14:51:38.073 * User requested shutdown...
1:M 22 Jun 2024 14:51:38.075 * Saving the final RDB snapshot before exiting.
1:M 22 Jun 2024 14:51:38.082 * DB saved on disk
1:M 22 Jun 2024 14:51:38.083 # Redis is now ready to exit, bye bye...

% docker run -d redis       run the container in the background wth -d for detached mode
cb8923b0e23585c064915697cbdbc03f07c1d3cbc354fa6092ada289c9bb4053


% docker ps             List the containers running and the container id, status, ports, etc

CONTAINER ID   IMAGE            COMMAND                  CREATED         STATUS         PORTS      NAMES
cb8923b0e235   redis            "docker-entrypoint.s…"   2 minutes ago   Up 2 minutes   6379/tcp   nice_banzai

docker ps -a                        this comand will show all containsers

CONTAINER ID   IMAGE            COMMAND                  CREATED          STATUS                      PORTS      NAMES
cb8923b0e235   redis            "docker-entrypoint.s…"   16 minutes ago   Up 6 minutes                6379/tcp   nice_banzai
8dd9a11288bf   redis            "docker-entrypoint.s…"   13 hours ago     Exited (0) 18 minutes ago              crazy_yonath
350f92754f59   postgres:15.7    "docker-entrypoint.s…"   2 days ago       Up 2 days                   5432/tcp   second-postgres
3b3b25fb8d76   postgres:13.15   "docker-entrypoint.s…"   2 days ago       Up 2 days                   5432/tcp   some-postgres

Container Port vs Host Port

    Multiple containers can run on your host machine

    Your laptop has only certain ports available

        You need to create a binding between the host port and the container port

    You will cause a conflict when using the same port on the host machine

        some-app://localhost:3001

    michaelbradley@Michaels-iMac-2 Chapter7_Containers_With_Docker % docker run -p 6000:6379 -d redis
    b83c87cc01491344a0b0b49f03c59e99d99260cb3d432ccd8540a15182f80b54

    If you try to start another version on the same host port it will fail.

    michaelbradley@Michaels-iMac-2 Chapter7_Containers_With_Docker % docker run -p 6000:6379 -d redis:6.2
    2dae27d3e61db537a4fdfb4fe560f080b215d9a3039fc80f2fa1eff72ab45101
    docker: Error response from daemon: driver failed programming external connectivity on endpoint sad_ptolemy (68c1795a341f354f0675765ab9b0c266a617310f3a7bc7390ee62a5ad604d223): Bind for 0.0.0.0:6000 failed: port is already allocated.

    michaelbradley@Michaels-iMac-2 Chapter7_Containers_With_Docker % docker run -p 6001:6379 -d redis:6.2
    3b445f8c1c535c343d3207e0f4946b367b7c4c8ecbf3e2b8bfd8513297c41b15
    michaelbradley@Michaels-iMac-2 Chapter7_Containers_With_Docker % docker ps -a
    CONTAINER ID   IMAGE            COMMAND                  CREATED          STATUS                      PORTS                    NAMES
    3b445f8c1c53   redis:6.2        "docker-entrypoint.s…"   10 seconds ago   Up 10 seconds               0.0.0.0:6001->6379/tcp   goofy_gould
    2dae27d3e61d   redis:6.2        "docker-entrypoint.s…"   2 minutes ago    Created                                              sad_ptolemy
    b83c87cc0149   redis            "docker-entrypoint.s…"   6 minutes ago    Up 6 minutes                0.0.0.0:6000->6379/tcp   peaceful_roentgen
    25135d670cb4   redis            "docker-entrypoint.s…"   7 minutes ago    Exited (0) 6 minutes ago                             funny_snyder
    e196536b49fb   redis:6.2        "docker-entrypoint.s…"   18 minutes ago   Exited (0) 14 minutes ago                            gallant_diffie
    350f92754f59   postgres:15.7    "docker-entrypoint.s…"   2 days ago       Up 2 days                   5432/tcp                 second-postgres
    3b3b25fb8d76   postgres:13.15   "docker-entrypoint.s…"   2 days ago       Up 2 days                   5432/tcp                 some-postgres
    michaelbradley@Michaels-iMac-2 Chapter7_Containers_With_Docker % 

    

# Usage

% docker pull redis
Using default tag: latest
latest: Pulling from library/redis
2cc3ae149d28: Already exists 
916a4f350e12: Pull complete 
b41a54a9a617: Pull complete 
a32d5b47cfbb: Pull complete 
8b29e70f14b1: Pull complete 
a8e51fa2ab60: Pull complete 
4f4fb700ef54: Pull complete 
9fe463190b6a: Pull complete 
Digest: sha256:e422889e156ebea83856b6ff973bfe0c86bce867d80def228044eeecf925592b
Status: Downloaded newer image for redis:latest
docker.io/library/redis:latest

% docker images
REPOSITORY   TAG       IMAGE ID       CREATED       SIZE
redis        latest    aceb1262c1ea   4 weeks ago   117MB
postgres     15.7      2f77c5a59ad5   6 weeks ago   425MB
postgres     13.15     f82ad01a3be6   6 weeks ago   419MB
    

docker run redis
1:C 22 Jun 2024 02:24:32.070 * oO0OoO0OoO0Oo Redis is starting oO0OoO0OoO0Oo
1:C 22 Jun 2024 02:24:32.070 * Redis version=7.2.5, bits=64, commit=00000000, modified=0, pid=1, just started
1:C 22 Jun 2024 02:24:32.070 # Warning: no config file specified, using the default config. In order to specify a config file use redis-server /path/to/redis.conf
1:M 22 Jun 2024 02:24:32.070 * monotonic clock: POSIX clock_gettime
1:M 22 Jun 2024 02:24:32.071 * Running mode=standalone, port=6379.
1:M 22 Jun 2024 02:24:32.073 * Server initialized
1:M 22 Jun 2024 02:24:32.073 * Ready to accept connections tcp


% docker ps
CONTAINER ID   IMAGE            COMMAND                  CREATED         STATUS         PORTS      NAMES
8dd9a11288bf   redis            "docker-entrypoint.s…"   3 minutes ago   Up 3 minutes   6379/tcp   crazy_yonath
350f92754f59   postgres:15.7    "docker-entrypoint.s…"   2 days ago      Up 2 days      5432/tcp   second-postgres
3b3b25fb8d76   postgres:13.15   "docker-entrypoint.s…"   2 days ago      Up 2 days      5432/tcp   some-postgres
michaelbradley@Michaels-iMac-2.local /Users/michaelbradley 
% 


% docker run -d redis
cb8923b0e23585c064915697cbdbc03f07c1d3cbc354fa6092ada289c9bb4053

docker cb8923b0e23585c064915697cbdbc03f07c1d3cbc354fa6092ada289c9bb4053 stop
docker cb8923b0e23585c064915697cbdbc03f07c1d3cbc354fa6092ada289c9bb4053 start

% docker ps
CONTAINER ID   IMAGE            COMMAND                  CREATED         STATUS         PORTS      NAMES
cb8923b0e235   redis            "docker-entrypoint.s…"   2 minutes ago   Up 2 minutes   6379/tcp   nice_banzai
350f92754f59   postgres:15.7    "docker-entrypoint.s…"   2 days ago      Up 2 days      5432/tcp   second-postgres
3b3b25fb8d76   postgres:13.15   "docker-entrypoint.s…"   2 days ago      Up 2 days      5432/tcp   some-postgres

 for i in cb8923b0e235
do
docker stop $i          
docker start $i
done

% for i in cb8923b0e235
do
docker stop $i
docker start $i
docker ps
for> done
cb8923b0e235
cb8923b0e235
CONTAINER ID   IMAGE            COMMAND                  CREATED         STATUS                  PORTS      NAMES
cb8923b0e235   redis            "docker-entrypoint.s…"   9 minutes ago   Up Less than a second   6379/tcp   nice_banzai
350f92754f59   postgres:15.7    "docker-entrypoint.s…"   2 days ago      Up 2 days               5432/tcp   second-postgres
3b3b25fb8d76   postgres:13.15   "docker-entrypoint.s…"   2 days ago      Up 2 days               5432/tcp   some-postgres
michaelbradley@Michaels-iMac-2.local


docker ps -a
CONTAINER ID   IMAGE            COMMAND                  CREATED          STATUS                      PORTS      NAMES
cb8923b0e235   redis            "docker-entrypoint.s…"   16 minutes ago   Up 6 minutes                6379/tcp   nice_banzai
8dd9a11288bf   redis            "docker-entrypoint.s…"   13 hours ago     Exited (0) 18 minutes ago              crazy_yonath
350f92754f59   postgres:15.7    "docker-entrypoint.s…"   2 days ago       Up 2 days                   5432/tcp   second-postgres
3b3b25fb8d76   postgres:13.15   "docker-entrypoint.s…"   2 days ago       Up 2 days                   5432/tcp   some-postgres