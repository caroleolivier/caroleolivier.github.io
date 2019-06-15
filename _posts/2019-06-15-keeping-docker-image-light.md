---
layout: post
title: Keeping your Docker 🐳 image light 
date: 2019-06-15
---

I was recently setting up a new project and noticed I was building a rather big [Docker](https://www.docker.com) image.
It contained a lot of stuff and a lot was unnecessary for runtime (e.g. source code 😬).
<br/>
I took a look to see how I could reduce it to the bare minimum. And this is how I came across [Docker multi-stage builds](https://docs.docker.com/develop/develop-images/multistage-build/).


## Context

The project is a [node.JS](https://nodejs.org/en/) application using [yarn](https://yarnpkg.com) for managing dependencies.
<br/>
The ideal Docker image should contain:
* The transpiled code: `lib`
* The dependencies: `node_modules`
* The metadata: `package.json`

At least this is what I think is needed!

## Challenge 1: Node dependencies

At first I thought I could build the code (locally or on the build server) and then copy `lib`, `node_modules` and `package.json` in the image. So basically have something like:

```bash
    # Dockerfile
    FROM node:10
    WORKDIR /code

    COPY lib /code/lib
    COPY node_modules /code/node_modules
    COPY package.json /code

    CMD ["node", "lib/index.js"] 
```

The problem with this approach comes with installing dependencies.
<br/>
Some dependencies contain system specific libraries. I had this problem with [@google-cloud/pubsub](https://github.com/googleapis/nodejs-pubsub). It relies on [GRPC](https://grpc.io/) which would install a MacOS specific library not compatible with the Linux distribution of my base Docker image.
<br/>
So I would install the dependencies locally, build the code locally, build the docker image and it would fail at runtime because of an incorrect/missing library (I don't have the full error anymore, but hopefully you get the idea).

So lesson learnt from this: install your dependency inside your Docker image, it is safer.

```bash
    # Dockerfile
    FROM node:10
    WORKDIR /code

    # ...
    COPY package.json /code/
    RUN yarn install
    # ...

    CMD ["node", "lib/index.js"] 
```


## Challenge 2: Building the code

Once the dependencies are installed, we can build the code (i.e. transpile, minify, etc...).
<br/>
This means the source code needs to be copied over.

```bash
    # Dockerfile
    FROM node:10
    WORKDIR /code

    COPY . /code
    RUN yarn install

    RUN yarn build

    CMD ["node", "lib/index.js"] 
```

<br/>
This is fine, but I don't want it to stay inside the Docker image. It is of no use and needs to be removed.
<br/>
However, when I tried this approach, I realised I had to remove a looooooot of files. So my Dockerfile ended up looking something like:

```bash
    # Dockerfile
    FROM node:10
    WORKDIR /code

    COPY . /code
    RUN yarn install

    RUN yarn build

    RUN rm -rf src
    RUN rm file1
    RUN rm file2
    RUN rm file3
    # ...

    CMD ["node", "lib/index.js"] 
```

So not very nice.
<br/>
I am sure I could have found more elegant ways around this (like delete everything but `lib`, `node_modules` and `packages.json` or copy just the files needed for building the code and delete them) but it still didn't feel right.
<br/>
After a bit of digging I came across [Docker multi-stage builds](https://docs.docker.com/develop/develop-images/multistage-build/).


## Solution: Docker multi-stage builds

This is a really nice feature that allows to build code and throw away what's not needed super easily.
<br/>
Basically you first build your code, then create a super clean Docker image by copying over exactly what you need.
<br/>
I won't go into details because the doc is very clear and I recommend reading it. But this is what my final Dockerfile looked like:

```bash
    # Dockerfile
    # 1) build the code
    FROM node:10 as builder
    WORKDIR /code

    COPY . /code
    RUN yarn install

    RUN yarn build

    # 2) get new base layer and copy over what's needed from previous layer
    FROM node:10
    WORKDIR /code

    COPY --from=builder /code/package.json .
    COPY --from=builder /code/lib ./lib
    COPY --from=builder /code/node_modules ./node_modules

    CMD ["node", "lib/index.js"] 
```

<br/>
Here we go, super clean Docker image 🥳