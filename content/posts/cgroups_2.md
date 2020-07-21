---
title: "control groups (cgroups) - CPUACCT"
date: 2020-06-23T18:00:23+05:30
draft: false
toc: false
images:
tags:
  - Linux
  - Containers
  - cgroups
---

As explained in the previous post, control groups are used to limit, prioritize, etc., the available resources in a system. In this series of post we will be creating, limiting, assigning tasks to the cgroups.

---
**NOTE**

In this whole process I will be using a Arch-based machine with Systemd, specifically Manjaro (please don't come after me).

---

## Part II - Demonstrating

### libcgroup

We can control the cgroups using shell commands. This can be done with the help of `libcgroup` package. The package provides a lot of features to the user. The features may vary like mounting, editing the values, etc.

#### Installation

To install libcgroup in your system, run the appropriate command:

For Arch-based machines:
```
pacman -S install libcgroup

If you are unable to do so, download the package from aur:

git clone https://aur.archlinux.org/libcgroup.git
cd libcgroup
makepkg -si
```

For Debian-based machines:
```
apt install libcgroup
```

For Fedora-based machines:
```
yum install libcgroup
```

#### Available cgroups

To see the available cgroups in your system, use:
```
lscgroup # This package is installed with libcgroup
```

![lscgroup](/img/2_1.png)

cgroups are located at `\sys\fs\cgroup\`, run the ls command to see the files in the directory:
```
ls -l \sys\fs\cgroup\
```
![ls -l \sys\fs\cgroup](/img/2_2.png)

### Working with cgroups

There are a 12 control groups:

* `blkio`      - Block I/O (or) set limits to read/write from/to.
* `cpu`        - Control CPU time using CFS (Completely Fair Scheduler).
* `cpuacct`    - Reports are generated regarding the usage of CPU resources by a process.
* `cpuset`     - Assings inividual CPUs and memory nodes to tasks (in a single cgroup).
* `devices`    - Allow/Deny access to devices.
* `freezer`    - Suspends/Resumes a process.
* `hugetlb`    - Allows/Denys the use of huge pages for a specific group.
* `memory`     - Set limits on usage for memory for a task/process.
* `net_cls`    - Allows to note/mark specific packets from a group.
* `net_prio`   - Set the priority dynamically to the network traffic.
* `perf_event` - Allows access to a perf events to a group.
* `pids`       - To limit the number of process/tasks from being forked for cloned when a certain limit is reached.

In this post, we will be looking at cpuacct.

#### CPUACCT

The cpuacct (CPU accounting controller) is used to group the tasks that are to be monitored for the usage of CPU.

CPUACCT can support multiple hierarchies. For example, if a group contains two subgroups, the subgroups usage can be monitored by the parent group.

To create CPUACCT groups we need to mount the cgroup filesystem (do this if the cgroup is not already mounted):

```
mount -t cgroup -o cpuacct none /sys/fs/cgroup
```

This mounts the parent group, can be visible at /sys/fs/cgroup.

You can view all the contents of the cpuacct folder, using ls command:

```
ls -l /sys/fs/cgroup/cpuacct/
```
![ls -l /sys/fs/cgroup/cpuacct](/img/2_4.png)

There are a lot of files in the cpuacct cgroup, we can see that most files starts with the controller name i.e. cpuacct. This is helpful when we combine two controllers like cpuacct and devices, there will be no conflicts between the controllers.

Now we will look at the tasks file which contains the PIDs that are attached to the cgroup. When booted all the tasks are entered into `/sys/fs/cgroup/cpuacct/tasks`. We can view it using the cat command:

```
cat /sys/fs/cgroup/cpuacct/tasks
```
![cat /sys/fs/cgroup/cpuacct/tasks](/img/2_3.png)

There are different files like cpuacct.usage, cpuacct.stat, etc. present in the directory.

The cpuacct.usage is used to see the consumed CPU time (presented in nanoseconds):
```
cat /sys/fs/cgroup/cpuacct/cpuacct.usage
```
![cat /sys/fs/cgroup/cpuacct/cpuacct.usage](/img/2_5.png)

The cpuacct.stat is used to see the consumed user and system CPU time by all the tasks included in the cgroup, this also includes the tasks in the lower hierarchy. It is presented as:

* `user` - CPU time consumed by tasks in user mode.
* `system` - CPU time consumed by tasks in system (kernel) mode.

To see the content of the file use the cat command:
```
cat /sys/fs/cgroup/cpuacct/cpuacct.stat
```
![cat /sys/fs/cgroup/cpuacct/cpuacct.stat](/img/2_6.png)

#### Creating New Groups under CPUACCT

The new groups should be created under `/sys/fs/cgroup/cpuacct`.

```
cd /sys/fs/cgroup/cpuacct
mkdir test1
cd test1
```

`ls -l` to see the files created in the test directory:

![ls -1](/img/2_7.png)

You can see the files the are in the parent group are also created in the child group.

I would like to monitor the CPU usage of the current bash shell. To do that echo the PID of the shell in to the `tasks` file.

```
echo $$ > tasks

cat tasks # To see the PID of the bash
```
![tasks file](/img/2_8.png)

To see the CPU time consumed by the bash shell, see the `cpuacct.usage`.

![cat cpuacct.usage](/img/2_9.png)

To see the CPU time consumed by user and system modes, see the `cpuacct.stat`.

![cat cpuacct.stat](/img/2_10.png)

The values of user and system in the stat file are in USER_HZ units.

There are `percpu` and `peruser` files which shows you the CPU time consumed by a single cpu (for multi-cored cpus) and CPU time consumed by a single user (if there are different users attached to the cgroup) respectively.

You can attach as many processes as you want, just echo the PID into the tasks file.

#### Persistent group configuration

The newly created cgroups will be deleted after a reboot. To have the cgroups maintained even after a reboot we need to make persistent group configuration by editing the `/etc/cgconfig.conf` file.

Syntax for creating a cgroup and adding controllers:
```
group <groupname> {
  perm {
# who can manage limits
    admin {
      uid = $USER;
      gid = $GROUP;
    }
# who can add tasks to this group
    task {
      uid = $USER;
      gid = $GROUP;
    }
  }
# create this group in cpu and memory controllers
  <controller { }>
  cpu { }
  memory { }
}
```

where,

* `groupname`  - The name of the group.
* `perm`       - Permissions (optional). This is added for controlling the rights of the group.
* `Controller` - To which controller will the group be created in (in our case the controller is cpuacct).

For example, I created a persistent group configuration for cpuacct:

```group test1 {
  perm {
    admin {
      uid = theupbeat;
      gid = theupbeat;
    }
    task {
      uid = theupbeat;
      gid = theupbeat;
    }
  }
  cpuacct { }
}
```


### Resources

1. https://wiki.archlinux.org/index.php/cgroups
2. https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/6/html/resource_management_guide/ch-using_control_groups
3. https://www.kernel.org/doc/Documentation/cgroup-v1/cpuacct.txt
4. https://sysadmincasts.com/episodes/14-introduction-to-linux-control-groups-cgroups
5. https://manpages.ubuntu.com/manpages/xenial/man5/cgconfig.conf.5.html
