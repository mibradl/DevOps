# Chapter2
This module covered the introduction to Package Manager

# Name: Intro to Package Manager


# Description: 

  In this module we learned making use of the Package Managers to install software on Linux.

  What is a Package Manager?
  What is a Software Package? Compressed Archive, containing all required files.
  Dependencies: Other files needed to run the package
  Package Manager handles this and determines where files go.
  If uninstalled the PM knows where to locate files.
  Easy upgrade of software

  Package Manager is already included in every Linux distribution
  In Ubuntu, you have AdvancedPackagedTool package manager available
  Difference between APT and APT-GET

  APT is more user-friendly and offers search command 

  APT-GET can perform the same, but is more specific.

  Package Manager fetches the packages from its storage location, called a repository.

  apt update - is usedd to update package index

  pulls the latest changes from the APT repo
  the apt package index is essentally a database
  holds records of of available packages in the repo

  stored locally in /etc/apt

  Alternative ways to install software, due to verification process on Ubuntu it may not be available
  
  Ubuntu Software on google.  In the search type in Intellij

  Snap - is a software packaging and deployment system for OS using Linux Kernal

  Snap is bundled with app and all its dependencies

  Also used to install IDEs such as VisioStudio

  Difference between APT and SNAP

  APT dependencies are separate, only for specific Linux distributions (package type .deb), manual updates, smaller in size because of the separate dependencies, shared

  SNAP is self-contained and includes dependencies in the package, universal packages (type .snap),  auto updates lager in size because of the bundled dependencies, storage not shared so not as efficient

  APT is preferred

  Add Repository - for new packages which do not have an official repository

  Add the Docker repository to APT sources

  sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linu/ubuntu..."

  Repoistory will be added to /etc/apt/sources.list

  The next apt install package name will look into this new added repository

  PPA - Personal Package Archive provided by the community
  Anybody can create this PPA to distribute software
  Not verified by Lunix

  Other distros grouped based on same source code
  Distros in the same category use same package manager

  Main Categories

  Debian based - Ubuntu, Debian, Mint; use APT, APT-GET
  Red Hat based - RHEL, Centos, Fedora; use YUM or DNF




# Technologies Used:

  Package Manager for Ubuntu - APT, APT-GET, SNAP

# Usage

root@web-server:~# apt
apt 2.0.10 (amd64)
Usage: apt [options] command

apt is a commandline package manager and provides commands for
searching and managing as well as querying information about packages.
It provides the same functionality as the specialized APT tools,
like apt-get and apt-cache, but enables options more suitable for
interactive use by default.

Most used commands:
  list - list packages based on package names
  search - search in package descriptions
  show - show package details
  install - install packages
  reinstall - reinstall packages
  remove - remove packages
  autoremove - Remove automatically all unused packages
  update - update list of available packages
  upgrade - upgrade the system by installing/upgrading packages
  full-upgrade - upgrade the system by removing/installing/upgrading packages
  edit-sources - edit the source information file
  satisfy - satisfy dependency strings

  root@web-server:~# apt search openjdk
Sorting... Done
Full Text Search... Done
crypto-policies/focal 20190816git-1 all
  unify the crypto policies used by different applications and libraries

default-jdk/focal 2:1.11-72 amd64
  Standard Java or Java compatible Development Kit

default-jdk-doc/focal 2:1.11-72 amd64
  Standard Java or Java compatible Development Kit (documentation)

default-jdk-headless/focal 2:1.11-72 amd64
  Standard Java or Java compatible Development Kit (headless)

default-jre/focal 2:1.11-72 amd64
  Standard Java or Java compatible Runtime

default-jre-headless/focal 2:1.11-72 amd64
  Standard Java or Java compatible Runtime (headless)

java-package/focal 0.62 all
  Utility for creating Java Debian packages

jtreg/focal-updates,focal-security 5.1-b01-2~20.04 all
  Regression Test Harness for the OpenJDK platform

jtreg6/focal-updates,focal-security 6.1+2-1ubuntu1~20.04 all
  Regression Test Harness for the OpenJDK platform

