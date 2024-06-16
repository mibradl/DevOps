# Chapter4
This module we explored how to install tools on Windows.

# Name: Installation Help for Windows User Part 2

# Description: 

Download Maven & Add to "PATH"

Google or Bing Maven Download

https://maven.apache.org/download.cgi?.

The latest maven versions are located under Files, as a binary tar file

we are using the 3.9.1-bin.zip file

In the downloads folder double click on the zip and extract it

The file will be extracted to:

    C:\Users\nana\Downloads\apache-maven-3.9.1-bin
    check - show extracted files when completed

    click Extract

    bin - executables located hear - mvn
    boot
    conf
    lib
    
    README
    NOTICE
    LICENSE


To execute the mvn command the location has to be set in the environment variable section, under System variables setion; listed in the PATH variable.

    Edit the System variables section.
    Click New to create a new PATH
    Paste in the explicit path: C:\Users\nana\Downloads\apache-maven-3.9.1-bin\apache-maven
    Click ok
    Open a new terminal using cmd command
    Restart intellij and begin writing your maven application

Type in mvn to test

At this location: C:\Users\nanaj\IdeaProjects\java-maven-app>  mvn package

This will build the application - should see a build success

With this you will be ready follow the other video to perform builds and run them.

Close Maven project


Setup Java Gradle Project in Intellij

    Using Google or Bing search for download gradle

    https://gradle.org/install/

    Installing manually

        Select Binary-only

    From the Downloads directory click Extract to de-compress the file

    Select the C: 

    Right Click and create new folder

    And based on installation folder, type in Gradle

    Next, copy the contents of Gradle extract from the *\bin folder and copy it to the Gradle folder.

    In the Advanced system setting we want to add the explicit PATH the system env variables

        Select New

        C:\Gradle\gradle-8.1\bin

        Must configure Intellij to use Gradle by clicking on Customize
        All Settings
        In the Top Left Search bar type: gradle
        Under the Gradle Source, change the Distribution to Local installation
        For Location: C:\Gradle\gradle-8.1  Intellij will locate this automatically

        Type Apply and Ok

        Click Project on the left

        Get from VCS
        Git
        https://gitlab.com/tgwn-devops-bootcamp/2023/04-build-tools/java-app/

        Intellij will recognize it's a Gradle project and start installing all the dependencies; the Plugins and the libraries and build the application.

        Now we can run the JavaApp to ensure it executes.


        Next, we can build the gradle app.

            type: gradle build

            BUILD SUCCESSFUL in 38s



        2 different environments

        1. Because Intellij was configured with the Maven application it knows were everything is and therefore can run the application by just clicking the green arrow.

        2. By clicking on the terminal we are in the operating system environment and therefore can use the CLI to run the application based on setting up the PATH variable using the: mvn package command

        These environments must be setup separately.


Setup Node project in Intellij

        Select Project 
            Get from VCS
            Git
            URL: https://github.com/techworld-nana/react-nodejs-example
            Directory: C:\User\nana\IdeaProjects\react-nodejs-example

            Click Clone to copy it to your local directory

Javascript projects are much simpler than Java projects, so much faster to setup.

        Requires NPM to install all the dependencies for Node.js

        From Google or Bing type in Node download

            Click Download Node.js

            This will list installer for OS

                Select Windows Installer(.msi), 64-bit - will install and configure binary automatically

                In the downloads folder: node-v18.16.0-x64

                Installs Nodejs and NPM, uing the install wizard

                    Node.js
                    NPM package manager
                    add to PATH

            Will require a restart of intellij

                   node -v
                   npm -v

                   Located in Program files

            Since the package.json file is located in the api directory we have to move to that location in the command line to perform the npm install

                This will install all the dependencies for this application

                Will receive notice Run npm install -g npm@9.6.4 to update!

            Next, perform node server to start node.js
            allow access

                Server listening on the port::3080

                











# Usage

https://gitlab.com/twn-devops-bootcamp/latest/04-build-tools/java-maven-app