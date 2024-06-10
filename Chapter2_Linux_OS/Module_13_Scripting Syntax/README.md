# Chapter2
This module discusses the use of syntax and concepts.

# Name: Concepts and syntax


# Description: 

In this module we continued to expand on the topic of scripting and review concepts and syntax.

VARAIABLES - used to store data that can be referenced later. Similar to variables in general programming languages.

#!/bin/bash #declaration and location of bash

echo "Setup and configure server"

file_name=mongo.yaml
config_file=$(cat mongo.yaml)

echo "using file $file_name to view configuration"
echo "contains contents of configuration: $config_file"

CONDITION STATEMENTS - allow you to alter the control flow of the program. E.g. Execute a command when a certain condition is true.

if-else statements

if [ condition ]
then
  statement
else
  statement
fi

File test operators

-b checks if file is a block special file
-c checks if file is a character special file
-d checks if file is a directory
-f checks if file is an ordinary 
-g checks if file  has its set group id bit set
-k checks if file has its sticky bit
-p checks if file is a named pipe
-t checks if file descriptor is open
-u checks if file has it set user bit set
-r checks if file is readable
-w checks if file is writable
-x checks if file is executable
-s checks if file has a size greater than 0
-e check if file  exist


Relational operators

-eq checks if the value of two operands are equal
-ne checks if the value of two operands are equal or not
-gt checks if the value of the left operand is greater than the right operand
-lt checks if the value of the left operand is less than the right operand
-ge checks if the value of the left operand is greater than or equal the right operand
-le checks if the value of the left operand is less than or equal the right operand

String Operators

== checks if value of two operands (string) are equal
!= checks if value of two operands (string) are not equal
-z checks if the give string operand size is zero
-n  checks if the give string operand size is not zero
str checks if str is not the empty string



Passing Arguments to Script
Read User Input
Loops
Functions

Passing Argument to Script

Arguments passed to script are processed in the same order in which they're sent

The indexing of arguments start at 1

user_group=$1

Reuse the same script with different parameters


Read User Input

#!/bin/bash

echo "Reading user input"

read -p "Please enter your password: " user_pwd

echo "thanks for your password $user_pwd"

#!/bin/bash

  $* =
  Represent all the arguments as a single string

echo "all params: $*"
echo "number of params: $#"

  $# =
  Total number of arguments provided

  Shell loops

  Loops in Linux
  Enables you to execute a set of commands repeatedly
  There are different types of loops:
    - while loop
    - for loop
    - until loop
    - select loop

  For Loop
  Operates on the lists of items
  Repeats a set of commands for every item in the list

  for var in word1 word2 ...
  do 
    statement(s) to be executed for every word
  done

  for param in $*
    do
      echo $param
    done

  for param in $*
    do
    if [ -d "param" ]
    then
      echo "executing $param scripts in the config folder"
      ls -l "$param"

    else
      echo $param

    done

  While Loop

  Execute as set of commands repeatedly until some condition is matched

  ex: ping until service is available
      monitor some endpoint every 10 secs
      validate that some service is available


  while condition
  do
    statement(s) to be executed if command is true
  done


  sum=
  while true
    do
    read -p "enter a score" score

    if [ $score" == "q" ]
    then
      break
    fi

    Double Parenthesis for arithmetic operations

    $((2-4))  $(( $num1 + $num2 ))

    sum=$((sum+$score))
    echo "total score: $sum"

    done

    [ - POSIX 

    [[ - BASH 

    more features - don't need to enclose the variable in quotes ""
    but: you lose portability

    Better alternatives:

    Python
    Ansible


  



  

# Usage

echo "Setup and configure server"

file_name=config.yaml

config_dir=$1

file_name=config.yaml
if [ -d "$config_dir" ]
then
  echo "config directory contents"
  config_files=$(ls "$config_dir")
else
  echo "config dir found. Creating one"
  mkdir -pv "$config_dir"
  touch "config_dir/config.sh"


echo ""
user_group=$2
echo ""

if [ "$user_group" == "mike" ]
then
  echo "configure the server"
elif
  [ "$user_group" == "admin" ]
then
  echo "administer the server"
else
  echo "no permission to configure the server. wrong user group"
fi