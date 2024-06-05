# Chapter2
This module covers the use of the UNIX Text Editor, called VIM.

# Name: VIM Basics


# Description: 

  In this module we demo'd the VIM Text Editor. This modules explains why the CLI Text Editor is needed, as well as how it is used. How to write to a file. The answer is using VIM. Linux command line has a built in text editor called Vi or it's richer more modern version called VIM.

  Why CLI?

  Small modifications can be faster.
  Faster to create and edit at the same time.
  Supports opening all kinds fo file formats.
  When working on a remote server, it is often the only option.
  Git CLI - writing the commit message
  Display Kubernetes configuration files
  Quickly editing one line in a file



  
Usage: 

root@web-server:~# apt install vim
Reading package lists... Done
Building dependency tree       
Reading state information... Done
The following packages were automatically installed and are no longer required:
  fontconfig-config fonts-dejavu-core java-common libavahi-client3 libavahi-common-data libavahi-common3 libcups2 libfontconfig1 libgraphite2-3 libharfbuzz0b libjpeg-turbo8 libjpeg8
  liblcms2-2 libpcsclite1
Use 'apt autoremove' to remove them.
The following additional packages will be installed:
  vim-common vim-runtime vim-tiny
Suggested packages:
  ctags vim-doc vim-scripts indent
The following packages will be upgraded:
  vim vim-common vim-runtime vim-tiny
4 upgraded, 0 newly installed, 0 to remove and 61 not upgraded.
Need to get 7793 kB of archives.
After this operation, 0 B of additional disk space will be used.
Do you want to continue? [Y/n] 

root@web-server:~# vim --version
VIM - Vi IMproved 8.1 (2018 May 18, compiled May 03 2024 02:36:35)
Included patches: 1-213, 1840, 214-579, 1969, 580-1848, 4975, 5023, 2110, 1849-1854, 1857, 1855-1857, 1331, 1858, 1858-1859, 1873, 1860-1969, 1992, 1970-1992, 2010, 1993-2068, 2106, 2069-2106, 2108, 2107-2109, 2109-2111, 2111-2112, 2112-2269, 3612, 3625, 3669, 3741, 1847
Modified by team+vim@tracker.debian.org
Compiled by team+vim@tracker.debian.org
Huge version without GUI.  Features included (+) or not (-):

Two modes:

Command Mode (default mode)

Can't edit the text
Whatever you type is interpreted as a command
Navigate, Search, Delete, Undo, etc

Edit mode

Allows you to enter text

type in i to insert

close the edit mode by clicking esc

:wq to write qui

:q! discards the changes

Most frequently used commands

1. esc dd to delete line
2. d10 and and d again
3. u to undo previous command and u again to do it again.
4. A take you to end of the line and insert
5. 0 goes to the beginning of the line
6. $ end of the line
7. jump to specific line in code (ie line 12) type 12G
8. in command mode use /pattern and n for next and N previous
9. replace string :%s/nginx/webex, and u to undo


