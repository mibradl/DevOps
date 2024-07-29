# Chapter9
This module we will continue deploy to EC2 server from CICD pipeline Part 2.

# Name: Deploy to EC2 server from Jenkins Pipeline CICD

# Description: 

Using Docker Compose for Deployment

        Just started 1 Docker Container - docker run

        Small application with multiple services would start with Docker Compose

            Start with docker-compose command

            Run yaml file

            docker-compose -f docker-compose.yaml up


        Install docker-compose on EC2 Instance

        Create docker-compose.yaml file

        Adjust Jenkinsfile to execute docker-compose command on EC2 Instance


Install Docker Compose


        docker --version
        Docker version 25.0.3, build 4debf41

        docker-compose
        -bash: docker-compose: command not found


        Install docker-compose using binaries
        sudo curl -L https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m) -o /usr/local/bin/docker-compose
        
        sudo chmod +x /usr/local/bin/docker-compose



Create Docker Compose File

    1) Our Java Maven Docker Image

    2) Third-party Postgres Docker Image

        version: '3.8'
        services:
            java-maven-app:
            image: bradlmi/demo-app:aap-1.0
            ports:
                - 8080:8080

            postgres:
            image: postgres:15
            ports:
                - 5432:5432
            environment:
                - POSTGRES_PASSWORD=my-pwd


Make Jenkinsfile adjustments

    Need the docker-compose.yaml file on EC2 Instance!

    [ec2-user@ip-172-31-89-89 ~]$ ls -l
    total 4
    -rw-r--r--. 1 ec2-user ec2-user 243 Jul 29 02:15 docker-compose.yaml


    [ec2-user@ip-172-31-89-89 ~]$ docker ps
    CONTAINER ID   IMAGE                      COMMAND                  CREATED         STATUS         PORTS                                                 NAMES
    7bb3a83ca096   bradlmi/demo-app:aap-1.0   "docker-entrypoint.s…"   5 minutes ago   Up 5 minutes   3080/tcp, 0.0.0.0:8080->8080/tcp, :::8080->8080/tcp   ec2-user-java-maven-app-1
    2bcdb7ff37b0   postgres:15                "docker-entrypoint.s…"   5 minutes ago   Up 5 minutes   0.0.0.0:5432->5432/tcp, :::5432->5432/tcp             ec2-user-postgres-1
    [ec2-user@ip-172-31-89-89 ~]$

