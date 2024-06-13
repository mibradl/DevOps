# Chapter3
This module we discuss how to delete branches.

# Name: Deleting Branches

# Description: 

There are two options for dealing the branch once it's merged with the master:

Leave the branch

Delete the branch

Best practice is the delete the branch after merging because no one knows:

    which one is active
    which one is finished
    which one is merged


Create new branch when needed

Deleting the branch avoids confusion

Delete the branch locally

1. git checkout master

2. Perform git pull

3. git branch -d feature/database-connection (-d flag deletes)








# Usage



