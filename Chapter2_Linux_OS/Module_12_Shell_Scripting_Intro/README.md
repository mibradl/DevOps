# Chapter2
This module discusses the use of input and output

# Name: Introduction to Shell Scripting


# Description: 

In this module we learned what a Shell is, the different cells and the concepts and syntax of bash scripting.

Variables
Condition Statementss
Passing Arguments to Script
Read User Input
Loops
Functions
  
All these commands can be run as a script

adduser tim
groupadd devops
mkdir project
touch file.txt
chmod 750 /path
sudo apt docker
docker run

These can be written to a file and executed. Also the file is movable. This is called a shell script. *.sh

Shell vs sh vs Bash

The program that interprets and executes the various commands that we type in the terminal. Translates our command so that the OS Kernal can understand it

Diffent Shell Implementations

sh (bourne shell) /bin/sh
  used to be the default shell

bash (borne again shell) /bin/bash
improved version of sh
default shell program for most UNIX like systems

Shebang - Why is it called "shebang"?

The first 2 characters: "#!"

# = in musical notation, also called "sharp"
! = also called bang

All shell scripts have the .sh file extension

How does OS know which shell to use? We tell the OS!

#!/bin/sh - bourne shell
#!/bin/bash - bourne again shell
#!/bin/zsh

Write and execute script

#!/bin/bash #declaration and location of bash

echo "Setup and configure server"
~                                    

-rw-r--r--   1 michaelbradley  staff    81 Jun  7 11:46 setup.sh
michaelbradley@Michaels-iMac-2 ~ % chmod 744 setup.sh 

michaelbradley@Michaels-iMac-2 ~ % ./setup.sh        
Setup and configure server









# Usage

michaelbradley@Michaels-iMac-2 ~ % ls /bin |grep bash
bash

#!/bin/bash #first line of the script

#!/bin/bash

echo "Setup and configure server"

-rw-r--r--   1 michaelbradley  staff    81 Jun  7 11:46 setup.sh
michaelbradley@Michaels-iMac-2 ~ % chmod 744 setup.sh 

michaelbradley@Michaels-iMac-2 ~ % ./setup.sh        
Setup and configure server

