# Chapter3
In this module we discuss resolving merge conflicts.

# Name: Resolving Merge Conflicts

# Description: 

Two or more developers update the same line of code and begin pushing code to remote repo. This of course will cause merge conflicts.

Before I can make any changes I first have to fetch that change.

And although I pull the change from the remote repo with rebase (git pull -r), I receive a merge conflict message. The issue is Git doesn't know which of the two changes you want to take.

You must tell Git which change to take.

This can be performed in the editor like VSCode or Intellij via the diff screen prompt, which shows each change the users made that are conflicting. Select which change to keep and continue with the commit and push via the tool prompts.

From the command-line you can perform the following:

git rebase --continue




# Usage



