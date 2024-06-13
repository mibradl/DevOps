# Chapter3
This module we learned when to use .gitignore to omit certain folders and files.

# Name: Git Stash - Save work-in-progress changes (stash)

# Description: 

Git Stash

Suppose you are working on a local project and you need to move to another branch.  So, there are unfinished changes, which don't want to commit yetl

If you do:

git checkout master
error: Your local changes to the following files would be overwritten by checkout:
        file.txt
Please commit your changes or stash them before you switch branches.
Aborting

git stash
saved working directory and index state WIP on <branch name>

git status - will show is up to date

git checkout master
switched to master branch

once finished

git checkout feature/database-connection
switched to branch, but how do I get my changes back?

git stash pop = git the changes back
dropped refs/stash@{0}

                    git stash pop

Working Directory <--------------- Stash

Another use Case for stashing

Making changes to the current branch

Notice something isn't working anymore

    Did I break it with my changes?

Hide changes (revert) temporarily to test if it works without my code changes

    git stash

Bring changes back to my local working directory

    git stash pop

    




# Usage



