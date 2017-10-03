---
layout: post
title: Continuous Integration in GitHub
date: 2017-10-03
---

I have been working on a project for the last couple of days and I decided it was about time I set up some kind of Continuous Integration tool to my repo in GitHub. I will describe here how you can achieve that in a few minutes with Travis CI.

First of all, what is Continuous Integration?

> Continuous integration is a DevOps software development practice where developers regularly merge their code changes into a central repository, after which automated builds and tests are run. Continuous integration most often refers to the build or integration stage of the software release process and entails both an automation component (e.g. a CI or build service) and a cultural component (e.g. learning to integrate frequently). The key goals of continuous integration are to find and address bugs quicker, improve software quality, and reduce the time it takes to validate and release new software updates.
<br />
> [AWS](https://aws.amazon.com/devops/continuous-integration/)


It turns out there are a lot of CI tools that integrate with GitHub, you can find a non-exhaustive list [here](https://github.com/marketplace/category/continuous-integration). I was looking for a free solution and found this list [here](https://github.com/ligurio/Continuous-Integration-services/blob/master/continuous-integration-services-list.md) maintained by the GitHub community. I spotted [Travis](https://travis-ci.org) in the list. I am not familiar with Travis at all (I have used [Jenkins](https://jenkins.io/) and [TeamCity](https://www.jetbrains.com/teamcity/) in the past) but I have seen it being used quite a lot on GitHub so I thought I would give it a try.

#### Setting up Travis

Setting up Travis for my application took me literally 5 minutes, it was incredibly quick. All I had to do was to follow the guide [here](https://docs.travis-ci.com/user/getting-started/) which I can summarise in my case by:

1. Log in to Travis using my GitHub account.
2. Activate Travis-CI for my repository.
3. Add [.travis.yml](https://github.com/caroleolivier/minimal-react-starter/blob/v3.0.0/.travis.yml) file to my repository.

Done!

I now have a CI tool set up for my [project](https://github.com/caroleolivier/minimal-react-starter/tree/v3.0.0) and can monitor the status on the [Build Status page](https://travis-ci.org/caroleolivier/minimal-react-starter) (as well as being notified automatically by email) so cool!


#### Worth knowing about Travis

_Build lifecycle_

I found that [documentation page](https://docs.travis-ci.com/user/customizing-the-build#The-Build-Lifecycle) very useful to understand what Travis is doing when it builds and tests. This will certainly become handy later when/if my build becomes more complex.

_Build Status icon_

You can add the status of your build to any markdown file as described [here](https://docs.travis-ci.com/user/status-images/). This is the current status of my project [![Build Status](https://travis-ci.org/caroleolivier/minimal-react-starter.svg?branch=master)](https://travis-ci.org/caroleolivier/minimal-react-starter) hopefully it is green ^^

_Linter for .travis.yml_

Travis has developed an [online linter](http://lint.travis-ci.org/) very useful if you have syntax errors and can't spot them (mind the extra space...).

_Build Matrix_

I haven't used the [matrix feature](https://docs.travis-ci.com/user/customizing-the-build/#Build-Matrix) yet but I think it is worth mentioning as it seems quite powerful. In my case I may use it later to build the client and server sides of the same application that live in the same repository. One solution is described in this [article](https://lord.io/blog/2014/travis-multiple-subdirs/).
