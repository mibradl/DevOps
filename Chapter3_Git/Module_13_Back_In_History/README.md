# Chapter3
This module we learned about going back in history.

# Name: Going Back in History

# Description: 

Another interesting concepting understand how to use the logs to go back in history, using the commits. So, there is a list of my activities.

Each commit has this unique hash that identifies the commit.

Let's suppose we have some sort of bug that we don't know which commit caused the bug.

Based on the date stamp and the commit we are able to determine when the problem began.

In order to reproduce the problem we are able to go back in time referencing the unique hash.

We can go back to a specific project version

This can be performed by issuing the following command:

    git checkout d159195e40e995743bdc86c720c0bcd73d0ab77d

    Note: if there are other changes in flight remember you can perform git stash to save your changes.

Refer to usage below

git switch -
Previous HEAD position was 6640bc2 chapter 10

This will return you back to the previous HEAD position and restore hash.

# Usage

michaelbradley@Michaels-iMac-2 DevOps % git checkout 6640bc2d6496333bfce026db58bb702e7db672da
error: Your local changes to the following files would be overwritten by checkout:
        Chapter3_Git/Module_12_Git_Stash/README.md
Please commit your changes or stash them before you switch branches.
Aborting

michaelbradley@Michaels-iMac-2 DevOps % git stash
Saved working directory and index state WIP on main: 6cfd940 added Git Stash
michaelbradley@Michaels-iMac-2 DevOps % git status
On branch main
Your branch is up to date with 'origin/main'.

michaelbradley@Michaels-iMac-2 DevOps % git checkout 6640bc2d6496333bfce026db58bb702e7db672da
Note: switching to '6640bc2d6496333bfce026db58bb702e7db672da'.

You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by switching back to a branch.

If you want to create a new branch to retain commits you create, you may
do so (now or later) by using -c with the switch command. Example:

  git switch -c <new-branch-name>

Or undo this operation with:

  git switch -

Turn off this advice by setting config variable advice.detachedHead to false

HEAD is now at 6640bc2 chapter 10

michaelbradley@Michaels-iMac-2 DevOps % git switch -
Previous HEAD position was 6640bc2 chapter 10
Switched to branch 'main'
Your branch is up to date with 'origin/main'.
michaelbradley@Michaels-iMac-2 DevOps % 