# Chapter7
This module we explore what a Container is versus an Image

# Name: Container verus Image

# Description: 

Technically a container is made up of images    

        Docker Images

        Layers of images stacked on top of each other.

        At the base you would have a linux based image - alpine:3.17

        It important for them to be small, that's why most are alpine.

        This will ensure the container stays small, which is the reason for using containers.

        On top of the layer would be the intermediary applicaton layer - postgresql

    Requires Docker Desktop - allows the docker runtime engine to run in the background to support docker on your machine

        When installed the docker desktop will show Docker engine is running

    Run a Docker Image

        From docker hub we will pull down postgres image.

        Since this is a Public Repo

            no login is necessary

            no authentication is required

            docker run --name some-postgres -e POSTGRES_PASSWORD=mysecretpassword -d postgres
                
                -e sets up environment variables to run inside of the container

                docker run --name some-postgres -e POSTGRES_PASSWORD=mysecretpassword -d postgres:13.15

                13.15: Pulling from library/postgres - pulling from docker hub

                docker image is made up of layers

                2cc3ae149d28: Pull complete 
                e6fd2e12bb17: Pull complete 
                31b1d7acb36a: Pull complete 
                daed6e01d2bd: Pull complete 
                bef99882b45e: Pull complete 
                23ba358eeb18: Pull complete 
                e026dced26a8: Pull complete 
                dfddffae2362: Pull complete 
                93d519ce0602: Pull complete 
                ca1bdcd4df86: Pull complete 
                19ecff9742eb: Pull complete 
                c24ce334bc29: Pull complete 
                1d9383d6f424: Pull complete 
                8eb77e161de9: Pull complete 
                Digest: sha256:1386cc5f123a5f69f1ef42a423f8086c59a9001edf7ecca82baf010213691675
                Status: Downloaded newer image for postgres:13.15

                separate images are downloaded

                advantage: only different layers are downloaded. If they are already pulled they are not pulled


                If the next verions were to pulled down it probably take less time because some layers are already downloaded on the machine

                docker run - means to pull and start container

                michaelbradley@Michaels-iMac-2 Chapter7_Containers_With_Docker % docker ps -a
                CONTAINER ID   IMAGE            COMMAND                  CREATED          STATUS          PORTS      NAMES
                3b3b25fb8d76   postgres:13.15   "docker-entrypoint.s…"   14 minutes ago   Up 14 minutes   5432/tcp   some-postgres


                PostgreSQL init process complete; ready for start up.

                2024-06-20 02:04:05.371 UTC [1] LOG:  starting PostgreSQL 13.15 (Debian 13.15-1.pgdg120+1) on x86_64-pc-linux-gnu, compiled by gcc (Debian 12.2.0-14) 12.2.0, 64-bit
                2024-06-20 02:04:05.371 UTC [1] LOG:  listening on IPv4 address "0.0.0.0", port 5432
                2024-06-20 02:04:05.371 UTC [1] LOG:  listening on IPv6 address "::", port 5432
                2024-06-20 02:04:05.372 UTC [1] LOG:  listening on Unix socket "/var/run/postgresql/.s.PGSQL.5432"
                2024-06-20 02:04:05.375 UTC [61] LOG:  database system was shut down at 2024-06-20 02:04:05 UTC
                2024-06-20 02:04:05.378 UTC [1] LOG:  database system is ready to accept connections


                Docker Image                        Docker Container

                actual package                      actually start the application

                artifact that ca be moved           container environment is created

                not running                         running

                If I wanted to run a later version of the same application I only pull a separate version and specify tag information

                michaelbradley@Michaels-iMac-2 Chapter7_Containers_With_Docker % docker ps -a
                CONTAINER ID   IMAGE            COMMAND                  CREATED          STATUS          PORTS      NAMES
                350f92754f59   postgres:15.7    "docker-entrypoint.s…"   23 seconds ago   Up 21 seconds   5432/tcp   second-postgres
                3b3b25fb8d76   postgres:13.15   "docker-entrypoint.s…"   31 minutes ago   Up 31 minutes   5432/tcp   some-postgres
                michaelbradley@Michaels-iMac-2 Chapter7_Containers_With_Docker % 