Improvement: Extract to Shell Script


    Jenkinsfile


    pipeline {   
    agent any
    tools {
        maven 'maven-3.9'
    }
    environment {
        IMAGE_NAME = 'bradlmi/demo-app:aap-2.0'    //Create environment variable for image tag endpoint on private docker repo
    }

    stages {
        stage('build app') {
            steps {
                echo 'building application jar...'
                buildJar()


    Deploy to EC2 instance

    stage("deploy") {
            steps {
                script {
                    echo 'deploying docker image to EC2...'
                    def shellCmd = "bash ./server-cmds.sh ${IMAGE_NAME}"
                    def ec2Instance = "ec2-user@44.203.54.164"
                    sshagent(['ec2-server-key']) {
                        sh "scp server-cmds.sh c:/home/ec2-user"
                        sh "scp docker-compose.yaml ${ec2Instance}:/home/ec2-user"
                        sh "ssh -o StrictHostKeyChecking=no ${ec2Instance} ${shellCmd}"
                    }
                }
            }


    Picked up variable in shell script on ec2 instance

    #!/usr/bin/env bash

    export IMAGE=$1
    docker-compose -f docker-compose.yaml up --detach
    echo "success"


    docker-compose -f docker-compose.yaml down
    WARN[0000] /home/ec2-user/docker-compose.yaml: the attribute `version` is obsolete, it will be ignored, please remove it to avoid potential confusion 
    [+] Running 3/3
    ✔ Container ec2-user-java-maven-app-1  Removed                                                                                                                                           10.3s 
    ✔ Container ec2-user-postgres-1        Removed                                                                                                                                            0.3s 
    ✔ Network ec2-user_default             Removed 


    docker ps
    CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES



Perform build from Jenkins

    [Pipeline] sh
    + scp server-cmds.sh ec2-user@44.203.54.164:/home/ec2-user
    [Pipeline] sh
    + scp docker-compose.yaml ec2-user@44.203.54.164:/home/ec2-user
    [Pipeline] sh
    + ssh -o StrictHostKeyChecking=no ec2-user@44.203.54.164 bash ./server-cmds.sh
    time="2024-07-29T03:03:02Z" level=warning msg="/home/ec2-user/docker-compose.yaml: the attribute `version` is obsolete, it will be ignored, please remove it to avoid potential confusion"
    Network ec2-user_default  Creating
    Network ec2-user_default  Created
    Container ec2-user-postgres-1  Creating
    Container ec2-user-java-maven-app-1  Creating
    Container ec2-user-postgres-1  Created
    Container ec2-user-java-maven-app-1  Created
    Container ec2-user-java-maven-app-1  Starting
    Container ec2-user-postgres-1  Starting
    Container ec2-user-java-maven-app-1  Started
    Container ec2-user-postgres-1  Started
    success

    docker ps

    CONTAINER ID   IMAGE                      COMMAND                  CREATED         STATUS         PORTS                                                 NAMES
    a0dd9dce8c6f   postgres:15                "docker-entrypoint.s…"   4 minutes ago   Up 4 minutes   0.0.0.0:5432->5432/tcp, :::5432->5432/tcp             ec2-user-postgres-1
    aafcf3688448   bradlmi/demo-app:aap-1.0   "docker-entrypoint.s…"   4 minutes ago   Up 4 minutes   3080/tcp, 0.0.0.0:8080->8080/tcp, :::8080->8080/tcp   ec2-user-java-maven-app-1
    [ec2-user@ip-172-31-89-89 ~]$ 


Replace Docker image with newly build verion

    When Jenkins pipeline runs: new docker image verion is built and pushed to repo

    From Jenkinsfile to Shell Script via parameter

    Access via $1 Shell Script
    + export environment variable on EC2

    [Pipeline] sh
    + scp server-cmds.sh ec2-user@44.203.54.164:/home/ec2-user
    [Pipeline] sh
    + scp docker-compose.yaml ec2-user@44.203.54.164:/home/ec2-user
    [Pipeline] sh
    + ssh -o StrictHostKeyChecking=no ec2-user@44.203.54.164 bash ./server-cmds.sh bradlmi/demo-app:aap-2.0


# Usage


[ec2-user@ip-172-31-89-89 ~]$ sudo curl -L https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m) -o /usr/local/bin/docker-compose
        % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                        Dload  Upload   Total   Spent    Left  Speed
        0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0
        0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0
        100 60.2M  100 60.2M    0     0  88.2M      0 --:--:-- --:--:-- --:--:-- 88.2M    


[ec2-user@ip-172-31-89-89 ~]$ ls -l /usr/local/bin/docker-compose 
-rwxr-xr-x. 1 root root 63160243 Jul 29 00:55 /usr/local/bin/docker-compose

[ec2-user@ip-172-31-89-89 ~]$ docker-compose --version
Docker Compose version v2.29.1



deploying docker image to EC2...
[Pipeline] sshagent
[ssh-agent] Using credentials ec2-user
[ssh-agent] Looking for ssh-agent implementation...
[ssh-agent]   Exec ssh-agent (binary ssh-agent on a remote machine)
$ ssh-agent
SSH_AUTH_SOCK=/tmp/ssh-XXXXXX2uGPeH/agent.158934
SSH_AGENT_PID=158937
Running ssh-add (command line suppressed)
Identity added: /var/jenkins_home/workspace/ultibranch-pipeline_jenkins-jobs@tmp/private_key_7443355236136385416.key (/var/jenkins_home/workspace/ultibranch-pipeline_jenkins-jobs@tmp/private_key_7443355236136385416.key)
[ssh-agent] Started.
[Pipeline] {
[Pipeline] sh
+ scp docker-compose.yaml ec2-user@44.203.54.164:/home/ec2-user
[Pipeline] sh
+ ssh -o StrictHostKeyChecking=no ec2-user@44.203.54.164 docker-compose -f docker-compose.yaml up --detach
time="2024-07-29T02:15:43Z" level=warning msg="/home/ec2-user/docker-compose.yaml: the attribute `version` is obsolete, it will be ignored, please remove it to avoid potential confusion"
 postgres Pulling 
 efc2b5ad9eec Pulling fs layer 
 5a5182daaaef Pulling fs layer 
 9bd58f90d997 Pulling fs layer 
 2ef118f9f903 Pulling fs layer 
 cc5a81cd1ffc Pulling fs layer 
 e92d4f57ba22 Pulling fs layer 
 fc2c1d24e9ca Pulling fs layer 
 c77b1b16c09f Pulling fs layer 
 1c1d8adbc955 Pulling fs layer 
 9154ac3ec50f Pulling fs layer 
 24dc5078fe3b Pulling fs layer 
 68acc6218bcf Pulling fs layer 
 532895c2736a Pulling fs layer 
 a08535c970e7 Pulling fs layer 
 2ef118f9f903 Waiting 
 cc5a81cd1ffc Waiting 
 e92d4f57ba22 Waiting 
 fc2c1d24e9ca Waiting 
 c77b1b16c09f Waiting 
 1c1d8adbc955 Waiting 
 9154ac3ec50f Waiting 
 24dc5078fe3b Waiting 
 68acc6218bcf Waiting 
 532895c2736a Waiting 
 a08535c970e7 Waiting 
 9bd58f90d997 Downloading [>                                                  ]  49.56kB/4.534MB
 5a5182daaaef Downloading [==================================================>]  1.162kB/1.162kB
 5a5182daaaef Verifying Checksum 
 5a5182daaaef Download complete 
 9bd58f90d997 Verifying Checksum 
 9bd58f90d997 Download complete 
 efc2b5ad9eec Downloading [>                                                  ]  293.3kB/29.13MB
 efc2b5ad9eec Downloading [=================>                                 ]  10.36MB/29.13MB
 efc2b5ad9eec Downloading [===================================>               ]  20.83MB/29.13MB
 2ef118f9f903 Downloading [>                                                  ]  23.96kB/1.447MB
 cc5a81cd1ffc Downloading [>                                                  ]  85.49kB/8.066MB
 efc2b5ad9eec Verifying Checksum 
 efc2b5ad9eec Download complete 
 cc5a81cd1ffc Downloading [==============================>                    ]   4.96MB/8.066MB
 2ef118f9f903 Verifying Checksum 
 2ef118f9f903 Download complete 
 efc2b5ad9eec Extracting [>                                                  ]  294.9kB/29.13MB
 cc5a81cd1ffc Verifying Checksum 
 cc5a81cd1ffc Download complete 
 e92d4f57ba22 Downloading [>                                                  ]  16.38kB/1.196MB
 fc2c1d24e9ca Downloading [==================================================>]     116B/116B
 fc2c1d24e9ca Verifying Checksum 
 fc2c1d24e9ca Download complete 
 c77b1b16c09f Downloading [==================================================>]  3.143kB/3.143kB
 c77b1b16c09f Verifying Checksum 
 c77b1b16c09f Download complete 
 efc2b5ad9eec Extracting [==>                                                ]  1.475MB/29.13MB
 e92d4f57ba22 Downloading [==================================================>]  1.196MB/1.196MB
 e92d4f57ba22 Verifying Checksum 
 e92d4f57ba22 Download complete 
 1c1d8adbc955 Downloading [>                                                  ]  536.5kB/106.9MB
 9154ac3ec50f Downloading [==================================================>]  9.779kB/9.779kB
 9154ac3ec50f Verifying Checksum 
 9154ac3ec50f Download complete 
 24dc5078fe3b Downloading [==================================================>]     128B/128B
 24dc5078fe3b Verifying Checksum 
 24dc5078fe3b Download complete 
 efc2b5ad9eec Extracting [====>                                              ]  2.359MB/29.13MB
 1c1d8adbc955 Downloading [===>                                               ]  7.552MB/106.9MB
 1c1d8adbc955 Downloading [========>                                          ]  17.81MB/106.9MB
 efc2b5ad9eec Extracting [====>                                              ]  2.654MB/29.13MB
 532895c2736a Downloading [==================================================>]  5.414kB/5.414kB
 532895c2736a Verifying Checksum 
 532895c2736a Download complete 
 68acc6218bcf Downloading [==================================================>]     170B/170B
 68acc6218bcf Verifying Checksum 
 68acc6218bcf Download complete 
 1c1d8adbc955 Downloading [===========>                                       ]  24.28MB/106.9MB
 a08535c970e7 Downloading [==================================================>]     185B/185B
 a08535c970e7 Verifying Checksum 
 a08535c970e7 Download complete 
 efc2b5ad9eec Extracting [=====>                                             ]  2.949MB/29.13MB
 1c1d8adbc955 Downloading [================>                                  ]  34.53MB/106.9MB
 efc2b5ad9eec Extracting [======>                                            ]  3.539MB/29.13MB
 1c1d8adbc955 Downloading [===================>                               ]  42.09MB/106.9MB
 1c1d8adbc955 Downloading [=======================>                           ]  49.64MB/106.9MB
 1c1d8adbc955 Downloading [============================>                      ]   59.9MB/106.9MB
 efc2b5ad9eec Extracting [=======>                                           ]  4.424MB/29.13MB
 1c1d8adbc955 Downloading [================================>                  ]  68.53MB/106.9MB
 efc2b5ad9eec Extracting [=========>                                         ]  5.308MB/29.13MB
 1c1d8adbc955 Downloading [===================================>               ]  76.63MB/106.9MB
 efc2b5ad9eec Extracting [==========>                                        ]  6.193MB/29.13MB
 1c1d8adbc955 Downloading [=======================================>           ]  84.72MB/106.9MB
 1c1d8adbc955 Downloading [===========================================>       ]  92.28MB/106.9MB
 efc2b5ad9eec Extracting [===========>                                       ]  6.783MB/29.13MB
 1c1d8adbc955 Downloading [===============================================>   ]  100.9MB/106.9MB
 1c1d8adbc955 Verifying Checksum 
 1c1d8adbc955 Download complete 
 efc2b5ad9eec Extracting [=============>                                     ]  7.668MB/29.13MB
 efc2b5ad9eec Extracting [================>                                  ]  9.437MB/29.13MB
 efc2b5ad9eec Extracting [===================>                               ]  11.21MB/29.13MB
 efc2b5ad9eec Extracting [======================>                            ]  13.27MB/29.13MB
 efc2b5ad9eec Extracting [===========================>                       ]  16.22MB/29.13MB
 efc2b5ad9eec Extracting [=================================>                 ]  19.76MB/29.13MB
 efc2b5ad9eec Extracting [======================================>            ]  22.41MB/29.13MB
 efc2b5ad9eec Extracting [==========================================>        ]  24.48MB/29.13MB
 efc2b5ad9eec Extracting [==========================================>        ]  24.77MB/29.13MB
 efc2b5ad9eec Extracting [==============================================>    ]  27.13MB/29.13MB
 efc2b5ad9eec Extracting [================================================>  ]  28.02MB/29.13MB
 efc2b5ad9eec Extracting [================================================>  ]  28.31MB/29.13MB
 efc2b5ad9eec Extracting [==================================================>]  29.13MB/29.13MB
 efc2b5ad9eec Pull complete 
 5a5182daaaef Extracting [==================================================>]  1.162kB/1.162kB
 5a5182daaaef Extracting [==================================================>]  1.162kB/1.162kB
 5a5182daaaef Pull complete 
 9bd58f90d997 Extracting [>                                                  ]  65.54kB/4.534MB
 9bd58f90d997 Extracting [===============================>                   ]  2.884MB/4.534MB
 9bd58f90d997 Extracting [==================================================>]  4.534MB/4.534MB
 9bd58f90d997 Pull complete 
 2ef118f9f903 Extracting [=>                                                 ]  32.77kB/1.447MB
 2ef118f9f903 Extracting [==================================================>]  1.447MB/1.447MB
 2ef118f9f903 Pull complete 
 cc5a81cd1ffc Extracting [>                                                  ]   98.3kB/8.066MB
 cc5a81cd1ffc Extracting [=================>                                 ]  2.851MB/8.066MB
 cc5a81cd1ffc Extracting [===========================>                       ]  4.424MB/8.066MB
 cc5a81cd1ffc Extracting [==============================>                    ]  4.915MB/8.066MB
 cc5a81cd1ffc Extracting [=====================================>             ]  6.095MB/8.066MB
 cc5a81cd1ffc Extracting [================================================>  ]  7.864MB/8.066MB
 cc5a81cd1ffc Extracting [==================================================>]  8.066MB/8.066MB
 cc5a81cd1ffc Pull complete 
 e92d4f57ba22 Extracting [=>                                                 ]  32.77kB/1.196MB
 e92d4f57ba22 Extracting [==================================================>]  1.196MB/1.196MB
 e92d4f57ba22 Pull complete 
 fc2c1d24e9ca Extracting [==================================================>]     116B/116B
 fc2c1d24e9ca Extracting [==================================================>]     116B/116B
 fc2c1d24e9ca Pull complete 
 c77b1b16c09f Extracting [==================================================>]  3.143kB/3.143kB
 c77b1b16c09f Extracting [==================================================>]  3.143kB/3.143kB
 c77b1b16c09f Pull complete 
 1c1d8adbc955 Extracting [>                                                  ]  557.1kB/106.9MB
 1c1d8adbc955 Extracting [=>                                                 ]  2.785MB/106.9MB
 1c1d8adbc955 Extracting [==>                                                ]  6.128MB/106.9MB
 1c1d8adbc955 Extracting [===>                                               ]  8.356MB/106.9MB
 1c1d8adbc955 Extracting [=====>                                             ]  11.14MB/106.9MB
 1c1d8adbc955 Extracting [======>                                            ]  13.93MB/106.9MB
 1c1d8adbc955 Extracting [=======>                                           ]  16.71MB/106.9MB
 1c1d8adbc955 Extracting [========>                                          ]  18.94MB/106.9MB
 1c1d8adbc955 Extracting [==========>                                        ]  21.73MB/106.9MB
 1c1d8adbc955 Extracting [===========>                                       ]  23.95MB/106.9MB
 1c1d8adbc955 Extracting [============>                                      ]  26.18MB/106.9MB
 1c1d8adbc955 Extracting [=============>                                     ]  29.52MB/106.9MB
 1c1d8adbc955 Extracting [===============>                                   ]  32.31MB/106.9MB
 1c1d8adbc955 Extracting [================>                                  ]  35.65MB/106.9MB
 1c1d8adbc955 Extracting [==================>                                ]  38.99MB/106.9MB
 1c1d8adbc955 Extracting [===================>                               ]  41.78MB/106.9MB
 1c1d8adbc955 Extracting [=====================>                             ]  45.12MB/106.9MB
 1c1d8adbc955 Extracting [======================>                            ]  48.46MB/106.9MB
 1c1d8adbc955 Extracting [=======================>                           ]  50.69MB/106.9MB
 1c1d8adbc955 Extracting [========================>                          ]  51.81MB/106.9MB
 1c1d8adbc955 Extracting [=========================>                         ]  53.48MB/106.9MB
 1c1d8adbc955 Extracting [=========================>                         ]  55.15MB/106.9MB
 1c1d8adbc955 Extracting [==========================>                        ]  57.38MB/106.9MB
 1c1d8adbc955 Extracting [===========================>                       ]  59.05MB/106.9MB
 1c1d8adbc955 Extracting [=============================>                     ]  62.39MB/106.9MB
 1c1d8adbc955 Extracting [==============================>                    ]  65.73MB/106.9MB
 1c1d8adbc955 Extracting [================================>                  ]  69.07MB/106.9MB
 1c1d8adbc955 Extracting [=================================>                 ]  71.86MB/106.9MB
 1c1d8adbc955 Extracting [==================================>                ]  74.65MB/106.9MB
 1c1d8adbc955 Extracting [====================================>              ]  77.43MB/106.9MB
 1c1d8adbc955 Extracting [=====================================>             ]  80.22MB/106.9MB
 1c1d8adbc955 Extracting [=======================================>           ]  83.56MB/106.9MB
 1c1d8adbc955 Extracting [========================================>          ]   86.9MB/106.9MB
 1c1d8adbc955 Extracting [==========================================>        ]  90.24MB/106.9MB
 1c1d8adbc955 Extracting [==========================================>        ]  91.91MB/106.9MB
 1c1d8adbc955 Extracting [===========================================>       ]  93.59MB/106.9MB
 1c1d8adbc955 Extracting [============================================>      ]  95.81MB/106.9MB
 1c1d8adbc955 Extracting [=============================================>     ]  97.48MB/106.9MB
 1c1d8adbc955 Extracting [==============================================>    ]  99.16MB/106.9MB
 1c1d8adbc955 Extracting [===============================================>   ]  100.8MB/106.9MB
 1c1d8adbc955 Extracting [===============================================>   ]  101.9MB/106.9MB
 1c1d8adbc955 Extracting [================================================>  ]  103.1MB/106.9MB
 1c1d8adbc955 Extracting [================================================>  ]  104.2MB/106.9MB
 1c1d8adbc955 Extracting [=================================================> ]  105.3MB/106.9MB
 1c1d8adbc955 Extracting [=================================================> ]  106.4MB/106.9MB
 1c1d8adbc955 Extracting [==================================================>]  106.9MB/106.9MB
 1c1d8adbc955 Pull complete 
 9154ac3ec50f Extracting [==================================================>]  9.779kB/9.779kB
 9154ac3ec50f Extracting [==================================================>]  9.779kB/9.779kB
 9154ac3ec50f Pull complete 
 24dc5078fe3b Extracting [==================================================>]     128B/128B
 24dc5078fe3b Extracting [==================================================>]     128B/128B
 24dc5078fe3b Pull complete 
 68acc6218bcf Extracting [==================================================>]     170B/170B
 68acc6218bcf Extracting [==================================================>]     170B/170B
 68acc6218bcf Pull complete 
 532895c2736a Extracting [==================================================>]  5.414kB/5.414kB
 532895c2736a Extracting [==================================================>]  5.414kB/5.414kB
 532895c2736a Pull complete 
 a08535c970e7 Extracting [==================================================>]     185B/185B
 a08535c970e7 Extracting [==================================================>]     185B/185B
 a08535c970e7 Pull complete 
 postgres Pulled 
 Network ec2-user_default  Creating
 Network ec2-user_default  Created
 Container ec2-user-postgres-1  Creating
 Container ec2-user-java-maven-app-1  Creating
 Container ec2-user-java-maven-app-1  Created
 Container ec2-user-postgres-1  Created
 Container ec2-user-java-maven-app-1  Starting
 Container ec2-user-postgres-1  Starting
 Container ec2-user-postgres-1  Started
 Container ec2-user-java-maven-app-1  Started
[Pipeline] }
$ ssh-agent -k
unset SSH_AUTH_SOCK;
unset SSH_AGENT_PID;
echo Agent pid 158937 killed;
[ssh-agent] Stopped.
[Pipeline] // sshagent
[Pipeline] }
[Pipeline] // script
[Pipeline] }
[Pipeline] // withEnv
[Pipeline] }
[Pipeline] // stage
[Pipeline] }
[Pipeline] // withEnv
[Pipeline] }
[Pipeline] // withEnv
[Pipeline] }
[Pipeline] // withEnv
[Pipeline] }
[Pipeline] // node
[Pipeline] End of Pipeline
Finished: SUCCESS


