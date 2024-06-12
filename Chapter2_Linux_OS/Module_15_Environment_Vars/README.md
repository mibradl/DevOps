# Chapter2
This module we learned where the OS stores all it's configurations?

# Name: Environment Variables

# Description: 
The OS stores all of it's configurations in a place call environment variable. Environment variables achieve this by using key value pairs. There is a name that is referenced and that name or variable has a value. These environment variable are available for the whole environment. By convention, names are defined in UPPER CASE.

User can change these environment variable values.
Variables are variable, which means they can be changes.

List all Variables

printenv

Print specific Environment Variables

printenv USER

echo $USER

Application Env Variable

Why would we create our own env vars?

App needs some credentials to connect to a database. It will need an Userid and password to connect to the DB, or perhaps a secret token. Can't hard-code this into the application because others will see it.

Set this data as environment variables on a server
Apps can read those environment varialbes
By creating the env vars, we make them available in the environment
All apps and processes can now access these environment variables

DB_USER=mysqluser
DB_PWD=secretpassword

Every programmimg language allows you to access environment variables

import mysql.connector

DB_USER=os.getenv('DB_USER')
DB_PWD=os.environ.get('DB_PWD')

my = mysql.connector.connect(
  host="https://prod-database:5432",
  user=DB_USER,
  password=DB_PWD
)

Note: this is a python examplet

Makes the application more flexible.

DEV, TESTING, PRODUCTION

Each DB will have its own credentials

Do not hardcode these connection values (DB URL, DB USER, DB Pwd)

DB_URL=https://dev-db:5432
DB_USER=mysql-dev-user
DB_PWD=dev_password

DB_URL=https://prod-db:5432
DB_USER=mysql-prod-user
DB_PWD=prod_password

No need to change application code

Env Vars are very important

Creating Env Vars

export DB_USERNAME=dbuser
export DB_PASSWORD=secretpwdvalue
export DB_NAME=mydb

printenv | grep DB

To use in batch script use:

echo $DB_NAME

To null the value

unset DB_NAME

Persisting Environment Variables

There is a file in every user home directory.

vim .bashrc

This is the shell specific configuration file. Per user-shell specific configuration files.

E.g. if you are using Bash, you can declare the variables in the ~/.bashrc file

Variables set in this file are loaded whenever a bash login is entered.

export DB_USER=dbuser
export DB_PWD=secretvl
export DB_NAME=mydb

PATH=$PATH:/home/michaelbradley

source ~/.bashrc 

Load the new env vars into the current shell sessions

To set Environment Variable system-wide

vim /etc/environment

PATH Environment Variable

List of directories to executable files, separated by :


Add custom command program

#!/bin/bash

echo "Welcome to DevOps Bootcamp $USER"



# Usage

printenv |less

TERM_PROGRAM=Apple_Terminal
SHELL=/bin/zsh
TERM=xterm-256color
TMPDIR=/var/folders/ng/dwh76f5111ncbrdb1q85gcw40000gn/T/
TERM_PROGRAM_VERSION=443
TERM_SESSION_ID=3EFF0239-D85D-459F-B40E-31BCA069F834
USER=michaelbradley
SSH_AUTH_SOCK=/private/tmp/com.apple.launchd.fb7USLHh6F/Listeners
PATH=/usr/local/bin:/usr/local/sbin:/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin
__CFBundleIdentifier=com.apple.Terminal
PWD=/Users/michaelbradley
XPC_FLAGS=0x0
XPC_SERVICE_NAME=0
SHLVL=1
HOME=/Users/michaelbradley
LOGNAME=michaelbradley
OLDPWD=/Users/michaelbradley/Downloads/Demo-projects/it-beginners-course
HOMEBREW_PREFIX=/usr/local

michaelbradley@Michaels-iMac-2 ~ % printenv USER
michaelbradley



    