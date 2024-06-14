# Chapter3
This module we learned how to merge branches.

# Name: Merging Branches

# Description: 

Suppose you have this bugfix and you significantly behind the master.  Master has two commits that have not been added to bugfix. Before completing your bugfix you should merge the changes from master branch.

local master branch needs to be up-to-date

git checkout master

git pull - to ensure all the changes are up to date

git checkout bugfix/user-auth-error
Your branch is up to date with origin/bugfix/user-auth-error

git merge master

This will take the master branch and merge it with bugfix/user-auth-error

git push - to push changes to remote bugfix/user-auth-error

Next we need to create a pull request that can be merged into master

someone reviews/approves the pull request

# Usage

