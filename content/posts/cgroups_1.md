---
title: "control groups (cgroups) - Introduction"
date: 2020-06-17T10:30:00+05:30
draft: false
toc: false
images:
tags:
  - Linux
  - Containers
  - cgroups
---


The Linux Kernel is full of mysteries and tools which can help boost the performance, improve the efficiency and at the same time break the system. This series mainly focuses on the "**cgroups**" feature in the Linux Kernel. There will be a series of posts explaining, demonstrating, and tweaking it.

## Part I - Introduction

### What is cgroups

**cgroups** is a feature available to the end user which is present in Linux Kernel. It can be used in limiting, prioritizing, accounting and controlling the resources to a specific group of processes/tasks.

A resource may be CPU time, Memory, Network bandwidth, etc. For example, if a browser (or) any application uses more than 25% of the memory, it can be limited to use not more than 15% with the help of cgroups.

As the name says, it is a group of processes aggregated/partitioned. The cgroups is organised hierarchically, which makes the child inherit the parent's limits/configuration.

### The Model

The hierarchy is implemented everywhere in the Linux Kernel. The hierarchy is set of cgroups arranged in tree. Take **init process**, it is the first thing that starts after the boot, this makes it the parent of all the other processes. The Linux Model is a single hierarchy/tree.

Whereas the cgroup model can have different groups of hierarchies, making it an unconnected trees of tasks. Multiple hierarchy means that for each subsystem there exist a cgroup model.

### Development

It seems weird and interesting that the idea of cgroups was first coined by Google, mainly the Devs - Paul Menage and Rohit Seth. It was first implemented in the year of 2006 and named as "[Process Containers](https://lwn.net/Articles/236038/)". Later on the it has been to changed to "control groups" due to the name having containers which caused a lot of confusions.

The control groups were first implemented in Linux Kernel version 2.6.24 (released in 2008). Continuing from there on, the developers have been adding features and supporting it till date. The features include kernfs, unified hierarchy, firewalling, etc.

Overall there were two versions of cgroups. The [v1](https://man7.org/linux/man-pages/man7/cgroups.7.html#CGROUPS_VERSION_1) which was developed by the devs from google and the [v2](https://man7.org/linux/man-pages/man7/cgroups.7.html#CGROUPS_VERSION_2) was developed by Tejun Heo after acquiring it. The v2 was first implemented in Linux Kernel version 4.5 (released in 2016).


### Why do we need it?

There were many attempts to create a system that can help in tracking the resources. But they lack in grouping/partitioning of processes which lead to forking or creating child processes in the same group as their parent.

The cgroups provide the required mechanisms to implement this efficiently. cgroups has also provided multiple hierarchy support, which would help in dividing different tasks for different subsystems. This enables parallel hierarchy that helps in handling complex combinations of processes, without which would to lead to combinations of tasks in the same cgroup.

#### Example (Taken from [5](https://www.kernel.org/doc/Documentation/cgroup-v1/cgroups.txt))

![example](/img/1.png)


Consider the above hierarchy which illustrate the use of multiple hierarchy in a university where various users like professors, students and system use the same server. The usage is divided in the following way

Memory:
  * Professors - 45%
  * Students - 30%
  * System - 25%

Disk:
  * Professors - 45%
  * Students - 30%
  * System - 25%

Network:
  * WWW Browsing - 25%
  * Network File System - 55%
  * Others - 20%

When we use multiple hierarchy, the tasks can be classified differently for different resources, like allocating resource subsystems into different hierarchies, we can easily modify the system to work differently depending on who is using.

When we only use a single hierarchy, the admin needs to create a separate cgroup for every specific application used and needs to allocate the resources. Single hierarchy also makes it hard to provide enhanced privileges to a user.

### Alternative

There are three different system calls that can be used to limit the resources to a specific process/tasks.
* `getrlimit`
* `setrlimit`
* `prlimit`

Calls to the `getrlimit` and `setrlimit` system allow a process to read and set limitations on the system resources it may consume. The `prlimit` allows the resource limits of a process specified by PID to be set and read.

For more understanding and usage, check the man pages:
* [getrlimit](https://man7.org/linux/man-pages/man2/getrlimit.2.html)
* [setrlimit](https://linux.die.net/man/2/setrlimit)
* [prlimit](https://www.man7.org/linux/man-pages/man1/prlimit.1.html)


### Different cgroups

There are 12 different types of cgroups in Linux:

* `blkio` - Block I/O (or) set limits to read/write from/to.
* `cpu` - Control CPU time using CFS (Completely Fair Scheduler).
* `cpuacct` - Reports are generated regarding the usage of CPU resources by a process.
* `cpuset` - Assings inividual CPUs and memory nodes to tasks (in a single cgroup).
* `devices` - Allow/Deny access to devices.
* `freezer` - Suspends/Resumes a process.
* `hugetlb` - Allows/Denys the use of huge pages for a specific group.
* `memory` - Set limits on usage for memory for a task/process.
* `net_cls` - Allows to note/mark specific packets from a group.
* `net_prio` - Set the priority dynamically to the network traffic.
* `perf_event` - Allows access to a perf events to a group.
* `pids` -  To limit the number of process/tasks from being forked for cloned when a certain limit is reached.

### Enabled cgroups

To see the what/which cgroups are present in your system:

```
cat /proc/cgroups
```
![cat /proc/cgroups](/img/2.png)

<br>

You can also see via sysfs:

```
ls -l /sys/fs/cgroup/
```

![ls -l /sys/fs/cgroup/](/img/3.png)


To get cgroups for a specific PID, we can use **cat /proc/[PID]/cgroup/**.

To get the PIDs use:
```
ps -d
```

![ps -d](/img/4.png)


I will be showing the cgroups of the ZSH shell (PID: 72980)

```
cat /proc/72980/cgroup
```

![cat /proc/72980/cgroup](/img/5.png)


### Continued

This is a series that explains the cgroups. The following posts will be demonstrating and tweaking the cgroups.


### Resources

1. https://en.wikipedia.org/wiki/Cgroups#cite_ref-4
2. https://wiki.archlinux.org/index.php/cgroups
3. https://www.grant.pizza/blog/understanding-cgroups/#practically-using-cgroups
4. https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/performance_tuning_guide/chap-red_hat_enterprise_linux-performance_tuning_guide-overview
5. https://www.kernel.org/doc/Documentation/cgroup-v1/cgroups.txt
6. https://www.kernel.org/doc/Documentation/cgroup-v1/
7. https://www.kernel.org/doc/Documentation/cgroup-v2.txt
8. https://man7.org/linux/man-pages/man7/cgroups.7.html#CGROUPS_VERSION_1
9. https://man7.org/linux/man-pages/man7/cgroups.7.html#CGROUPS_VERSION_2
10. https://0xax.gitbooks.io/linux-insides/content/Cgroups/linux-cgroups-1.html
