# Chapter4
This module we looked how to install tools on MacOS.

# Name: Installation and Setup on MacOS

# Description: 

Tools we need to install

We use build tools to buils a Java application

    1. Install Java
    2. Install Maven
    3. Install Gradle, an alternative Java tool

These are build tools for building java applications


Next we will need to build a JavaScript application

    1. NPM
    2. Nodejs

Also, good to have a nice code editor to work with:

    1. Intellije - makes it java friendly
    2. Visual Studio Code

Will use step by step guides, based on operating system

The DevOps Engineer will need to be comfortable installing tools on different OS systems often.

Download Intellij - Code Editor

Install Homebrew - Package Manager

Install Java, Maven, Gradle, Node & NPM


Clone Git Projects & Open in Intellij

You can find the Git repository links in the lecture description

Three Projects

Setup Java-Gradle Project
Java-app: https:/gitlab/twn-devops-bootcamp/latest/04-build-tools/java-app

Setup Java-Maven Project
Java-maven-app: https:/gitlab/twn-devops-bootcamp/latest/04-build-tools/java-maven-app

Setup Node.js-npm Project
React-nodejs-app: https:/gitlab/twn-devops-bootcamp/latest/04-build-tools/react-nodejs-example

Install Intellij

    Google Download Intellij

    Install Homebrew Package Manager

    With "Homebrew" Package Manager we will then install all the other tools/software

    /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

    Install Git CLI

        https://git-scm.com/download/mac

        brew install git

    Clone & Open Java Maven App in Intellij

        git clone git@gitlab.com:twn-devops-bootcamp/latest/04-build-tools/java-maven-app.git

    Install Java & Configure JDK in Intellij

        Intellij is not able to find a JDK
        JDK is needed to develop java applications

    Intellij IDEA | Settings... settings for IDEA

    File | Project Structure... configurations for the project


Perform install of Java

    brew install openjdk@17

    Unable to locate a Java Runtime

    sudo ln -sfn /usr/local/opt/openjdk@17/libexec/openjdk.jdk /Library/Java/JavaVirtualMachines/openjdk.jdk

    Change the version in the path, if you installed a different path

    michaelbradley@Michaels-iMac-2 new-project % java --version
    openjdk 17.0.11 2024-04-16
    OpenJDK Runtime Environment Homebrew (build 17.0.11+0)
    OpenJDK 64-Bit Server VM Homebrew (build 17.0.11+0, mixed mode, sharing)

From Intellij

    Java app started
2024-06-16T11:12:17.171-04:00  INFO 56469 --- [           main] o.s.b.a.w.s.WelcomePageHandlerMapping    : Adding welcome page: class path resource [static/index.html]
2024-06-16T11:12:17.262-04:00  INFO 56469 --- [           main] o.s.b.w.embedded.tomcat.TomcatWebServer  : Tomcat started on port(s): 8080 (http) with context path ''
2024-06-16T11:12:17.269-04:00  INFO 56469 --- [           main] com.example.Application                  : Started Application in 1.305 seconds (process running for 1.585)

Install Maven

% brew install --ignore-dependencies maven 
==> Downloading https://formulae.brew.sh/api/formula.jws.json
########################################################################################################################################################################################################## 100.0%
Warning: `--ignore-dependencies` is an unsupported Homebrew developer option!
Adjust your PATH to put any preferred versions of applications earlier in the
PATH rather than using this unsupported option!

michaelbradley@Michaels-iMac-2 java-maven-app % /usr/libexec/java_home -V
Matching Java Virtual Machines (1):
    17.0.11 (x86_64) "Homebrew" - "OpenJDK 17.0.11" /usr/local/Cellar/openjdk@17/17.0.11/libexec/openjdk.jdk/Contents/Home
/usr/local/Cellar/openjdk@17/17.0.11/libexec/openjdk.jdk/Contents/Home
michaelbradley@Michaels-iMac-2 java-maven-app % export JAVA_HOME=`/usr/libexec/java_home -v 17`

mvn package

Building jar: /Users/michaelbradley/new-project/java-maven-app/target/java-maven-app-1.1.0-SNAPSHOT.jar
[INFO] 
[INFO] --- spring-boot:3.0.5:repackage (default) @ java-maven-app ---
[INFO] Replacing main artifact with repackaged archive
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  4.571 s
[INFO] Finished at: 2024-06-16T13:40:32-04:00
[INFO] ------------------------------------------------------------------------
michaelbradley@Michaels-iMac-2 java-maven-app % 


