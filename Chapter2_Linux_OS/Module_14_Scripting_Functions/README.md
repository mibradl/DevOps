# Chapter2
This module discusses the use of syntax and concepts for functions.

# Name: Concepts and syntax - Functions

# Description: 
Why functions? 
Better overview when functions are named descriptively
Reuse code

Functions
Enable you to break down the overall functionality of a script into smaller, logical code blocks

function () {
  list of commands
}

This code block can then be referenced anywhere in the script multiple times.

Pass Parameters to a Function

Accept parameters in a function
The parameters passed in are represented by $1, $2 etc.

function create_file() {
  file_name=$1
  touch $file_name
  echo "file $file_name created"
}

create_file test.txt

create_file my_file.yaml

create_file myscript.sh


Best practices

Don't use too many parameters

Apply the Singe Responsibility Principle:
a function should only do one thing

Declare variable with a meaningful name for positional parameters within a function

Boolean

A data type that can only have two values

true or false

Returning Values from Functions

$? - Captures value returned by last command

# Usage


    function score_sum {
    sum=0
    while true
      do
      read -p "enter a score" score

      if [ $score" == "q" ]
      then
        break
      fi

    #Double Parenthesis for arithmetic operations

    $((2-4))  $(( $num1 + $num2 ))

    sum=$(( sum+$score ))
    echo "total score: $sum"  
    }

    score_sum 


    function create_file() {
      file_name=$1
      is_shell_script=$2
      touch $file_name
      echo "file $file_name created"

      if [ "$is_shell_script" = true ]
      then
        chmod u+x "$file_name" true
        echo "executed permission"
      fi
    }

create_file test.txt

create_file my_file.yaml

create_file myscript.sh

function sum() {
  total=$(($1+$2))
  return $total
}

sum 2 10
result=$?
echo "sum of 2 and 10 is $result"