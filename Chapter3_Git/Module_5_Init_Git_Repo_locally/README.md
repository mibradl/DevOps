# Chapter3
This module we learned about the concept of branches.

# Name: Concept of Branches

# Description: 

This module describes what to do in the event you have boiler-plate code that was develop and wish to add to the local git repository, instead of cloning the remote repo.

Master branch = main branch
is created by default, when initializing a git repo

Branches are used as a description of code being worked on

New features - naming standard feature/auth-user

Bug fixes - naming standard bugfix/auth-user-error

Best practice: 1 branch per bugfix or feature

Developers can commit without worrying about breaking the main branch

When done code can be merged with the main branch

New feature can be deployed

Large feature branches that are open for too long, increase the chance of merge conflicts

With branches you achieve a stable main (master) branch which is ready for deployment

Branches are based on Master and starts from same code base

Two ways to create a branch:

1. in the UI

2. Locally, on the CLI

The local branch shows the new remote branch was created

% git pull
From gitlab.com:drmibradl/test-local-init
 * [new branch]      bugfix/user-auth-error -> origin/bugfix/user-auth-error
Already up to date.
michaelbradley@Michaels-iMac-2.local /Users/michaelbradley/local-init [master]

To switch branches

git checkout <branch name> - switches branches

creates a copy of the feature/bugfix branch locally and connects to remote branch

Branches are based on master before creating new branch locally

git checkout -b <branch name> = creating and switching to new branch

Your branch is up to date with 'origin/master'.
michaelbradley@Michaels-iMac-2.local /Users/michaelbradley/local-init [master]
% git checkout -b feature/database-connection
Switched to a new branch 'feature/database-connection'
michaelbradley@Michaels-iMac-2.local /Users/michaelbradley/local-init [feature/database-connection]

Branch is created locally and checked out to the branch

Local updates can not be pushed to remote branch because it doesn't exist upstream (remotely). Thus, must push the branch.

% git push
fatal: The current branch feature/database-connection has no upstream branch.
To push the current branch and set the remote as upstream, use

    git push --set-upstream origin feature/database-connection

To have this happen automatically for branches without a tracking
upstream, see 'push.autoSetupRemote' in 'git help config'.

 git push --set-upstream origin feature/database-connection
Enumerating objects: 4, done.
Counting objects: 100% (4/4), done.
Delta compression using up to 12 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 363 bytes | 363.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0 (from 0)
remote: 
remote: To create a merge request for feature/database-connection, visit:
remote:   https://gitlab.com/drmibradl/test-local-init/-/merge_requests/new?merge_request%5Bsource_branch%5D=feature%2Fdatabase-connection
remote: 
To gitlab.com:drmibradl/test-local-init.git
 * [new branch]      feature/database-connection -> feature/database-connection
branch 'feature/database-connection' set up to track 'origin/feature/database-connection'.

Some companies have two branches:

1. Master 

2. Intermediary

For the duration of the sprint code is tested on the intermediary branch and then merged with master at the end of the sprint.

Trunk Based Development vs Feature Driven Development

Trunk Based

Better for continuous integration/delivery

Pipeline is triggered whenever feature/bugfix code is merged into master

test => build => deploy  -  continuous delivery

Deploying every single featuer/bugfix to at least staging server



Feature Driven Development

with DEV branch

Features/bugfixes are collected in development branch

Develop branch often becomes "work in progress" branch

Best Practice in DevOps

1. CI/CD
2. Trunk approach
3. No intermediary develop branch

Important to know there are different ways to Git workflows companies use

Ideal goal is to deploy changes on every merge



# Usage

Local Repository onl know about one branch initially

% git branch
* master
michaelbradley@Michaels-iMac-2.local /Users/michaelbradley/local-init [master]

% git pull
From gitlab.com:drmibradl/test-local-init
 * [new branch]      bugfix/user-auth-error -> origin/bugfix/user-auth-error
Already up to date.
michaelbradley@Michaels-iMac-2.local /Users/michaelbradley/local-init [master]

% git checkout bugfix/user-auth-error
branch 'bugfix/user-auth-error' set up to track 'origin/bugfix/user-auth-error'.
Switched to a new branch 'bugfix/user-auth-error'

Your branch is up to date with 'origin/master'.
michaelbradley@Michaels-iMac-2.local /Users/michaelbradley/local-init [master]
% git checkout -b feature/database-connection
Switched to a new branch 'feature/database-connection'
michaelbradley@Michaels-iMac-2.local /Users/michaelbradley/local-init [feature/database-connection]

% git push
fatal: The current branch feature/database-connection has no upstream branch.
To push the current branch and set the remote as upstream, use

    git push --set-upstream origin feature/database-connection

To have this happen automatically for branches without a tracking
upstream, see 'push.autoSetupRemote' in 'git help config'.

 git push --set-upstream origin feature/database-connection
Enumerating objects: 4, done.
Counting objects: 100% (4/4), done.
Delta compression using up to 12 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 363 bytes | 363.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0 (from 0)
remote: 
remote: To create a merge request for feature/database-connection, visit:
remote:   https://gitlab.com/drmibradl/test-local-init/-/merge_requests/new?merge_request%5Bsource_branch%5D=feature%2Fdatabase-connection
remote: 
To gitlab.com:drmibradl/test-local-init.git
 * [new branch]      feature/database-connection -> feature/database-connection
branch 'feature/database-connection' set up to track 'origin/feature/database-connection'.