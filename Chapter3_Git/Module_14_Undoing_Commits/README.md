# Chapter3
This module we learned how to undo commits.

# Name: Undoing Commits

# Description: 

Some things that must be understood.

Commits are only local. The remote repository doesn't know about it.

Suppose I realize that I made a mistake and I need to revert the change and undo the commit.  Since the change was only local and no other updates have been made I can safely reset the commit.

git log

This is a history of commits and shows the HEAD, which is the pointer of the last commit.

HARD

git reset --hard HEAD~1  ## ~1 represents the number of commits reverted and discards the changes.

SOFT

git reset --soft HEAD~1  ## reverts the commit, but does not discard the change

git reset HEAD~1 ## soft is the default

Use soft to keep the changes in your working directory


Amend - allows the previous commit to be amended and merged    

git add .
git commit --amend

changes the last commit

# Usage

git log

commit ed9464da69beb8601d7fe8a102dd9d1da97f8808 (HEAD -> main)
Author: Michael Bradley <michaelbradley@Michaels-iMac-2.local>
Date:   Fri Jun 14 10:27:31 2024 -0400

    corrected syntax

commit 84cafbaa464c7fc22f447c1d9b1eab896bde9374
Author: Michael Bradley <michaelbradley@Michaels-iMac-2.local>
Date:   Fri Jun 14 10:24:57 2024 -0400

    updated soft and amend

commit c4da65751900e54086b9507595ffbcc1022a7094 (origin/main, origin/HEAD)
Author: Michael Bradley <michaelbradley@Michaels-iMac-2.local>
Date:   Thu Jun 13 17:13:41 2024 -0400

    added going back in history

 michaelbradley@Michaels-iMac-2 DevOps % git reset --hard HEAD~1
HEAD is now at 84cafba updated soft and amend
michaelbradley@Michaels-iMac-2 DevOps % 



