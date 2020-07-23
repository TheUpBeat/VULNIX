---
title: "Creating and Managing Virtual Machines using Libvirt"
date: 2020-07-23T18:00:00+05:30
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

As we have seen a little into KVM/QEMU, Libvirt in the previous post, this post will demonstrate how to create, manage and remove virtual machines.

---
**Note**

I use Void Linux as my base operating system (recently hoped from Manjaro), so the commands will be according to it. But most of the packages are some with similar or different names.

---

Before installing packages for KVM/QEMU, we need to check for KVM support.

## Prerequisites for KVM:

* To use KVM the host's processor should support virtualization. It is named different for different processor `VT-x` for `Intel` and `AMD-V` for `AMD` processors.

* To check whether the system provides the support for virtualization for either CPUs, use the following command:

```
LC_ALL=C lscpu | grep Virtualization
```

Or

```
grep -E 'vmx|svm|0xc0f' /proc/cpuinfo
```

If nothing is displayed after running either of the two commands, then the processor does not support hardware virtualization, and will unable to use KVM.

The virtualization support can be turned on in the BIOS (if available).

* To enable KVM, your user must be added to the `KVM` group:

```
usermod -aG kvm <username>
```

* To check whether the modules are loaded in the kernel, use the following command:

```
lsmod | grep kvm
```

If the output is either `kvm_intel` or `kvm_amd`, the modules are loaded.

If the modules are not loaded, use the following command to do so:

```
modprobe kvm-amd # for AMD CPUs

modprobe kvm-intel # for Intel CPUs
```

* Kernel version should be greater than `2.6.60`.

## Packages and Dependencies

If everything is set right, we need to install the packages and dependencies that are required to create/manage a VM.

* `qemu` - This package provides the `x86_64` architecture emulators for both full-system emulation (`qemu-system-x86_64`) and usermode (`qemu-x86_64`) variants.

```
xbps-install -S qemu
```

* `bridge-utils` - Bridges the physical interfaces and the VM interfaces

```
xbps-install -S bridge-utils
```

* `libvirt` - The libvirt package provides the daemon libvirtd which handles library calls, manages virtual machines and hypervisor controls.

```
xbps-install -S libvirt
```

Add the user to the libvirt group:

```
usermod -aG libvirt <user>
```

To start the `libvirtd` daemon, run:

```
ln -s /etc/sv/libvirtd /var/service
```

To check the status:

```
sv status libvirtd
```

* `virt-manager` - virt-manager is a graphical tool for managing virtual machines via libvirt. It includes virt-install, virt-clone, virt-xml.

```
xbps-install -S virt-manager
```

Start the `virtlogd` service:

```
ln -s /etc/sv/virtlogd/ /var/service/
```

* `libguestfs` - libguestfs is a set of tools for accessing and modifying virtual machine (VM) disk images. It can access alomost any disk image including Vmware’s VMDK & Hyper-V disk format.

```
xbps-install -S libguestfs
```

This package includes virsh.

* `virt-viewer` - Virt Viewer provides a graphical viewer for the guest OS display. At this time is supports guest OS using the VNC or SPICE protocols.

```
xbps-install -S virt-viewer
```

**To install all the packages**

```
xbps-install -S qemu bridge-utils libvirt virt-manager libguestfs virt-viewer
```


## Creating a VM

Now that we have installed all the required packages and its dependencies, we can now proceed to create a virtual machine with `virt-install`.

Now before getting into it, the following are the details of the VM to be created:

```
VM Name   -   Manjaro
NETWORK   -   virbr0 (virbr0/br0)
RAM       -   4096
CPU       -   4
DISK      -   25GB
CD-ROM    -   /vol1/ISO/manjaro.iso
```

* `virt-install` options that are required to create a VM:

|	   Options		| 	   Values		|	   Description 		   |
|-------------------------------|-------------------------------| -------------------------------- |
| 	  -\-connect		|	  qemu:///system	| Connect to the localhost KVM	   |
|	  -\-name		| 	  Manjaro		| Name of the VM		   |
|	  -\-ram		|	  4096			| Size of RAM in MB		   |
|	  -\-vcpus		|	  4			| No. of vcpus 			   |
|	  -\-os-type		|	  linux			| OS type			   |
|	  -\-os-variant		|	  manjaro		| OS variant (use osinfo-query os) |
| 	  -\-network		|	  network=virbr0	| Bridge for network connectivity  |
|	  -\-graphics		|	  spice			| Configure guest display settings |
|	  -\-virt-type		|	  kvm			| Virtualization type as KVM or Xen|
|	  -\-disk size		|	  25			| Disk Space of the VM		   |
|	  -\-cdrom		|	  /path/of/iso		| Path for the ISO file		   |



