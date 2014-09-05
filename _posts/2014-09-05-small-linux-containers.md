---
layout: post
# headerstyle: ishtar
# headerstyle: alps
# headerstyle: bench
# headerstyle: door
# headerstyle: eagle
# headerstyle: flower
# headerstyle: hawk
# headerstyle: home
# headerstyle: ishtar
# headerstyle: midgets
# headerstyle: mountain
# headerstyle: network
# headerstyle: plates
# headerstyle: presidencia
# headerstyle: statue
# headerstyle: unicorn
# headerstyle: yoda
# headerstyle: martenitsa
# headerstyle: cloud
headerstyle: container
title: very small linux containers
date: 2014-09-05
tags: programming, devops, containers, docker
author: Alex Lovell-Troy
---

# These are my notes, but you can follow along.

Since we sysadmins climbed from the primordial ooze of mainframes, we've been
advocating for a one-to-one mapping of services to hosts.  That makes things
like security and updates easier.  The problem is that computers are
fantastically fast.  When they're only doing one thing, that generally means
that they spend most of their time sitting around doing nothing.  To take
advantage of that time doing nothing, modern computers run hundreds of programs
at once.  A kernel manages access to resources like memory and networking and
shares them with all the running programs, mostly equally.

## Virtualization versus Containerization

When two programs running on the same computer see the same kernel, but can't see each other, that's containerization.

When two programs running on the same computer see different kernels, and thus can't see each other, that's virtualization.

Virtualization gives you fully separate computers which is great for isolation and security, adds a negligible performance penalty, but also multiplies the number of virtual computers to manage with security patches and configuration changes.

Containerization relies on the underlying kernel to keep some things separate, but allows some shared access.  There's only one real computer to manage.

## A little history

Solaris had [Zones](https://en.wikipedia.org/wiki/Solaris_Containers) in 2004. FreeBSD had [Jails](https://wiki.freebsd.org/Jails) from version 4.0 which was released in 2000.  Linux went the route of full emulation and did some cool stuff with [QEMU](http://wiki.qemu.org/Main_Page), [KVM](http://www.linux-kvm.org/page/Main_Page), and the always mysterious [XEN](http://wiki.xenproject.org/wiki/Xen_Overview#What_is_the_Xen_Project_Hypervisor.3F) which isn't really Linux, but often used for Linux virtualization.



# You can build small images

## Start from small base images
* [scratch](https://registry.hub.docker.com/_/scratch/)

## Build your own very small image

### from scratch
* if you start with a scratch base image, you can make an image as small as your statically linked binary - [link](http://blog.xebia.com/2014/07/04/create-the-smallest-possible-docker-container/)

### buildroot [link](http://buildroot.uclibc.org/)
Buildroot is an awesome cross-compilation toolchain designed for embedded systems.  But, it can be great for containers as well.  It eschews package management by cross compiling everything and then squashing it into one tarball.  @jpetazzo has even written a [guide](http://blog.docker.com/2013/06/create-light-weight-docker-containers-buildroot/) for building a functional postgres image that weighs in at 16 Megabytes.  

Pretty slick!

### mkimage
In the [contrib](https://github.com/docker/docker/tree/master/contrib) directory of the main docker github repository is a script called mkimage which does what it says on the tin.
```sudo ./mkimage.sh -t alovelltroy/debian debootstrap --include=ceph,quagga --variant=minbase jessie ```

## Build big and then sqaush it
* [docker-squash](http://jasonwilder.com/blog/2014/08/19/squashing-docker-images/)

# Kernel Namespaces
* [systemd-nspawn](http://rich0gentoo.wordpress.com/2014/07/14/quick-systemd-nspawn-guide/)