Install Gradle

    brew install gradle

Clone Gradle

git clone git@gitlab.com:twn-devops-bootcamp/latest/04-build-tools/java-app.git


Configure Gradle From Intellij

    From Intellij > Preference > Search bar - Type gradle

    Under Gradle Source
        Distribution: Local Installation
        Location: this field will automatically map to gradle


    Click Apply & Ok

From the Project Page

    Select Open
    java-app


Intellij will detect this to be Gradle app on the right side panel

    Build was automatically performed by Intellij

From the terminal 

    gradle build


Clone & Open Node App in Intellij

    Clone Node.js

    git clone git@github.com:techworld-with-nana/react-nodejs-example.git

    michaelbradley@Michaels-iMac-2 new-project % ls -l
total 8
-rw-r--r--   1 michaelbradley  staff   53 Jun 12 20:11 README.md
drwxr-xr-x  12 michaelbradley  staff  384 Jun 16 14:12 java-app
drwxr-xr-x   8 michaelbradley  staff  256 Jun 16 11:12 java-maven-app
drwxr-xr-x  10 michaelbradley  staff  320 Jun 16 14:34 node-app
drwxr-xr-x   8 michaelbradley  staff  256 Jun 16 15:31 react-nodejs-example


Need to install Nodejs & NPM

    brew install node

    michaelbradley@Michaels-iMac-2 new-project % node -v; npm -v
v22.3.0
10.8.1

michaelbradley@Michaels-iMac-2.local /Users/michaelbradley/new-project/react-nodejs-example/api [master]
% npm start

> react-nodejs-example@1.0.0 start
> node server.bundle.js

Server listening on the port::3080





# Usage

michaelbradley@Michaels-iMac-2 it-beginners-course % brew    
Example usage:
  brew search TEXT|/REGEX/
  brew info [FORMULA|CASK...]
  brew install FORMULA|CASK...
  brew update
  brew upgrade [FORMULA|CASK...]
  brew uninstall FORMULA|CASK...
  brew list [FORMULA|CASK...]

Troubleshooting:
  brew config
  brew doctor
  brew install --verbose --debug FORMULA|CASK

Contributing:
  brew create URL [--no-fetch]
  brew edit [FORMULA|CASK...]

Further help:
  brew commands
  brew help [COMMAND]
  man brew
  https://docs.brew.sh

  michaelbradley@Michaels-iMac-2 ~ % git          
usage: git [-v | --version] [-h | --help] [-C <path>] [-c <name>=<value>]
           [--exec-path[=<path>]] [--html-path] [--man-path] [--info-path]
           [-p | --paginate | -P | --no-pager] [--no-replace-objects] [--bare]
           [--git-dir=<path>] [--work-tree=<path>] [--namespace=<name>]
           [--config-env=<name>=<envvar>] <command> [<args>]

These are common Git commands used in various situations:

start a working area (see also: git help tutorial)
   clone     Clone a repository into a new directory
   init      Create an empty Git repository or reinitialize an existing one

work on the current change (see also: git help everyday)
   add       Add file contents to the index
   mv        Move or rename a file, a directory, or a symlink
   restore   Restore working tree files
   rm        Remove files from the working tree and from the index

examine the history and state (see also: git help revisions)
   bisect    Use binary search to find the commit that introduced a bug
   diff      Show changes between commits, commit and working tree, etc
   grep      Print lines matching a pattern
   log       Show commit logs
   show      Show various types of objects
   status    Show the working tree status

grow, mark and tweak your common history
   branch    List, create, or delete branches
   commit    Record changes to the repository
   merge     Join two or more development histories together
   rebase    Reapply commits on top of another base tip
   reset     Reset current HEAD to the specified state
   switch    Switch branches
   tag       Create, list, delete or verify a tag object signed with GPG

collaborate (see also: git help workflows)
   fetch     Download objects and refs from another repository
   pull      Fetch from and integrate with another repository or a local branch
   push      Update remote refs along with associated objects

'git help -a' and 'git help -g' list available subcommands and some
concept guides. See 'git help <command>' or 'git help <concept>'
to read about a specific subcommand or concept.
See 'git help git' for an overview of the system.

michaelbradley@Michaels-iMac-2 new-project % git clone git@gitlab.com:twn-devops-bootcamp/latest/04-build-tools/java-maven-app.git
Cloning into 'java-maven-app'...
remote: Enumerating objects: 98, done.
remote: Total 98 (delta 0), reused 0 (delta 0), pack-reused 98 (from 1)
Receiving objects: 100% (98/98), 10.66 KiB | 3.55 MiB/s, done.
Resolving deltas: 100% (44/44), done.