# Usage



michaelbradley@Michaels-iMac-2 Chapter7_Containers_With_Docker % docker run --name some-postgres -e POSTGRES_PASSWORD=mysecretpassword -d postgres:13.15
Unable to find image 'postgres:13.15' locally
13.15: Pulling from library/postgres
2cc3ae149d28: Downloading [====================================>              ]  21.44MB/29.15MB
e6fd2e12bb17: Download complete 


michaelbradley@Michaels-iMac-2 Chapter7_Containers_With_Docker % docker logs some-postgres
The files belonging to this database system will be owned by user "postgres".
This user must also own the server process.

The database cluster will be initialized with locale "en_US.utf8".
The default database encoding has accordingly been set to "UTF8".
The default text search configuration will be set to "english".

Data page checksums are disabled.

fixing permissions on existing directory /var/lib/postgresql/data ... ok
creating subdirectories ... ok
selecting dynamic shared memory implementation ... posix
selecting default max_connections ... 100
selecting default shared_buffers ... 128MB
selecting default time zone ... Etc/UTC
creating configuration files ... ok
running bootstrap script ... ok
performing post-bootstrap initialization ... ok
syncing data to disk ... ok


Success. You can now start the database server using:

    pg_ctl -D /var/lib/postgresql/data -l logfile start

initdb: warning: enabling "trust" authentication for local connections
You can change this by editing pg_hba.conf or using the option -A, or
--auth-local and --auth-host, the next time you run initdb.
waiting for server to start....2024-06-20 02:04:05.108 UTC [48] LOG:  starting PostgreSQL 13.15 (Debian 13.15-1.pgdg120+1) on x86_64-pc-linux-gnu, compiled by gcc (Debian 12.2.0-14) 12.2.0, 64-bit
2024-06-20 02:04:05.129 UTC [48] LOG:  listening on Unix socket "/var/run/postgresql/.s.PGSQL.5432"
2024-06-20 02:04:05.132 UTC [49] LOG:  database system was shut down at 2024-06-20 02:04:04 UTC
2024-06-20 02:04:05.136 UTC [48] LOG:  database system is ready to accept connections
 done
server started

/usr/local/bin/docker-entrypoint.sh: ignoring /docker-entrypoint-initdb.d/*

waiting for server to shut down....2024-06-20 02:04:05.238 UTC [48] LOG:  received fast shutdown request
2024-06-20 02:04:05.239 UTC [48] LOG:  aborting any active transactions
2024-06-20 02:04:05.240 UTC [48] LOG:  background worker "logical replication launcher" (PID 55) exited with exit code 1
2024-06-20 02:04:05.241 UTC [50] LOG:  shutting down
2024-06-20 02:04:05.252 UTC [48] LOG:  database system is shut down
 done
server stopped

PostgreSQL init process complete; ready for start up.

2024-06-20 02:04:05.371 UTC [1] LOG:  starting PostgreSQL 13.15 (Debian 13.15-1.pgdg120+1) on x86_64-pc-linux-gnu, compiled by gcc (Debian 12.2.0-14) 12.2.0, 64-bit
2024-06-20 02:04:05.371 UTC [1] LOG:  listening on IPv4 address "0.0.0.0", port 5432
2024-06-20 02:04:05.371 UTC [1] LOG:  listening on IPv6 address "::", port 5432
2024-06-20 02:04:05.372 UTC [1] LOG:  listening on Unix socket "/var/run/postgresql/.s.PGSQL.5432"
2024-06-20 02:04:05.375 UTC [61] LOG:  database system was shut down at 2024-06-20 02:04:05 UTC
2024-06-20 02:04:05.378 UTC [1] LOG:  database system is ready to accept connections