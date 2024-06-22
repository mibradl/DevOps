# Chapter7
This module we explore Docker versus a VM.

# Name: Docker vs. Virtual Machine

# Description: 

Difference of a container and a virtual machine?

They are both virtual tools, so why is docker so widely used?

What advantages does it have over virtual machines?

How they work on top of an operating system?

How does Docker run its containers?

First, let's look how an OS is made up

    The OS has two layers

        OS Applications Layer - run on the Kernel layer (ubuntu, linuxmint, debian)  there are hundreds

        OS Kernel - is at the core of every os, and interacts between the hardware and software components

        Hardware - CPU, Disk

Docker and VM are both virtualization tools

What parts of the OS do they virtualize?

    Docker:

        Docker virtualizes the applications layer, contains the OS application layer

        Services and apps installed on top of that layer

        Uses the Kernal of the host

    Virtual Machine:

        Has the Applications Layer

        Also has the OS Kernel

        It virtualizes the complete operating system


    Docker                                                          Virtual Machine

    Docker images are much smaller              Size                Can be a couple of GB

    Usually a couple of MB                                          

    Containers take seconds to start            Speed               VMs take minutes to start

    Compatible only with Linux distro           Compatibility       VM is compatible with all OS

    Most containers are Linux based                                 Linux based Docker images cannot use Windows kernel

    Docker was originally built for Linux                           
        
    Later developer docker desktop for Windows
    and Mac, which makes it possible to run on
    Windows and Mac computers


    Docker Desktop  - uses a Hypervisor layer with a lightweight Linux distribution on top to provide the needed Linux Kernal to it possible to run on Windows and Mac OS computers
    



# Usage


    