michaelbradley@Michaels-iMac-2 new-project % brew install openjdk@17

==> openjdk@17
For the system Java wrappers to find this JDK, symlink it with
  sudo ln -sfn /usr/local/opt/openjdk@17/libexec/openjdk.jdk /Library/Java/JavaVirtualMachines/openjdk-17.jdk

openjdk@17 is keg-only, which means it was not symlinked into /usr/local,
because this is an alternate version of another formula.

If you need to have openjdk@17 first in your PATH, run:
  echo 'export PATH="/usr/local/opt/openjdk@17/bin:$PATH"' >> ~/.zshrc

For compilers to find openjdk@17 you may need to set:
  export CPPFLAGS="-I/usr/local/opt/openjdk@17/include"

michaelbradley@Michaels-iMac-2 new-project % java --version
openjdk 17.0.11 2024-04-16
OpenJDK Runtime Environment Homebrew (build 17.0.11+0)
OpenJDK 64-Bit Server VM Homebrew (build 17.0.11+0, mixed mode, sharing)


% brew install --ignore-dependencies maven 
==> Downloading https://formulae.brew.sh/api/formula.jws.json
########################################################################################################################################################################################################## 100.0%
Warning: `--ignore-dependencies` is an unsupported Homebrew developer option!
Adjust your PATH to put any preferred versions of applications earlier in the
PATH rather than using this unsupported option!

==> Downloading https://formulae.brew.sh/api/cask.jws.json
########################################################################################################################################################################################################## 100.0%
==> Fetching maven
==> Downloading https://ghcr.io/v2/homebrew/core/maven/manifests/3.9.7
########################################################################################################################################################################################################## 100.0%
==> Downloading https://ghcr.io/v2/homebrew/core/maven/blobs/sha256:240f0b40a436c827d062016c3ffbf677f3fa77caca41b2ced9fcdedaf18cf39e
########################################################################################################################################################################################################## 100.0%
==> Pouring maven--3.9.7.monterey.bottle.tar.gz
🍺  /usr/local/Cellar/maven/3.9.7: 94 files, 10.7MB
==> Running `brew cleanup maven`...
Disable this behaviour by setting HOMEBREW_NO_INSTALL_CLEANUP.
Hide these hints with HOMEBREW_NO_ENV_HINTS (see `man brew`).
michaelbradley@Michaels-iMac-2.local /Users/michaelbradley/new-project/java-maven-app [master]
% 


michaelbradley@Michaels-iMac-2 java-maven-app % brew install gradle
==> Downloading https://formulae.brew.sh/api/formula.jws.json
########################################################################################################################################################################### 100.0%
==> Downloading https://formulae.brew.sh/api/cask.jws.json
########################################################################################################################################################################### 100.0%
==> Downloading https://ghcr.io/v2/homebrew/core/gradle/manifests/8.8-1
########################################################################################################################################################################### 100.0%
==> Fetching dependencies for gradle: openjdk@21
==> Downloading https://ghcr.io/v2/homebrew/core/openjdk/21/manifests/21.0.3
########################################################################################################################################################################### 100.0%
==> Fetching openjdk@21
==> Downloading https://ghcr.io/v2/homebrew/core/openjdk/21/blobs/sha256:880517d05b646ad165ff7a8d82e23ab242f1f72240fca01d8746b4e081d52941
########################################################################################################################################################################### 100.0%
==> Fetching gradle
==> Downloading https://ghcr.io/v2/homebrew/core/gradle/blobs/sha256:bd2537fef2d3504b1b195131abbfbf302a4d4bf2e86984249dd4e8dcc2c2d1a5
########################################################################################################################################################################### 100.0%
==> Installing dependencies for gradle: openjdk@21
==> Installing gradle dependency: openjdk@21
==> Downloading https://ghcr.io/v2/homebrew/core/openjdk/21/manifests/21.0.3
Already downloaded: /Users/michaelbradley/Library/Caches/Homebrew/downloads/29e4abe21d98b321be8d508106141bfb94549690d035266e9030cbc8875eb3cd--openjdk@21-21.0.3.bottle_manifest.json
==> Pouring openjdk@21--21.0.3.monterey.bottle.tar.gz
🍺  /usr/local/Cellar/openjdk@21/21.0.3: 601 files, 331.7MB
==> Installing gradle
==> Pouring gradle--8.8.monterey.bottle.1.tar.gz
🍺  /usr/local/Cellar/gradle/8.8: 21,579 files, 451.0MB
==> Running `brew cleanup gradle`...
Disable this behaviour by setting HOMEBREW_NO_INSTALL_CLEANUP.
Hide these hints with HOMEBREW_NO_ENV_HINTS (see `man brew`).
michaelbradley@Michaels-iMac-2 java-maven-app %


