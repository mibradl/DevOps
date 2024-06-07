# Chapter2
This module discusses the use of input and output

# Name: Pipes and Redirects


# Description: 

  In this module we review how to use input and output on the command line. The output from one program can become the input of another, by chaining commands.

  The pipe "|" command connects the input with an output command

  cat /var/log/syslog | less

  less - displays the contents of the a file or command output, one page at a time. Allows navigation forwards and backwards.

  Typically, used for opening large files, as less doesn't read the entire file.

  TO FILTER - grep

  stands for: Globally Search for Regular Expression and Print out

  searches for a particular pattern of characters and display all lines that contain that pattern

  history | grep systemctl - command highlights the pattern

  root@web-server:/var# history |grep "usermod -G" - multiple words should be contained in double quotes

  root@web-server:/var# ls /usr/bin | grep python
  python3
  python3.8

  REDIRECTS - directing command execution into a file

  > character is the redirect operator - takes the output from the previous command and sends it to a file.

  michaelbradley@Michaels-iMac-2 ~ % cat mongo.yaml |grep - > lists.txt
  michaelbradley@Michaels-iMac-2 ~ % ls 
  Desktop		Downloads	Movies		Pictures	lists.txt	my-demo
  Documents	Library		Music		Public		mongo.yaml	my-project

  >> appends text to end of file

  Every program has 3 built-in streams:

  STDIN(0) = Standard input

  STDOUT (1) = Standard output

  STDERR (2) = Standard Error

  We pipe or redirect the standard output from one command to the standard output of another command

  command > stdout > stdin > command > stdout

  STDERR example

  michaelbradley@Michaels-iMac-2 ~ % ls -l doesntexist 
  ls: doesntexist: No such file or directory

  Chaining commands

  clear; sleep 10; echo "Enjoy the training"
  

# Usage

ls /usr/bin | less

space - skip forward page at a time
b - backward

NF
VGAuthService
[
aa-enabled
aa-exec
add-apt-repository
addpart
apport-bug
apport-cli
apport-collect
apport-unpack
apropos
apt
apt-add-repository
apt-cache
apt-cdrom
apt-config
apt-extracttemplates
apt-ftparchive
apt-get
apt-key
apt-mark
apt-sortpkgs
arch

history | grep systemctl
   34  sudo systemctl start mongod
   35  sudo systemctl status mongod
   50  systemctl restart mongod
   51  systemctl status mongod
   87  sudo systemctl status mongod
   90  systemctl status mongod
  194  systemctl status mongod
  195  systemctl restart mongod
  196  systemctl status mongod
  284  history | grep systemctl

root@web-server:/var# history |grep "usermod -G"
  260  usermod -G admin tom
