# Chapter3
This module we learned when to use .gitignore to omit certain folders and files.

# Name: Gitignore

# Description: 
Don't Track Certain Files

Add .gitignore file to your project (root directory)

To exclude certain folders or files from being tracked by Git

    files/folders specific to your editor

Build folders, where compiled code is located

Open .gitignore file and add any files or folders you plan to omit.

After performing git status you may still see file/folders that were cached by git. You will need to remove these from the cache.

git rm  -r --cached . or specific file  They are no longer tracked locally and removed remotely

git push 




# Usage



