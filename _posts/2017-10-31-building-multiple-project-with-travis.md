---
layout: post
title: Building multiple projects with Travis
date: 2017-10-31
---

A few weeks ago, I wrote a [post](https://caroleolivier.github.io/blog/2017/10/03/continuous-integration-in-github) about using [Travis](https://travis-ci.org/) as Continuous Integration tool for some of my repositories in [GitHub](https://github.com/). I briefly mentioned the [Build Matrix](https://docs.travis-ci.com/user/customizing-the-build/#Build-Matrix) feature, anticipating it will come handy in the future. And this day has come ^^


I am currently writing an application composed of a web application and a data api. I decided to put all the code in one repo (one could argue I should have put them in different repo but this is not the subject of this post). So the obvious question is how do I build both applications using Travis? It turns out it is super easy thanks to Build Matrix and this life saving StackOverflow [post](https://stackoverflow.com/questions/27644586/how-to-set-up-travis-ci-with-multiple-languages).
<br/>
My repo contains two folders: `web-app` for the web application (a classic JavaScript web application) and `data-api` for the service API (using [ASP.NET Core](https://docs.microsoft.com/en-us/aspnet/core/)). And this is Travis config file:

```
// .travis.yml
matrix:
  include:
    - language: node_js
      node_js:
        - "node"
      before_install:
        - cd web-app
    - language: csharp
      mono: none
      dotnet: 2.0.0
      before_install:
        - cd data-api
      script:
        - dotnet restore
        - dotnet build
```
Note that [Travis linter](https://lint.travis-ci.org/) can't parse the code and returns a lot of errors. However, it works perfectly as you can see here:

![Travis Build Matrix]({{ "/assets/blog/travis_build_matrix.png" | absolute_url }}){:height="80%" width="80%"}

This is pretty cool!


