---
layout: post
title: Docker Containerisation with Travis
date: 2017-11-06
---

A few days ago, I described [here](https://caroleolivier.github.io/blog/2017/11/04/dockerising-aspnet-core-app) how to package an [ASP.NET Core](https://docs.microsoft.com/en-us/aspnet/core/) application into a [Docker](https://www.docker.com/) container. Today, I will show how I integrated it with my Continous Integration workflow with [Travis](https://travis-ci.org/).

##### Building Docker images

[Travis supports Docker](https://docs.travis-ci.com/user/docker/) so it is very straightforward to build a Docker image with it. All you need to do is add Docker as a service dependency and add the Docker build command to the script section.

```yaml
- language: csharp
  sudo: required
  services:
  - docker
  mono: none
  dotnet: 2.0.0
  before_install:
  - cd path/to/slnfile
  script:
    - dotnet restore
    - dotnet build
    - dotnet publish -o pack
    - docker build -t mydockerimagename:$TRAVIS_COMMIT_NUMBER .
```

##### Pushing Docker images

One thing I haven't talked about yet is where my Docker image should be stored. Ultimately I want to be able to deploy my application so the Docker image needs to be available somewhere.
<br/>
The best (and simplest) way to store Docker images is to use a [Docker Registry](https://docs.docker.com/registry/), a service dedicated to storing Docker images and expose metadata about them. There are plenty of Docker Registry providers available. Most of the Cloud providers (probably all really) offer Docker Registries. I didn't do any extensive research around which one is the best, I picked one that seemed easy to use: [Docker Hub](https://hub.docker.com/). It has a nice web interface and is free (see [here](https://hub.docker.com/r/microsoft/aspnetcore/) for intance).
<br/>
I opened an account and created a [Docker Repository](https://docs.docker.com/glossary/?term=repository) for my images. A Docker repository is a collection of images with the same name but different tags. Typically, there is one registry per application and as many images as there are versions.
<br/>
Once my repository created, it was very easy to push images to it and to add the command to my Travis configuration file:
```yaml
...
  script:
    ....
    - docker build -t mydockerimagename:$TRAVIS_COMMIT_NUMBER .
    - docker push mydockerimagename:$TRAVIS_COMMIT_NUMBER
```

Actually, that's not quite everything. I had to make a few more changes to authenticate with Docker Hub because my repository was private. (Even if your repository is public you may still have to authenticate)


##### Adding credentials to Travis

As I said before, I couldn't just run `docker push`, I needed to provide my credentials and run `docker push --username myUsername --password myPassword`. This meant adding my credentials to .travis.yml.
<br/>
Luckily, Travis supports the encryption of variables so it was very easy to solve this problem. Please refer to Travis [documentation](https://docs.travis-ci.com/user/environment-variables/#Defining-encrypted-variables-in-.travis.yml) to know how to encrypt variables.


##### Final Travis configuration

And this is my final Travis configuration (or similar):
```yaml
- language: csharp
  sudo: required
  services:
    - docker
  mono: none
  dotnet: 2.0.0
  env:
    # DOCKER_USER
    - secure: "....."
    # DOCKER_PWD
    - secure: "...."
  before_install:
    - cd path/to/slnfile
  script:
    - dotnet restore
    - dotnet build
    - dotnet publish -o pack
    - docker login -u $DOCKER_USER -p $DOCKER_PWD
    - docker build -t $DOCKER_USER/repositoryName:$TRAVIS_COMMIT_NUMBER .
    - docker push $DOCKER_USER/repositoryName:$TRAVIS_COMMIT_NUMBER
```

So now, every time I push new code to my GitHub repository, Travis picks up the changes, builds the code, creates a Docker image and then publishes it to Docker Hub. This is really great but the last bit is missing: the deployment. This is what I will be looking at next.