```
virt-install --connect qemu:///system --name Manjaro --ram 4096 --vcpus 4 --os-type linux --os-variant manjaro --network bridge=virbr0 --graphics spice --virt-type kvm --disk size=25 --cdrom /vol1/ISO/manjaro-xfce-20.0.1-200511-linux56.iso
```

* To shutdown the machine

```
virsh shutdown <domain-name>
```

## Managing virtual machines with virsh

* The user can create, manage, delete, start and stop virtual machines with the virsh command.

* It can be used in scripts for automating some of the aspects of managing the virtual machine.

* The terminology virsh uses is a little different. The term `domain` will be used instead of the term guest virtual machine.

* The virtual machine that is created in one user cannot be retrieved by other user. To access all the virtual machines in a system, change the livbirt URL to access all the guests in a system.

```
virsh --connect qemu:///system list --all
```

The above command outputs the guest machines in a single system including all the ones from different users.

* The domain name of the machine create above is `Manjaro`.

* To start a virtual machine

```
virsh --connect qemu:///system start Manjaro
```

* To reboot a virtual machine

```
virsh --connect qemu:///system reboot Manjaro
```

* To shutdown a virtual machine

```
virsh --connect qemu:///system shutdown Manjaro
```

* To suspend a virtual machine

```
virsh --connect qemu:///system suspend Manjaro
```

* To resume a virtual machine

```
virsh --connect qemu:///system resume Manjaro
```

### To edit the storage path of the virtual machine

In KVM, the virtual machines are store in `/var/lib/libvirt/images`. The default storage path can be changed to the users preference.

* First, login to the host machine. Shutdown all the VMs, if any loaded.

* To list the storage pool

```
virsh --connect qemu:///system pool-list
```

* To get the info about the storage pool

```
virsh --connect qemu:///system pool-info default
```

* To get the path of the storage pool

```
virsh --connect qemu:///system pool-dumpxml default | grep -i path
```

* To check whether the existing VMs are located in the pool path

```
virsh --connect qemu:///system vol-list default | grep "/var/lib/libvirt/images"

virsh --connect qemu:///system vol-list default
```

* Before changing the path, stop the storage pool

```
virsh --connect qemu:///system pool-destroy default
```

* Edit the default pool configuration

```
virsh --connect qemu:///system pool-edit default
```

* Edit the default pool configuration

```
virsh --connect qemu:///system pool-edit default
```

* Change the path in the `<path>your_new_path</path>`

* After editing the path, start the storage pool.

```
virsh --connect qemu:///system pool-start default
```

* To verify

```
virsh --connect qemu:///system pool-dumpxml default | grep -i path
```

* To move the exciting VMs to the new path

```
mv /var/lib/libvirt/images/<name_of_vm>.qcow2 /your/new/path/
```

* Edit the VM config's to the new path

```
virsh --connect qemu:///system edit Manjaro
```

Change the `source file='/var/lib/libvirt/images/<name_of_vm>.qcow2'` to `source file='\your\new\path\<name_of_vm>.qcow2'`

* Everything is set. Start the KVM guest.

```
virsh --connect qemu:///system start Manjaro
```

### Add virtual disk to an active virtual machine

To add extra disk space to an existing VM

* `qemu-img` is used to create a virtual disk

```
qemu-img resize Manjaro.qcow2 +10G
```

This add ('+') 10GB to the virtual machine.


### Add Memory to the virtual machine

For example, lets try to increase the memory of the virtual machine from 4GB to 6GB.

* Shutdown the virtual machine

* Using `virsh edit` edit the VM

```
virsh --connect qemu:///system edit Manjaro
```

* Change the value of the memory size (in KiB) in `<memory unit='KiB'>4194304</memory>` to `<memory unit='KiB'>6291456</memory>`

* Save the file and exit. Restarting the VM will show the updated configuration.


### Add VCPUs to a virtual machine

To increase the number of VCPUs allocated to a virtual machine edit the xml file.

* Shutdown the virtual machine.

* Using `virsh edit` edit the configuration of the virtual machine.

* Change the value from `<vcpu placement='static'>2</vcpu>` to `<vcpu placement='static'>4</vcpu>`

* Save and exit. Restarting the virtual machine should change the configuration.

### Saving the configuration and recreating

The configuration of virtual machine can be saved and can be used to recreate the virtual machine.

* To Save

```
virsh --connect qemu:///connect dumpxml Manjaro > Manjaro.xml
```

* To recreate the machine from the `xml` configuration

```
virsh --connect qemu:///system create Manjaro.xml
```

### Delete the virtual machine

* Shutdown the virtual machine

* Destroy the virtual machine

```
virsh --connect qemu:///system destroy Manjaro
```

* Undefine the virtual machine

```
virsh --connect qemu:///system undefine Manjaro
```

* Now, remove the disk image file from the system

```
rm /var/lib/libvirt/images/Manjaro.qcow2
```

