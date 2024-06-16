# Chapter4
This module we explored how to install tools on Windows.

# Name: Installation Help for Windows User

# Description: 

Lecture Overview

How Windows File System differs?

How Windows CLI differs

Installation Help for tools we need in the Build Tools module

Window File System

There multiple roots, called drives:

    A:
    B:
    C:
    D:

    With a drive, same hierarchy of folders and files

    Not everything is a file in Windows!
    E.g. devices are not treated as files

    Windows uses backslashes for paths C:\Windows\System32\

    App files aren't split into different folders when installed

    Most are installed in C: drive, Program directory


Windows CLI

Different Commands on Windows, because of different Shell programs

E.g. PowerShell instead of Bash

Windows         Linux           Description

dir             ls -l           Directory listing
ren             mv              Rename a file
copy            cp              Copying a file
move            mv              Moving a file
cls             clear           Clear Screen
del             rm              Delete file
chdir           pwd             Returns your current directory location
time            date            Displays the time
cd              cd              Change the current directory
md              mkdir           To create a new directory folder
echo            echo            To print something on the screen


As a consequence, Window Scripts have a different syntax

Windows is more optimized for a GUI

Unix-like CLI commands are available e.g. WLS


Windows Installation Help

Windows & Setup on Windows OS

    Install Tools & Setup Projects for Build Tools Module

    Technologies                Projects

    Java                        
    Maven                       java maven application
    Gradle                      java gradle application
    Node.js                     nodejs application
    NPM


Setup Intellij - Code Editor    #Integrated Development Environment

Install Java and Maven

Setup Java-Maven Project in IntelliJ

Install Gradle and setup Java-Gradle Project in IntelliJ

Install Nodejs and npm and run in Intellij

Note: reviewed windows installation using intellij and watched the the java-maven-app cloned from the gitlab remote repo and Window local machine. Also, observed the file structure with SRC, the pom.xml, with the additional files Jenkinsfile, scriptgroovy, as well as the .gitignore. Additionally, noticed the External Libraries. Also, java-maven-app contains lifecycle, Plugins and Dependencies folders.

Will perform this same installation on a MacOs machine using Linux VM.

Install Java JDK (Java Development Kit)

    Setup Java SDK
    In File\Project Structure - shows No SDK is present on the machine
    (Software Development Kit)

You need Java Development Kit to Setup your java application
And SDK to write using java code

Thus, unable to resolve classes, etc

Intellij allows you to download the JDK

Will specify specific version so its compatible with Gradle

After indexing is complete the syntax will correct itself because the JDK is installed

Use Run tab to start the application

Use the red square to terminate the application

Intellij has a terminal interface for CLI

Under File\Settings\Terminal Under Applications Settings Shell Path click the dropdown and select C:\Windows\system32\cmd.exe and apply. 

    Restart the terminal
    Type in commands
    Can't find java because it's located in this path below

C:\Users\nanaj\.jdks\corretto-17.0.6\bin\java --version  

To set the path in the search bar to create environment variables, type in advanced system settings and it will take you to System Properties.

    At the the bottom of the screen you will see Environment Variables

    The Env Vars are split into User variables and system variables

        Users Vars are dedicated to my user id and other users have no visibility

        System Vars are system-wide for all users

    Under System Vars we will select the Path variable and select edit add a new path:

        C:\Users\nanaj\.jdks\corretto-17.0.6\bin

    This will make it available to all users on the system.

    Open a new terminal and type in java  - notice that java is now available.

    echo %PATH% will print out the Path variable value

    To pick up this new value will need to restart intellij.

Next we will install Maven.  Maven is also written in java. So, the pre-requisites are:

        JDK
        SDK

        These have already been installed.

        Another pre-req is having Java in the "Path". Maven uses the same path, so this is done.

        Another environment variable names "JAVA_HOME" must be set before we install Maven.

        Using the same procedure, go to the search bar and type in "advanced setting" and click environment variables and type new and JAVA_HOME for Variable Name and the Value as java path, minus the \bin:

        C:\Users\nanaj\.jdks\corretto-17.0.6

        echo %JAVA_HOME% will show the value set for this variable name

        This requires a restart of the entire session.

            This time so we don't have to restart intellij, we will type the cli command to start a new command prompt: cmd and type the command "echo %JAVA_HOME%"

    

# Usage

https://gitlab.com/twn-devops-bootcamp/latest/04-build-tools/java-maven-app