jtreg7/focal-updates,focal-security 7.3.1+1~us2-0ubuntu1~20.04.1 all
  Regression Test Harness for the OpenJDK platform

libasmtools-java/focal-updates,focal-security 7.0-b09-2ubuntu1~20.04 all
  OpenJDK AsmTools

root@web-server:~# java

Command 'java' not found, but can be installed with:

apt install default-jre              # version 2:1.11-72, or
apt install openjdk-11-jre-headless  # version 11.0.20.1+1-0ubuntu1~20.04
apt install openjdk-16-jre-headless  # version 16.0.1+9-1~20.04
apt install openjdk-17-jre-headless  # version 17.0.8.1+1~us1-0ubuntu1~20.04
apt install openjdk-8-jre-headless   # version 8u382-ga-1~20.04.1
apt install openjdk-13-jre-headless  # version 13.0.7+5-0ubuntu1~20.04

root@web-server:~# apt install openjdk-17-jre-headless
Reading package lists... Done
Building dependency tree       
Reading state information... Done
The following additional packages will be installed:
  ca-certificates-java fontconfig-config fonts-dejavu-core java-common libavahi-client3 libavahi-common-data libavahi-common3 libcups2 libfontconfig1 libgraphite2-3 libharfbuzz0b
  libjpeg-turbo8 libjpeg8 liblcms2-2 libpcsclite1
Suggested packages:
  default-jre cups-common liblcms2-utils pcscd libnss-mdns fonts-dejavu-extra fonts-ipafont-gothic fonts-ipafont-mincho fonts-wqy-microhei | fonts-wqy-zenhei fonts-indic
The following NEW packages will be installed:
  ca-certificates-java fontconfig-config fonts-dejavu-core java-common libavahi-client3 libavahi-common-data libavahi-common3 libcups2 libfontconfig1 libgraphite2-3 libharfbuzz0b
  libjpeg-turbo8 libjpeg8 liblcms2-2 libpcsclite1 openjdk-17-jre-headless
0 upgraded, 16 newly installed, 0 to remove and 65 not upgraded.
Need to get 45.9 MB of archives.
After this operation, 200 MB of additional disk space will be used.
Do you want to continue? [Y/n] Y

root@web-server:~# java --version
openjdk 17.0.10 2024-01-16
OpenJDK Runtime Environment (build 17.0.10+7-Ubuntu-120.04.1)
OpenJDK 64-Bit Server VM (build 17.0.10+7-Ubuntu-120.04.1, mixed mode, sharing)

root@web-server:~# apt remove openjdk-11-jre-headless
Reading package lists... Done
Building dependency tree       
Reading state information... Done
The following packages were automatically installed and are no longer required:
  fontconfig-config fonts-dejavu-core java-common libavahi-client3 libavahi-common-data libavahi-common3 libcups2 libfontconfig1 libgraphite2-3 libharfbuzz0b libjpeg-turbo8 libjpeg8
  liblcms2-2 libpcsclite1
Use 'apt autoremove' to remove them.
The following packages will be REMOVED:

root@web-server:~# snap
The snap command lets you install, configure, refresh and remove snaps.
Snaps are packages that work across many different Linux distributions,
enabling secure delivery and operation of the latest apps and utilities.

Usage: snap <command> [<options>...]

Commonly used commands can be classified as follows:

           Basics: find, info, install, remove, list
          ...more: refresh, revert, switch, disable, enable, create-cohort
          History: changes, tasks, abort, watch
          Daemons: services, start, stop, restart, logs
      Permissions: connections, interface, connect, disconnect
    Configuration: get, set, unset, wait
      App Aliases: alias, aliases, unalias, prefer
          Account: login, logout, whoami
        Snapshots: saved, save, check-snapshot, restore, forget
           Device: model, remodel, reboot, recovery
     Quota Groups: set-quota, remove-quota, quotas, quota
  Validation Sets: validate
        ... Other: warnings, okay, known, ack, version
      Development: validate

For more information about a command, run 'snap help <command>'.
For a short summary of all commands, run 'snap help --all'.