[ec2-user@ip-172-31-89-89 ~]$ ls -l
total 4
-rw-r--r--. 1 ec2-user ec2-user 243 Jul 29 02:15 docker-compose.yaml

[ec2-user@ip-172-31-89-89 ~]$ docker ps
CONTAINER ID   IMAGE                      COMMAND                  CREATED         STATUS         PORTS                                                 NAMES
7bb3a83ca096   bradlmi/demo-app:aap-1.0   "docker-entrypoint.s…"   5 minutes ago   Up 5 minutes   3080/tcp, 0.0.0.0:8080->8080/tcp, :::8080->8080/tcp   ec2-user-java-maven-app-1
2bcdb7ff37b0   postgres:15                "docker-entrypoint.s…"   5 minutes ago   Up 5 minutes   0.0.0.0:5432->5432/tcp, :::5432->5432/tcp             ec2-user-postgres-1
[ec2-user@ip-172-31-89-89 ~]$


Adding shell script

deploying docker image to EC2...
[Pipeline] sshagent
[ssh-agent] Using credentials ec2-user
[ssh-agent] Looking for ssh-agent implementation...
[ssh-agent]   Exec ssh-agent (binary ssh-agent on a remote machine)
$ ssh-agent
SSH_AUTH_SOCK=/tmp/ssh-XXXXXX40V44B/agent.161428
SSH_AGENT_PID=161431
Running ssh-add (command line suppressed)
Identity added: /var/jenkins_home/workspace/ultibranch-pipeline_jenkins-jobs@tmp/private_key_4552880397932920458.key (/var/jenkins_home/workspace/ultibranch-pipeline_jenkins-jobs@tmp/private_key_4552880397932920458.key)
[ssh-agent] Started.
[Pipeline] {
[Pipeline] sh
+ scp server-cmds.sh ec2-user@44.203.54.164:/home/ec2-user
[Pipeline] sh
+ scp docker-compose.yaml ec2-user@44.203.54.164:/home/ec2-user
[Pipeline] sh
+ ssh -o StrictHostKeyChecking=no ec2-user@44.203.54.164 bash ./server-cmds.sh
time="2024-07-29T03:03:02Z" level=warning msg="/home/ec2-user/docker-compose.yaml: the attribute `version` is obsolete, it will be ignored, please remove it to avoid potential confusion"
 Network ec2-user_default  Creating
 Network ec2-user_default  Created
 Container ec2-user-postgres-1  Creating
 Container ec2-user-java-maven-app-1  Creating
 Container ec2-user-postgres-1  Created
 Container ec2-user-java-maven-app-1  Created
 Container ec2-user-java-maven-app-1  Starting
 Container ec2-user-postgres-1  Starting
 Container ec2-user-java-maven-app-1  Started
 Container ec2-user-postgres-1  Started
success
[Pipeline] }
$ ssh-agent -k
unset SSH_AUTH_SOCK;
unset SSH_AGENT_PID;
echo Agent pid 161431 killed;
[ssh-agent] Stopped.
[Pipeline] // sshagent
[Pipeline] }
[Pipeline] // script
[Pipeline] }
[Pipeline] // withEnv
[Pipeline] }
[Pipeline] // stage
[Pipeline] }
[Pipeline] // withEnv
[Pipeline] }
[Pipeline] // withEnv
[Pipeline] }
[Pipeline] // withEnv
[Pipeline] }
[Pipeline] // node
[Pipeline] End of Pipeline
Finished: SUCCESS


