# Chapter2
This module we look at User and File Permissions.

# Name: Users and Permissions


# Description: 

  In this module we review User Account, Groups & Ownership and file permissions. We looked at Linux commands for managing Users and their permissions. When referring to User Permissions in Linux its related to reading, writing and execution of files.

  The command ls -l shares additional information, which includes owners, group, and others.

  There are two concepts that need to be understood:

  OWNERSHIP - who owns the files or direcotry?

  First is the owner the second is primary group, the third is every one else

  use chown to change ownership

  chown tom file

  CHANGE OWNER AND GROUP
  
  chown michael:tom file

  CHANGE GROUP

  chgrp michael file

  PERMISSIONS - 

  d r w x r w x r - x
    Owner
          Group
                Other - all other users, who are not the file owner or don't belong to the group owner

  - regular file
  d directory
  c character device file
  l symbolic

    Owner represents the next 3 columns
    r read
      w write - can edit the file
        x execute - can execute scripts/executable

    - - - means no permisson     

    - no permission

    MODIFY PERMISSIONS

    Using symbolic values

    chmod -x file remove x from all users

    chmod g-x file remove x from group only

    chmod g+x file add x to the group only

    chmod u+x add x to user will allow to run a script

    u - owner, g - group, o - other
    + add, - remove

    chmod g=r-x filename
    chmod u=rwx filename

    Using numeric values for the entire block

    Absolute Numeric Value

    Number    Permission Type     Symbol

     0        No permission       ---
     1        Execute             --x
     2        write               -w-
     3        write/execute       -wx
     4        read                r--
     5        read/execute        r-x
     6        read/write          rw-
     7        read/write/execute  rwx

     chmod 777 filename  defines the entire block

There ways to set permissions

1. Symbolic mode
2. Set permissions
3. Numeric mode

hidden files must be displayed using ls -la

# Usage

chmod +/- r/w/x/-
chmod -x file
chmod +x file

chmod u/g/o +/- file

chmod 750