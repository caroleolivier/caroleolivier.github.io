---
layout: post
title: What's a Docker image?
date: 2022-07-27
---

## TL/DR

A Docker image is basically a file system.

And this file system is cleverly assembled by merging chunk of file systems called layers.

## In practice

In the Dockerfile, each different command generates a new layer (part of the file system).

For instance, the example below will generate a file system with 2 files: `file1.txt` and `file2.txt`.

```docker
FROM scratch

COPY file1.txt file1.txt # --> 1 layer with file1.txt

COPY file2.txt file2.txt # --> 1 layer with file2.txt
```

The [scratch image](https://hub.docker.com/_/scratch) is the smallest layer you can start with.  
It must be there.  
It's an empty file system.

There are hello-world examples [here](https://github.com/docker-library/hello-world).  


## Interesting commands

### Listing layers

```
$ docker history image_name
```

Literally lists all the layers that are part of the image.

### Unpacking an image

```
$ sudo docker image save ubuntu > docker_ubuntu.tar
$ tar -xvf docker_ubuntu.tar
```

You'll need to then tar the layer and you'll see it's a bunch of files.

Note that for multi layers images, you'll have to tar each layer individually.


### Inspecting an image

```
$ docker inspect image_name
```

It gives a bunch of info about an image.

One info is where the layers for the images are stored on the host so you can explore the content from there too.


Soooo that demystifies what a Docker image is: a file system :)
