---
title: "KVM, QEMU, Libvirt"
date: 2020-07-23T15:00:00+05:30
draft: false
toc: false
images:
tags:
  - KVM
  - QEMU
  - Libvirt
  - Virtual Machine
  - VM
---

## Introduction

I was looking into virtualization in Linux systems. As we know Linux has arsenal of tools in its back-pocket. It did have something up its sleeve KVM, QEMU, Libvirt. And there a lot of other 3rd party options like VMWare, VirtulBox to name some.

Its interesting that KVM is a kernel module in the Linux system, which helps in getting native-like performance in the virtual machine. So we will be looking into tweaking KVM, QEMU, Libvirt to our convenience and usage.

## A tl;dr of Virtualization

* Well, Virtualization is a technology which lets a user create multiple virtual instances of the same host OS or different one in a single, physical hardware.

* This means we can create, manage and use multiple operating systems in a single machine.

* There are many uses of a virtual machine. For Example
    - To test an application in a particular environment.
    - To experience an operating system before moving to it.
    - To improve ones privacy by creating a true Internet sandboxes.
    - For Data centers and Service providers:
        - Resource efficiency
        - Easier management
        - Faster provisioning

* Virtualization is done with the help of **Hypervisor**. A Hypervisor is a software that creates and runs virtual machine. It connects directly to the hardware and allows a user to split a single system into separate, different environments. It also distributes the resources to the VM and the host appropriately. It can also be called as virtual machine monitor (VMM).

![Virtual machine](/img/4_1.png)

### Hypervisor

* There are 2 types of Hypervisors
  - Type 1 - Bare Metal hypervisors
  - Type 2 - Hosted hypervisors

#### Type 1 Hypervisor

* A Type 1 hypervisor directly interacts with the physical hardware's CPU, memory, and storage.

* They are mostly used in server-based environments.

* These type of hypervisors are highly efficient. They provide bare-metal performances.

* They require a separate management machine to control/administer different VMs and control the host machine.

* Examples:

  - KVM (Kernel-based Virtual Machine)
  - Xen
  - VMWare ESXi

#### Type 2 Hypervisor

* A Type 2 hypervisor runs in an application which runs in an OS.

* They are mostly used by the individuals running multiple systems in their personal computer.

* These type of hypervisors enables the use of multiple guest operating systems alongside the host for the end user.

* They access the resources via host operating system introducing latency, which decreases the performance.

* The security of these guest machines can be compromised.

* Examples:

  - QEMU
  - VirtualBox
  - VMWare Player
  - VMWare Workstation


### Types of Virtualization

* Desktop virtualization

* Network virtualization

* Storage virtualization

* Data virtualization

* Application virtualization

* Data center virtualization

* CPU virtualization

* GPU virtualization

* Linux virtualization

* Cloud virtualization

We will looking into Linux Virtualization with KVM, QEMU, Libvirt.


## KVM

* KVM stands for Kernel-based Virtual Machine.

* It is built into the Linux kernel from 2.6.20.

* KVM make the Linux into a hypervisor, which allows a user to to run multiple virtual machines in the host machine.

* KVM converts Linux into type-1 hypervisor. Every guest machine hosted on the host machine in Linux with KVM is treated as a regular process. 

* The guest machines uses the standard Linux scheduler built into the kernel with the help of KVM.

* Rest of the components like network card, graphics adapter, CPUs, memory and storage are virtualized.

* **Features**

	- As KVM is built in kernel it acquires the scalability and performance. 
	- As the VM is a Linux process, it can be controlled through the kernel features like cgroups, completely fair scheduler, name spaces, etc.
	- It inherits memory management from the kernel, like kernel same-paging, non-uniform memory access.
	- It can support different types and hardware.
	- KVM uses SELinux (Security Enhanced Linux) and secure virtualization (sVirt) to improve VM's security.
	- Lower latency issues.

## QEMU

* QEMU stands for Quick Emulator.

* It is an open source machine emulator and virtualizer.

* With the help of dynamic binary translation it can emulate a different machine's processor. 

* For example, using QEMU an user can emulate an ARM board in their PC. 

* By using binary translation it achieves good performance.

* It can use other hypervisors like Xen, KVM to use Hardware-assisted virtualization. 

* **Features**

	- QEMU can save and restore a virtual machine wit all the application running.
	- Operating systems need not to be patched for running them in QEMU.
	- Can emulate a lot different types of architectures.
	- Uses special format for storing the virtual machine qcow or qcow2. This formats only take up as much disk space as the guest OS does.
	- Allows the host and guest machine to communicate with integrating services like SMB server.
	- Can also support different image formats like `.vdi`, `.vmdk`, etc.


## Libvirt

* Libvirt is an Open source API, daemon that provides a way to manage virtual machines.

* It can be used to manage KVM/QEMU, Xen, LXC, etc.

* **Features**

	- VM Management
	- Remote machine support
	- Storage management
	- Network interface management
	- Virtual NAT and Route based networking

* There are different user interfaces that use libvirt Virtual Machine Manager, GNOME Boxes, virsh, oVirt, etc.

## Differences between QEMU, KVM and Libvirt

* QEMU and KVM both act as a hypervisor.

* QEMU runs slower when the system doesn't have hardware virtualization. To resolve this we use KVM. KVM helps QEMU access the virtualization features on different architectures. KVM also adds acceleration feature to the QEMU process.

* Libvirt is a simple virtualization library. It is used to manage KVM and QEMU. It is effective and can handle a lot of hypervisors together. It consists of three utilities: API library, a daemon (libvirtd) and a command line tool (virsh).

* So in-short:
    - QEMU - Hypervisor
    - KVM - Accelerating agent
    - Libvirt - A Management library

## Continued

In the next post, we will be looking into virt-install to create VM and virsh to manage them.