michaelbradley@Michaels-iMac-2 new-project % git clone git@gitlab.com:twn-devops-bootcamp/latest/04-build-tools/java-app.git
Cloning into 'java-app'...
remote: Enumerating objects: 172, done.
remote: Total 172 (delta 0), reused 0 (delta 0), pack-reused 172 (from 1)
Receiving objects: 100% (172/172), 15.54 MiB | 10.98 MiB/s, done.
Resolving deltas: 100% (45/45), done.
michaelbradley@Michaels-iMac-2 new-project % 



Starting Gradle Daemon...
Gradle Daemon started in 986 ms
Download https://repo.spring.io/snapshot/org/springframework/boot/org.springframework.boot.gradle.plugin/3.1.0-SNAPSHOT/org.springframework.boot.gradle.plugin-3.1.0-20230518.223113-300.pom, took 33 ms
Download https://repo.spring.io/snapshot/org/springframework/boot/spring-boot-gradle-plugin/3.1.0-SNAPSHOT/maven-metadata.xml, took 108 ms
Download https://repo.spring.io/snapshot/org/springframework/boot/spring-boot-gradle-plugin/3.1.0-SNAPSHOT/spring-boot-gradle-plugin-3.1.0-20230518.223113-300.pom, took 30 ms
Download https://repo.spring.io/snapshot/org/springframework/boot/spring-boot-gradle-plugin/3.1.0-SNAPSHOT/spring-boot-gradle-plugin-3.1.0-20230518.223113-300.module, took 32 ms
Download https://plugins.gradle.org/m2/io/spring/gradle/dependency-management-plugin/1.1.0/dependency-management-plugin-1.1.0.pom, took 288 ms
Download https://plugins.gradle.org/m2/io/spring/gradle/dependency-management-plugin/1.1.0/dependency-management-plugin-1.1.0.module, took 114 ms
Download https://repo.spring.io/snapshot/org/springframework/boot/spring-boot-buildpack-platform/3.1.0-SNAPSHOT/maven-metadata.xml, took 32 ms
Download https://repo.spring.io/snapshot/org/springframework/boot/spring-boot-buildpack-platform/3.1.0-SNAPSHOT/spring-boot-buildpack-platform-3.1.0-20230518.223113-300.pom, took 31 ms
Download https://repo.spring.io/snapshot/org/springframework/boot/spring-boot-buildpack-platform/3.1.0-SNAPSHOT/spring-boot-buildpack-platform-3.1.0-20230518.223113-300.module, took 33 ms
Download https://repo.spring.io/snapshot/org/springframework/boot/spring-boot-loader-tools/3.1.0-SNAPSHOT/maven-metadata.xml, took 109 ms
Download https://repo.spring.io/snapshot/org/springframework/boot/spring-boot-loader-tools/3.1.0-SNAPSHOT/spring-boot-loader-tools-3.1.0-20230518.223113-300.pom, took 31 ms
Download https://repo.spring.io/snapshot/org/springframework/boot/spring-boot-loader-tools/3.1.0-SNAPSHOT/spring-boot-loader-tools-3.1.0-20230518.223113-300.module, took 32 ms
Download https://plugins.gradle.org/m2/org/springframework/spring-core/6.0.9/spring-core-6.0.9.pom, took 188 ms
Download https://plugins.gradle.org/m2/org/springframework/spring-core/6.0.9/spring-core-6.0.9.module, took 491 ms
Download https://plugins.gradle.org/m2/org/apache/httpcomponents/client5/httpclient5/5.2.1/httpclient5-5.2.1.pom, took 138 ms
Download https://plugins.gradle.org/m2/org/apache/httpcomponents/client5/httpclient5-parent/5.2.1/httpclient5-parent-5.2.1.pom, took 87 ms
Download https://plugins.gradle.org/m2/org/apache/httpcomponents/httpcomponents-parent/13/httpcomponents-parent-13.pom, took 62 ms
Download https://plugins.gradle.org/m2/com/fasterxml/jackson/core/jackson-databind/2.14.2/jackson-databind-2.14.2.module, took 49 ms
Download https://plugins.gradle.org/m2/com/fasterxml/jackson/module/jackson-module-parameter-names/2.14.2/jackson-module-parameter-names-2.14.2.module, took 51 ms
Download https://plugins.gradle.org/m2/org/junit/junit-bom/5.9.1/junit-bom-5.9.1.module, took 119 ms
Download https://plugins.gradle.org/m2/org/springframework/spring-jcl/6.0.9/spring-jcl-6.0.9.pom, took 118 ms
Download https://plugins.gradle.org/m2/org/springframework/spring-jcl/6.0.9/spring-jcl-6.0.9.module, took 70 ms
Download https://plugins.gradle.org/m2/com/fasterxml/jackson/core/jackson-core/2.14.2/jackson-core-2.14.2.module, took 47 ms
Download https://plugins.gradle.org/m2/com/fasterxml/jackson/core/jackson-annotations/2.14.2/jackson-annotations-2.14.2.module, took 50 ms
Download https://plugins.gradle.org/m2/org/apache/httpcomponents/core5/httpcore5-h2/5.2/httpcore5-h2-5.2.pom, took 71 ms
Download https://plugins.gradle.org/m2/org/apache/httpcomponents/core5/httpcore5/5.2/httpcore5-5.2.pom, took 123 ms
Download https://plugins.gradle.org/m2/org/apache/httpcomponents/core5/httpcore5-parent/5.2/httpcore5-parent-5.2.pom, took 150 ms
Download https://repo.spring.io/snapshot/org/springframework/boot/spring-boot-gradle-plugin/3.1.0-SNAPSHOT/spring-boot-gradle-plugin-3.1.0-20230518.223113-300.jar, took 96 ms
Download https://plugins.gradle.org/m2/org/apache/httpcomponents/client5/httpclient5/5.2.1/httpclient5-5.2.1.jar, took 209 ms
Download https://plugins.gradle.org/m2/org/springframework/spring-jcl/6.0.9/spring-jcl-6.0.9.jar, took 202 ms
Download https://plugins.gradle.org/m2/io/spring/gradle/dependency-management-plugin/1.1.0/dependency-management-plugin-1.1.0.jar, took 272 ms
Download https://plugins.gradle.org/m2/org/apache/httpcomponents/core5/httpcore5-h2/5.2/httpcore5-h2-5.2.jar, took 267 ms
Download https://plugins.gradle.org/m2/org/apache/httpcomponents/core5/httpcore5/5.2/httpcore5-5.2.jar, took 338 ms
Download https://repo.spring.io/snapshot/org/springframework/boot/spring-boot-buildpack-platform/3.1.0-SNAPSHOT/spring-boot-buildpack-platform-3.1.0-20230518.223113-300.jar, took 437 ms
Download https://repo.spring.io/snapshot/org/springframework/boot/spring-boot-loader-tools/3.1.0-SNAPSHOT/spring-boot-loader-tools-3.1.0-20230518.223113-300.jar, took 443 ms
Download https://plugins.gradle.org/m2/org/springframework/spring-core/6.0.9/spring-core-6.0.9.jar, took 820 ms
> Task :prepareKotlinBuildScriptModel UP-TO-DATE
Download https://repo.spring.io/snapshot/org/springframework/boot/spring-boot-dependencies/3.1.0-SNAPSHOT/spring-boot-dependencies-3.1.0-20230518.223113-300.pom, took 96 ms
Download https://repo.maven.


