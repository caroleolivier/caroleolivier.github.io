---
layout: post
title: Container from scratch
date: 2022-07-28
---

This brilliant [video](https://www.youtube.com/watch?v=8fi7uSYlOdc) basically shows the principles behind the creation of containers.  
The speaker is [Liz Rice](https://www.lizrice.com/).  
I'm super thankful to her for teaching me about containers via this content.

## What I did

I went through the video and replicated what she did.  
It took me a little while (~3 days including distractions like faffing with my Linux), lots of pausing and readings.  
But I learnt a lot and this post is a collection of learnings and notes I made.

## Pre-requisites

* Use Linux.
* Install Docker following [docs.docker.com/engine/install/ubuntu/](https://docs.docker.com/engine/install/ubuntu/)
* Install Go following [go.dev/doc/install](https://go.dev/doc/install)

## What actually is a container?

A container is basically a process that:
1. can't see everything on the host thanks to Linux **Namespaces**
2. can't access all resources thanks to Linux **Cgroups**
3. has a mounted file system with all it needs

Hence how the process is contained.

### Namespaces

From the video:

> Namespaces limit what a process can see.

From Linux man [page](https://man7.org/linux/man-pages/man7/namespaces.7.html)

> A namespace wraps a global system resource in an abstraction that
>       makes it appear to the processes within the namespace that they
>       have their own isolated instance of the global resource.  Changes
>       to the global resource are visible to other processes that are
>       members of the namespace, but are invisible to other processes.
>       One use of namespaces is to implement containers.

Examples of resources include:
* hostname
* process ids
* mounts

My main takeaways from the video on Namespaces are that when you create a new process:

1. You can spawn it in a new namespace using [CLONE_NEWUTS](https://man7.org/linux/man-pages/man7/uts_namespaces.7.html)  
This allows you to get a new hostname independent of the host hostname.

2. You can set it in a new PID namespace by using the flag [CLONE_NEWPID](https://man7.org/linux/man-pages/man2/clone.2.html#:~:text=same%20clone%20call.-,CLONE_NEWPID,-(since%20Linux%202.6.24))  
That's how processes in the new namespace start at 1.

3. You can create a new mount namespace by using the flag [CLONE_NEWNS](https://man7.org/linux/man-pages/man7/mount_namespaces.7.html)  
This allows to isolate the list of processes displayed by `ps` when `proc` is mounted.


And that's basically the idea for creating an isolated process, i.e. a container.

There are more resources to cover, including limiting network interfaces, user group ids and they all work in the same way.

### chroot

[chroot](https://man7.org/linux/man-pages/man2/chroot.2.html) is super important as it allows to change the root directory of the calling process.

This means that by calling `chroot` one can change what file system the process sees/uses.

And in Docker, this new root points to the Docker image (e.g. file system) of the Docker container.

### Cgroups

From the video:

> Cgroups limit what a process can use.

From Linux man [page](https://man7.org/linux/man-pages/man7/cgroups.7.html)

> Control groups, usually referred to as cgroups, are a Linux
>       kernel feature which allow processes to be organized into
>       hierarchical groups whose usage of various types of resources can
>       then be limited and monitored.  The kernel's cgroup interface is
>       provided through a pseudo-filesystem called cgroupfs.  Grouping
>       is implemented in the core cgroup kernel code, while resource
>       tracking and limits are implemented in a set of per-resource-type
>       subsystems (memory, CPU, and so on)


So basically to limit resources a process can use, you need to:
1. Create a *cgroup*  
This means creating a directory under `/sys/fs/cgroup/system.slice/` called whatever you want to call your cgroup.  

2. Add the limits  
This means add files containing the limits.  
For instance `memory.max` to limit memory

3. Assign the PID of the process to the cgroup  
This means adding the PID number to the file `cgroup.procs` inside the cgroup folder

This is an edited snippet code extract from the video that limits the number of processes that can run inside the container to 20:

```go
func cg() {
    cgroups := "/sys/fs/cgroup/"
    pids := filepath.Join(cgroups, "pids")
    os.Mkdir(filepath.Join(pids, "liz"), 0755)
    ioutil.WriteFile(filepath.Join(pids, "liz/pids.max"), []byte("20"), 0700)
}
```

Source code on [GitHub](https://github.com/lizrice/containers-from-scratch/blob/master/main.go).


#### ⚠️ cgroup v1 vs v2

The video shows the usage of cgroup v1 but nowadays v2 is used.  
Some info about v1 vs v2 in the man page [here](https://man7.org/linux/man-pages/man7/cgroups.7.html#:~:text=by%20descendant%20cgroups.-,Cgroups%20version%201%20and%20version%202,-The%20initial%20release)

But exploring the `/sys/fs/cgroup` is pretty similar.

If you run this to restrict the container resource to 500 mbytes.
```
$ docker run -ti -m=500 ubuntu /bin/bash
```

You'll see that a new cgroup called something like `docker-containerid` will appear under `/sys/fs/cgroup/system.slice/` and will have the 500mbytes memory limit specified in the command.


## Summary

So a container is just a process that can only see and use certain resources on the host.

This is made possible by Linux Namespaces, chroot and Cgroups.



And Docker is one implementation of containerisation 🐋