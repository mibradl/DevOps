# Chapter4
This module we reviewed how to build the artifact.

# Name: Build an Artifact

# Description: 

How to build the artifact?

    This done using a build tool

    Specific to the programming language

        Java
        Maven
        Gradle

    Tools install dependencies

    Compile and compress your code

Build tools in Java and differences

    Java uses JAR or WAR file
    Maven  uses XML
    Gradle uses Groovy 

    Maven and Gradle have command line tool and commands to execute the tasks

    Perform Gradle Build

        gradle build

        [text](../../../../../new-project/java-app/build/libs/java-app-1.0-SNAPSHOT.jar)
        This jar file has all the code to execute the app

    Build Maven Project

        mvn install

        [text](../../../../../new-project/java-maven-app/target/java-maven-app-1.1.0-SNAPSHOT.jar)
        




# Usage

 gradle build

Deprecated Gradle features were used in this build, making it incompatible with Gradle 9.0.

You can use '--warning-mode all' to show the individual deprecation warnings and determine if they come from your own scripts or plugins.

For more on this, please refer to https://docs.gradle.org/8.8/userguide/command_line_interface.html#sec:command_line_warnings in the Gradle documentation.

BUILD SUCCESSFUL in 5s
5 actionable tasks: 5 up-to-date


michaelbradley@Michaels-iMac-2 java-maven-app % mvn install 
[INFO] Scanning for projects...
[INFO] 
[INFO] ---------------------< com.example:java-maven-app >---------------------
[INFO] Building java-maven-app 1.1.0-SNAPSHOT
[INFO]   from pom.xml
[INFO] --------------------------------[ jar ]---------------------------------
[INFO] 
[INFO] --- resources:3.3.1:resources (default-resources) @ java-maven-app ---
[WARNING] Using platform encoding (UTF-8 actually) to copy filtered resources, i.e. build is platform dependent!
[INFO] Copying 1 resource from src/main/resources to target/classes
[INFO] 
[INFO] --- compiler:3.11.0:compile (default-compile) @ java-maven-app ---
[INFO] Nothing to compile - all classes are up to date
[INFO] 
[INFO] --- resources:3.3.1:testResources (default-testResources) @ java-maven-app ---
[WARNING] Using platform encoding (UTF-8 actually) to copy filtered resources, i.e. build is platform dependent!
[INFO] skip non existing resourceDirectory /Users/michaelbradley/new-project/java-maven-app/src/test/resources
[INFO] 
[INFO] --- compiler:3.11.0:testCompile (default-testCompile) @ java-maven-app ---
[INFO] No sources to compile
[INFO] 
[INFO] --- surefire:3.2.5:test (default-test) @ java-maven-app ---
[INFO] No tests to run.
[INFO] 
[INFO] --- jar:3.4.1:jar (default-jar) @ java-maven-app ---
[INFO] 
[INFO] --- spring-boot:3.0.5:repackage (default) @ java-maven-app ---
[INFO] Replacing main artifact with repackaged archive
[INFO] 
[INFO] --- install:3.1.1:install (default-install) @ java-maven-app ---
[INFO] Installing /Users/michaelbradley/new-project/java-maven-app/pom.xml to /Users/michaelbradley/.m2/repository/com/example/java-maven-app/1.1.0-SNAPSHOT/java-maven-app-1.1.0-SNAPSHOT.pom
[INFO] Installing /Users/michaelbradley/new-project/java-maven-app/target/java-maven-app-1.1.0-SNAPSHOT.jar to /Users/michaelbradley/.m2/repository/com/example/java-maven-app/1.1.0-SNAPSHOT/java-maven-app-1.1.0-SNAPSHOT.jar
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  1.321 s
[INFO] Finished at: 2024-06-16T16:10:15-04:00
[INFO] ------------------------------------------------------------------------