% gradle build

Welcome to Gradle 8.8!

Here are the highlights of this release:
 - Running Gradle on Java 22
 - Configurable Gradle daemon JVM
 - Improved IDE performance for large projects

For more details see https://docs.gradle.org/8.8/release-notes.html

Starting a Gradle Daemon, 1 incompatible Daemon could not be reused, use --status for details

Deprecated Gradle features were used in this build, making it incompatible with Gradle 9.0.

You can use '--warning-mode all' to show the individual deprecation warnings and determine if they come from your own scripts or plugins.

For more on this, please refer to https://docs.gradle.org/8.8/userguide/command_line_interface.html#sec:command_line_warnings in the Gradle documentation.

BUILD SUCCESSFUL in 6s
5 actionable tasks: 4 executed, 1 up-to-date
michaelbradley@Michaels-iMac-2.local /Users/michaelbradley/new-project/java-app [master]


michaelbradley@Michaels-iMac-2 new-project % git clone git@github.com:techworld-with-nana/react-nodejs-example.git
Cloning into 'react-nodejs-example'...
remote: Enumerating objects: 5779, done.
remote: Counting objects: 100% (564/564), done.
remote: Compressing objects: 100% (491/491), done.
remote: Total 5779 (delta 78), reused 73 (delta 73), pack-reused 5215
Receiving objects: 100% (5779/5779), 6.11 MiB | 10.28 MiB/s, done.
Resolving deltas: 100% (1123/1123), done.


