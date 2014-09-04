---
published: false
---

## Small linux containers.  My comparison.

Since we sysadmins climbed from the primordial ooze of mainframes, we've been
advocating for a one-to-one mapping of services to hosts.  That makes things
like security and updates easier.

## Options for very small containers.

## Start from small base images
* [scratch](https://registry.hub.docker.com/_/scratch/)

## Build your own very small image
### from scratch
* if you start with a scratch base image, you can make an image as small as your statically linked binary - [link](http://blog.xebia.com/2014/07/04/create-the-smallest-possible-docker-container/)

### buildroot [link](http://buildroot.uclibc.org/)
Buildroot is an awesome cross-compilation toolchain designed for embedded systems.  But, it can be great for containers as well.  It eschews package management by cross compiling everything and then squashing it into one tarball.  @jpetazzo has even written a [guide](http://blog.docker.com/2013/06/create-light-weight-docker-containers-buildroot/) for building a functional postgres image that weighs in at 16 Megabytes.  

Pretty slick!
### mkimage

## Build big and then sqaush it
* [docker-squash](http://jasonwilder.com/blog/2014/08/19/squashing-docker-images/)

### Docker with tiny base images
Docker provides a few ways of building tiny images.  In the [contrib](https://github.com/docker/docker/tree/master/contrib) directory of the main docker github repository is a script called mkimage which does what it says on the tin.
```sudo ./mkimage.sh -t alovelltroy/debian debootstrap --include=ceph,quagga --variant=minbase jessie ```
They also have provided a few tiny base images to work from: