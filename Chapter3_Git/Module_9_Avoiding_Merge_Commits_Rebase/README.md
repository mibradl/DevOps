# Chapter3
This module we discuss avoiding merge rebase.

# Name: Avoiding Merge Commits (Rebase)

# Description: 

If changes occur on the feature branch at the same time and one is pushed, the other local repo does not know about it and will cause a conflict when attempting to push.

to correct the other developer must perform a git pull (git fetch + git merge)

Once added to the staging area (git add .) and committed to the local repo, then the change can be pushed to the remote repo.

This push now contains two commits, the update and the merged commit, which is in turn pushed to the master.

To avoid this perform git pull -r

This pulls all the updates that were performed previously and stack them and then adds your change on top of the stack in one command.

no merge commit, much cleaner project history

git log will show the history with your change at the HEAD

Last step is to perform the git push to the remote repo.








# Usage



