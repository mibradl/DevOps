# Chapter3
This module we reviewed how to initialize a project locally.

# Name: Initialize a Git Project Locally

# Description: 

This module describes what to do in the event you have boiler-plate code that was develop and wish to add to the local git repository, instead of cloning the remote repo.

1. Create local git repository with "git init"

2. Add to staging area by performing git add

3. Commit the change to add the local repository

4. git status & git log will show code initialized and at head of the master branch

Local repo knows nothing about the remote repo yet

git remote add origin git@gitlab.com:drmibradl/test-local-init.git

origin = remote git project/repo

repo is connected
branch is not!

% git push
fatal: The current branch master has no upstream branch.
To push the current branch and set the remote as upstream, use

    git push --set-upstream origin master

To have this happen automatically for branches without a tracking
upstream, see 'push.autoSetupRemote' in 'git help config'.

To connect the branch

% git push --set-upstream origin master
Enumerating objects: 3, done.
Counting objects: 100% (3/3), done.
Delta compression using up to 12 threads
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 475 bytes | 475.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0 (from 0)
To gitlab.com:drmibradl/test-local-init.git
 * [new branch]      master -> master
branch 'master' set up to track 'origin/master

Branch connected

# Usage

% cd local-init 
michaelbradley@Michaels-iMac-2.local /Users/michaelbradley/local-init 
% git init
hint: Using 'master' as the name for the initial branch. This default branch name
hint: is subject to change. To configure the initial branch name to use in all
hint: of your new repositories, which will suppress this warning, call:
hint:
hint: 	git config --global init.defaultBranch <name>
hint:
hint: Names commonly chosen instead of 'master' are 'main', 'trunk' and
hint: 'development'. The just-created branch can be renamed via this command:
hint:
hint: 	git branch -m <name>
Initialized empty Git repository in /Users/michaelbradley/local-init/.git/
michaelbradley@Michaels-iMac-2.local /Users/michaelbradley/local-init 
% git status
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
	Dockerfile
	react-nodejs-example/

nothing added to commit but untracked files present (use "git add" to track)

git add . 
warning: adding embedded git repository: react-nodejs-example
hint: You've added another git repository inside your current repository.
hint: Clones of the outer repository will not contain the contents of
hint: the embedded repository and will not know how to obtain it.
hint: If you meant to add a submodule, use:
hint:
hint: 	git submodule add <url> react-nodejs-example
hint:
hint: If you added this path by mistake, you can remove it from the
hint: index with:
hint:
hint: 	git rm --cached react-nodejs-example
hint:
hint: See "git help submodule" for more information.
hint: Disable this message with "git config advice.addEmbeddedRepo false"
michaelbradley@Michaels-iMac-2.local /Users/michaelbradley/local-init 
% git status
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
	new file:   Dockerfile
	new file:   react-nodejs-example

michaelbradley@Michaels-iMac-2.local /Users/michaelbradley/local-init 
% git commit
[master (root-commit) dc3e91a] Initializing react-app
 Committer: Michael Bradley <michaelbradley@Michaels-iMac-2.local>
Your name and email address were configured automatically based
on your username and hostname. Please check that they are accurate.
You can suppress this message by setting them explicitly:

    git config --global user.name "Your Name"
    git config --global user.email you@example.com

After doing this, you may fix the identity used for this commit with:

    git commit --amend --reset-author

 2 files changed, 16 insertions(+)
 create mode 100644 Dockerfile
 create mode 160000 react-nodejs-example
michaelbradley@Michaels-iMac-2.local /Users/michaelbradley/local-init [master]
% git status
On branch master
nothing to commit, working tree clean
michaelbradley@Michaels-iMac-2.local /Users/michaelbradley/local-init [master]

 git log
commit dc3e91ab397eccb04c4ebe986b33c8214f52d51d (HEAD -> master)
Author: Michael Bradley <michaelbradley@Michaels-iMac-2.local>
Date:   Wed Jun 12 22:57:38 2024 -0400

    Initializing react-app
michaelbradley@Michaels-iMac-2.local /Users/michaelbradley/local-init [master]

michaelbradley@Michaels-iMac-2.local /Users/michaelbradley/local-init [master]
% git remote add origin git@gitlab.com:drmibradl/test-local-init.git
michaelbradley@Michaels-iMac-2.local /Users/michaelbradley/local-init [master]
% git status
On branch master
nothing to commit, working tree clean
michaelbradley@Michaels-iMac-2.local /Users/michaelbradley/local-init [master]
% git push
fatal: The current branch master has no upstream branch.
To push the current branch and set the remote as upstream, use

    git push --set-upstream origin master

To have this happen automatically for branches without a tracking
upstream, see 'push.autoSetupRemote' in 'git help config'.

michaelbradley@Michaels-iMac-2.local /Users/michaelbradley/local-init [master]
% % git push
fatal: The current branch master has no upstream branch.
To push the current branch and set the remote as upstream, use

    git push --set-upstream origin master

To have this happen automatically for branches without a tracking
upstream, see 'push.autoSetupRemote' in 'git help config'.

michaelbradley@Michaels-iMac-2.local /Users/michaelbradley/local-init [master]
% git push --set-upstream origin master
Enumerating objects: 3, done.
Counting objects: 100% (3/3), done.
Delta compression using up to 12 threads
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 475 bytes | 475.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0 (from 0)
To gitlab.com:drmibradl/test-local-init.git
 * [new branch]      master -> master
branch 'master' set up to track 


