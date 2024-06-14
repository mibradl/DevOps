# Chapter3
This module we learned how to undo commits.

# Name: Undoing Commits

# Description: 

Some things that must be understood.

Commits are only local. The remote repository doesn't know about it.

Suppose I realize that I made a mistake and I need to revert the change and undo the commit.  Since the change was only local and no other updates have been made I can safely reset the commit.

git log

This is a history of commits and shows the HEAD, which is the pointer of the last commit.

git reset --hard HEAD~1  ## ~1 represents the number of commits reverted and discards the changes.

git reset --soft HEAD~1  ## reverts the commit, but does not discard the change

git reset HEAD~1 ## soft is the default

Use soft to keep the changes in your working directory


Amend - allows the previous commit to be amended and merged.

git add .
git commit --amend

changes the last commit

# Usage



