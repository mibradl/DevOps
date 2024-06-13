# Chapter3
This module we learned how work with Git.

# Name: Working with Git

# Description: 

Three Stages of Code in Local Repository

Working Directory - git add . to promote to staging area.

Staging Area - once in the staging directory a commit will move the change to the local repo.

Local Repository



# Usage

touch README.md

michaelbradley@Michaels-iMac-2.local /Users/michaelbradley/new-project 
% touch README.md
michaelbradley@Michaels-iMac-2.local /Users/michaelbradley/new-project 
% ls 
README.md
michaelbradley@Michaels-iMac-2.local /Users/michaelbradley/new-project 

vim READme.md

% git status
On branch main

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
	README.md

nothing added to commit but untracked files present (use "git add" to track)
michaelbradley@Michaels-iMac-2.local /Users/michaelbradley/new-project

% git status
On branch main

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
	new file:   README.md

michaelbradley@Michaels-iMac-2.local /Users/michaelbradley/new-project6

% git add .
michaelbradley@Michaels-iMac-2.local /Users/michaelbradley/new-project 
% git status
On branch main

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
	new file:   README.md

michaelbradley@Michaels-iMac-2.local /Users/michaelbradley/new-project 

CHANGE ADDED TO LOCAL REPOSITORY

% git status
On branch main

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
	new file:   README.md

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
	modified:   README.md
michaelbradley@Michaels-iMac-2.local /Users/michaelbradley/new-project 

STATUS SHOWS CHANGE THAT WAS STAGED

% git commit
[main (root-commit) 472ce2c] Added readme.md file
 Committer: Michael Bradley <michaelbradley@Michaels-iMac-2.local>
Your name and email address were configured automatically based
on your username and hostname. Please check that they are accurate.
You can suppress this message by setting them explicitly:

    git config --global user.name "Your Name"
    git config --global user.email you@example.com

After doing this, you may fix the identity used for this commit with:

    git commit --amend --reset-author

 1 file changed, 1 insertion(+)
 create mode 100644 README.md
michaelbradley@Michaels-iMac-2.local /Users/michaelbradley/new-project [main]

COMMIT PLACES CHANGE TO LOCAL REPOSITORY

% git log
commit 472ce2c68635be60213ae8bc59d4b41d11da97cd (HEAD -> main)
Author: Michael Bradley <michaelbradley@Michaels-iMac-2.local>
Date:   Wed Jun 12 20:14:30 2024 -0400

    Added readme.md file
michaelbradley@Michaels-iMac-2.local /Users/michaelbradley/new-project [main]
%     

% git push
Enumerating objects: 3, done.
Counting objects: 100% (3/3), done.
Writing objects: 100% (3/3), 249 bytes | 249.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0 (from 0)
To gitlab.com:drmibradl/new-project.git
 * [new branch]      main -> main

Push to remote repository