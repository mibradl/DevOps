# Chapter5
This module we cover how to deploy the application to the Droplet.

# Name: Deploy Application on Droplet

# Description: 

Build Jar File

Copy to remote Server

Run Application on the remote server

Build App with Gradle

    clone git@gitlab.com:twn-devops-bootcamp/latest/05-cloud/java-react-example.git

    Copy jar file to remote server

       scp build/libs/java-react-example.jar root@164.90.128.228:/root

       java -jar java-react-example.jar

        Tomcat initialized with port(s): 7071 (http
        Started Application in 3.996 seconds (JVM running for 4.77)


    http://164.90.128.228:7071/

            Project: java-react-example

            Countries
            Albania
            Andorra
            Armenia
            Austria
            Azerbaijan
            Belarus
            Belgium
            Bosnia and Herzegovina
            Bulgaria
            Croatia
            Cyprus
            Czech Republic
            Denmark
            Estonia
            Finland
            France
            Georgia
            Germany
            Greece
            Hungary
            Iceland
            Ireland
            Italy
            Kazakhstan
            Kosovo
            Latvia
            Liechtenstein
            Lithuania
            Luxembourg
            Macedonia
            Malta
            Moldova
            Monaco

Run in detached mode:

    root@ubuntu-s-2vcpu-4gb-amd-nyc3-01:~# java -jar java-react-example.jar &
    [1] 4142
    root@ubuntu-s-2vcpu-4gb-amd-nyc3-01:~# 
    .   ____          _            __ _ _
    /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
    ( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
    \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
    '  |____| .__|_| |_|_| |_\__, | / / / /
    =========|_|==============|___/=/_/_/_/
    :: Spring Boot ::               (v2.7.11)


    root@ubuntu-s-2vcpu-4gb-amd-nyc3-01:~# ps aux |grep java
    root        4142  6.5  4.5 3414704 181696 pts/0  Sl   19:09   0:07 java -jar java-react-example.jar   #pid 4142
    root        4175  0.0  0.0   7076  2048 pts/0    S+   19:11   0:00 grep --color=auto java
    root@ubuntu-s-2vcpu-4gb-amd-nyc3-01:~# 


    root@ubuntu-s-2vcpu-4gb-amd-nyc3-01:~# netstat -ltpn
    Active Internet connections (only servers)
    Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name    
    tcp        0      0 127.0.0.54:53           0.0.0.0:*               LISTEN      694/systemd-resolve 
    tcp        0      0 127.0.0.53:53           0.0.0.0:*               LISTEN      694/systemd-resolve 
    tcp6       0      0 :::7071                 :::*                    LISTEN      4142/java            #pid 4142
    tcp6       0      0 :::22                   :::*                    LISTEN      1/init


# Usage

michaelbradley@Michaels-iMac-2 java-react-example % scp build/libs/java-react-example.jar root@164.90.128.228:/root
java-react-example.jar                                                                                                                                                      100%   17MB  10.5MB/s   00:01    
michaelbradley@Michaels-iMac-2 java-react-example %

-rw-r--r-- 1 root root 17923596 Jun 17 18:58 java-react-example.jar
root@ubuntu-s-2vcpu-4gb-amd-nyc3-01:~#

root@ubuntu-s-2vcpu-4gb-amd-nyc3-01:~# java -jar java-react-example.jar 

  .   ____          _            __ _ _
 /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
 \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
  '  |____| .__|_| |_|_| |_\__, | / / / /
 =========|_|==============|___/=/_/_/_/
 :: Spring Boot ::               (v2.7.11)

2024-06-17 19:00:37.671  INFO 4079 --- [           main] com.coditorium.sandbox.Application       : Starting Application using Java 1.8.0_412 on ubuntu-s-2vcpu-4gb-amd-nyc3-01 with PID 4079 (/root/java-react-example.jar started by root in /root)
2024-06-17 19:00:37.676  INFO 4079 --- [           main] com.coditorium.sandbox.Application       : No active profile set, falling back to 1 default profile: "default"
2024-06-17 19:00:37.813  INFO 4079 --- [           main] .e.DevToolsPropertyDefaultsPostProcessor : For additional web related logging consider setting the 'logging.level.web' property to 'DEBUG'
2024-06-17 19:00:39.740  INFO 4079 --- [           main] o.s.b.w.embedded.tomcat.TomcatWebServer  : Tomcat initialized with port(s): 7071 (http)
2024-06-17 19:00:39.762  INFO 4079 --- [           main] o.apache.catalina.core.StandardService   : Starting service [Tomcat]
2024-06-17 19:00:39.763  INFO 4079 --- [           main] org.apache.catalina.core.StandardEngine  : Starting Servlet engine: [Apache Tomcat/9.0.74]
2024-06-17 19:00:39.905  INFO 4079 --- [           main] o.a.c.c.C.[Tomcat].[localhost].[/]       : Initializing Spring embedded WebApplicationContext
2024-06-17 19:00:39.906  INFO 4079 --- [           main] w.s.c.ServletWebServerApplicationContext : Root WebApplicationContext: initialization completed in 2090 ms
2024-06-17 19:00:40.334  INFO 4079 --- [           main] o.s.b.a.w.s.WelcomePageHandlerMapping    : Adding welcome page: class path resource [static/index.html]
2024-06-17 19:00:40.576  INFO 4079 --- [           main] o.s.b.w.embedded.tomcat.TomcatWebServer  : Tomcat started on port(s): 7071 (http) with context path ''
2024-06-17 19:00:40.592  INFO 4079 --- [           main] com.coditorium.sandbox.Application       : çrunning for 4.77)