michaelbradley@Michaels-iMac-2 new-project % ls -l
total 8
-rw-r--r--   1 michaelbradley  staff   53 Jun 12 20:11 README.md
drwxr-xr-x  12 michaelbradley  staff  384 Jun 16 14:12 java-app
drwxr-xr-x   8 michaelbradley  staff  256 Jun 16 11:12 java-maven-app
drwxr-xr-x  10 michaelbradley  staff  320 Jun 16 14:34 node-app
drwxr-xr-x   8 michaelbradley  staff  256 Jun 16 15:31 react-nodejs-example
michaelbradley@Michaels-iMac-2 new-project % 



michaelbradley@Michaels-iMac-2 new-project % brew install node
==> Downloading https://formulae.brew.sh/api/formula.jws.json
########################################################################################################################################################################### 100.0%
==> Downloading https://formulae.brew.sh/api/cask.jws.json
########################################################################################################################################################################### 100.0%
node 22.2.0 is already installed but outdated (so it will be upgraded).
==> Downloading https://ghcr.io/v2/homebrew/core/node/manifests/22.3.0
########################################################################################################################################################################### 100.0%
==> Fetching dependencies for node: c-ares
==> Downloading https://ghcr.io/v2/homebrew/core/c-ares/manifests/1.30.0
########################################################################################################################################################################### 100.0%
==> Fetching c-ares
==> Downloading https://ghcr.io/v2/homebrew/core/c-ares/blobs/sha256:87b091b809c673b125927e4e318d4e6749bb0e558269ab5d1dfcb2385c50bbdf
########################################################################################################################################################################### 100.0%
==> Fetching node
==> Downloading https://ghcr.io/v2/homebrew/core/node/blobs/sha256:2de279a1bc9571ad22f46763f0495e95bbe22d59d62d6e30734fe3aafccfbd45
########################################################################################################################################################################### 100.0%
==> Upgrading node
  22.2.0 -> 22.3.0 
==> Installing dependencies for node: c-ares
==> Installing node dependency: c-ares
==> Downloading https://ghcr.io/v2/homebrew/core/c-ares/manifests/1.30.0
Already downloaded: /Users/michaelbradley/Library/Caches/Homebrew/downloads/ca777c03e46915fc70179dab0fdae08a6ea3efd093e5d4a7cc86e693cf5b85e5--c-ares-1.30.0.bottle_manifest.json
==> Pouring c-ares--1.30.0.monterey.bottle.tar.gz
🍺  /usr/local/Cellar/c-ares/1.30.0: 168 files, 888.1KB
==> Installing node
==> Pouring node--22.3.0.monterey.bottle.tar.gz
🍺  /usr/local/Cellar/node/22.3.0: 2,068 files, 76.5MB
==> Running `brew cleanup node`...
Disable this behaviour by setting HOMEBREW_NO_INSTALL_CLEANUP.
Hide these hints with HOMEBREW_NO_ENV_HINTS (see `man brew`).
Removing: /usr/local/Cellar/node/22.2.0... (2,070 files, 76.4MB)
Removing: /Users/michaelbradley/Library/Caches/Homebrew/node_bottle_manifest--22.2.0... (17.9KB)
==> Upgrading 1 dependent of upgraded formulae:
Disable this behaviour by setting HOMEBREW_NO_INSTALLED_DEPENDENTS_CHECK.
Hide these hints with HOMEBREW_NO_ENV_HINTS (see `man brew`).
mongosh 2.2.6 -> 2.2.9
==> Downloading https://ghcr.io/v2/homebrew/core/mongosh/manifests/2.2.9
########################################################################################################################################################################### 100.0%
==> Checking for dependents of upgraded formulae...
==> No broken dependents found!
michaelbradley@Michaels-iMac-2 new-project % 



michaelbradley@Michaels-iMac-2 new-project % node -v; npm -v
v22.3.0
10.8.1


michaelbradley@Michaels-iMac-2.local /Users/michaelbradley/new-project/react-nodejs-example/api [master]
% npm start

> react-nodejs-example@1.0.0 start
> node server.bundle.js

Server listening on the port::3080
