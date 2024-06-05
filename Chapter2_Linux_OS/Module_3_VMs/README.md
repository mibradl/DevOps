# Chapter2
This module explains operating systems and linux basics

# Name: Creating Virtual Machines


# Description: 

  This chapter introduces the concept of virtualization, used to
  host new systems on top of the OS of a physical machine, such as your laptop. This is known as a type 2 hypervisor. These are guests VMs who borrow 
  resources from the Host OS, like a server. The second type is known as a type 1 which sits directly on the baremetal hypervisor. This is typically 
  used on large systems within a corporation, used for hosting VMWare ESXi, Microsoft Hyper-v, etc. Once either of these models are setup one or more 
  VMs can be deployed and an OS provisioned for the user to install products of their choice and utilize tools such as: CLI, SSH, IDEs and scripts to 
  automate manual processes to make them available for reuse.  

# Technologies Used:

  Cloud Provider - Digital Ocean was used as the easiest path for the MacOS, as it doesn't require resources.

# Usage

  For demo exercise created Ubuntu version 20.04 on Digital Ocean

  root@web-server:~# cat /etc/os-release 
  NAME="Ubuntu"
  VERSION="20.04.4 LTS (Focal Fossa)"
  ID=ubuntu
  ID_LIKE=debian
  PRETTY_NAME="Ubuntu 20.04.4 LTS"
  VERSION_ID="20.04"
  HOME_URL="https://www.ubuntu.com/"
  SUPPORT_URL="https://help.ubuntu.com/"
  BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
  PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
  VERSION_CODENAME=focal
  UBUNTU_CODENAME=focal

