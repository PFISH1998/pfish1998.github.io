---
title: DockerNote
date: 2019-03-17 16:56:07
tags: notes

---

关于Docker的一些东西

<!--more-->

# Docker 原理和一些操作笔记 

## Docker 的原理

Docker 核心是利用 LXC(Linux Container) 来实现类似 VM 的功能，从而更加高效的利用硬件资源。与 VM 不同的，LXC 并不是一套硬件虚拟化方法，而是一个操作系统级的虚拟化方法。

它主要利用了 LXC 的 container，namespace, cgroup 等技术来实现与 VM 相似的隔离性和对资源的调配管理等功能。在 LXC 的基础上进行了进一步的封装，让用户不需要过多关心容器的管理，使得操作更加方便。


### Linux Container (LXC)

LXC 是一种轻量级的虚拟化手段。可以提供一个共享Kernel的轻量级的虚拟化，以便隔离进程和资源。有效的将单个操作系统管理的资源划分到各个孤立的组中。与传统虚拟技术相比，它的优势在于：

 1. 与宿主机使用用一个内核。
 2. 不需要指令级模拟。
 3. 容器可以在CPU核心的本地运行指令，不需要解释机制。
 4. 避免了准虚拟化和系统调用中的复杂性。
 5. 轻量级隔离，隔离的同时还提供共享机制，以实现容器与宿主机的资源共享。

### Linux Namespace

LXC 所实现的隔离性主要是来自于 kernel 的 Namespace.

Namespace 可以有效的帮助 Docker 将进程树、网络接口、消息、挂载点以及进程中的通信资源等隔离开。

#### pid namespace

可以将不同的进程隔离开，在不同的 namespace 中可以有相同的 pid。并且 namespace 允许嵌套，父 namespace 可以影响子 namespace 进程。

所有的 LXC 进程在 docker 中的进程的父进程为 docker 进程。

#### net namespace

拥有独立的 network devices, IP address, IP routing tables, /proc/net 目录。这样每个 container 的网络就能隔离开。

LXC 在此基础上有5中网络类型，docker 默认采用 veth 的方式将 container 中的虚拟网卡同 host 上的一个 docker bridge 连接在一起。

#### ipc namespace

container 中进程交互还是采用 Linux 常见的进程间交互方法(interprocess communication - IPC), 包括常见的信号量、消息队列和共享内存

#### mnt namespace

类似与chroot，将一个进程放到一个特定的目录执行。 mnt namespace 允许不同namespace 的进程看到的文件目录不同，这样每个 namespace 中的进程所看到的文件目录就被隔离开了。

#### uts namespace

UTS(UNIX Time-sharing Systerm) namespace 允许每个 container 拥有独立的 hostname 和 domain name, 使其在网络上可以被视为一个独立的节点而非 Host 上的一个进程。

