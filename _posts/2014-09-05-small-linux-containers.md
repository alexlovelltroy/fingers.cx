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

# Small linux containers.  My comparison.

Since we sysadmins climbed from the primordial ooze of mainframes, we've been
advocating for a one-to-one mapping of services to hosts.  That makes things
like security and updates easier.

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

