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