deploying docker image to EC2...
[Pipeline] sshagent
[ssh-agent] Using credentials ec2-user
[ssh-agent] Looking for ssh-agent implementation...
[ssh-agent]   Exec ssh-agent (binary ssh-agent on a remote machine)
$ ssh-agent
SSH_AUTH_SOCK=/tmp/ssh-XXXXXXBCLH5J/agent.164408
SSH_AGENT_PID=164411
Running ssh-add (command line suppressed)
Identity added: /var/jenkins_home/workspace/ultibranch-pipeline_jenkins-jobs@tmp/private_key_3537475193987545268.key (/var/jenkins_home/workspace/ultibranch-pipeline_jenkins-jobs@tmp/private_key_3537475193987545268.key)
[ssh-agent] Started.
[Pipeline] {
[Pipeline] sh
+ scp server-cmds.sh ec2-user@44.203.54.164:/home/ec2-user
[Pipeline] sh
+ scp docker-compose.yaml ec2-user@44.203.54.164:/home/ec2-user
[Pipeline] sh
+ ssh -o StrictHostKeyChecking=no ec2-user@44.203.54.164 bash ./server-cmds.sh bradlmi/demo-app:aap-2.0
time="2024-07-29T03:35:45Z" level=warning msg="/home/ec2-user/docker-compose.yaml: the attribute `version` is obsolete, it will be ignored, please remove it to avoid potential confusion"
 java-maven-app Pulling 
 b99da37831f3 Pulling fs layer 
 74ec8924afc6 Pulling fs layer 
 1ee9515eb0d0 Pulling fs layer 
 4f4fb700ef54 Pulling fs layer 
 4f4fb700ef54 Waiting 
 1ee9515eb0d0 Downloading [>                                                  ]  155.1kB/15.38MB
 b99da37831f3 Downloading [>                                                  ]  41.15kB/3.392MB
 1ee9515eb0d0 Downloading [==================================>                ]   10.6MB/15.38MB
 b99da37831f3 Verifying Checksum 
 b99da37831f3 Download complete 
 b99da37831f3 Extracting [>                                                  ]  65.54kB/3.392MB
 74ec8924afc6 Downloading [>                                                  ]  416.1kB/41.6MB
 1ee9515eb0d0 Verifying Checksum 
 1ee9515eb0d0 Download complete 
 74ec8924afc6 Downloading [===========>                                       ]  9.189MB/41.6MB
 b99da37831f3 Extracting [=======>                                           ]  524.3kB/3.392MB
 4f4fb700ef54 Downloading [==================================================>]      32B/32B
 4f4fb700ef54 Verifying Checksum 
 4f4fb700ef54 Download complete 
 b99da37831f3 Extracting [=============================>                     ]  2.032MB/3.392MB
 74ec8924afc6 Downloading [=====================>                             ]  17.97MB/41.6MB
 74ec8924afc6 Downloading [===============================>                   ]     26MB/41.6MB
 b99da37831f3 Extracting [========================================>          ]  2.753MB/3.392MB
 b99da37831f3 Extracting [==================================================>]  3.392MB/3.392MB
 b99da37831f3 Pull complete 
 74ec8924afc6 Downloading [========================================>          ]  33.94MB/41.6MB
 74ec8924afc6 Downloading [================================================>  ]  40.24MB/41.6MB
 74ec8924afc6 Verifying Checksum 
 74ec8924afc6 Download complete 
 74ec8924afc6 Extracting [>                                                  ]    426kB/41.6MB
 74ec8924afc6 Extracting [====>                                              ]  3.408MB/41.6MB
 74ec8924afc6 Extracting [=======>                                           ]   6.39MB/41.6MB
 74ec8924afc6 Extracting [==========>                                        ]  8.946MB/41.6MB
 74ec8924afc6 Extracting [================>                                  ]  13.63MB/41.6MB
 74ec8924afc6 Extracting [======================>                            ]  18.74MB/41.6MB
 74ec8924afc6 Extracting [=========================>                         ]  20.87MB/41.6MB
 74ec8924afc6 Extracting [============================>                      ]  23.43MB/41.6MB
 74ec8924afc6 Extracting [==============================>                    ]  25.56MB/41.6MB
 74ec8924afc6 Extracting [=================================>                 ]  28.11MB/41.6MB
 74ec8924afc6 Extracting [====================================>              ]  30.67MB/41.6MB
 74ec8924afc6 Extracting [=======================================>           ]  33.23MB/41.6MB
 74ec8924afc6 Extracting [===========================================>       ]  36.21MB/41.6MB
 74ec8924afc6 Extracting [===============================================>   ]  39.19MB/41.6MB
 74ec8924afc6 Extracting [==================================================>]   41.6MB/41.6MB
 74ec8924afc6 Pull complete 
 1ee9515eb0d0 Extracting [>                                                  ]  163.8kB/15.38MB
 1ee9515eb0d0 Extracting [===================>                               ]  6.062MB/15.38MB
 1ee9515eb0d0 Extracting [=======================================>           ]  12.12MB/15.38MB
 1ee9515eb0d0 Extracting [==================================================>]  15.38MB/15.38MB
 1ee9515eb0d0 Pull complete 
 4f4fb700ef54 Extracting [==================================================>]      32B/32B
 4f4fb700ef54 Extracting [==================================================>]      32B/32B
 4f4fb700ef54 Pull complete 
 java-maven-app Pulled 
 Container ec2-user-postgres-1  Creating
 Container ec2-user-java-maven-app-1  Creating
 Container ec2-user-java-maven-app-1  Created
 Container ec2-user-postgres-1  Created
 Container ec2-user-postgres-1  Starting
 Container ec2-user-java-maven-app-1  Starting
 Container ec2-user-java-maven-app-1  Started
 Container ec2-user-postgres-1  Started
success
[Pipeline] }
$ ssh-agent -k
unset SSH_AUTH_SOCK;
unset SSH_AGENT_PID;
echo Agent pid 164411 killed;
[ssh-agent] Stopped.
[Pipeline] // sshagent
[Pipeline] }
[Pipeline] // script
[Pipeline] }
[Pipeline] // withEnv
[Pipeline] }
[Pipeline] // stage
[Pipeline] }
[Pipeline] // withEnv
[Pipeline] }
[Pipeline] // withEnv
[Pipeline] }
[Pipeline] // withEnv
[Pipeline] }
[Pipeline] // node
[Pipeline] End of Pipeline
Finished: SUCCESS