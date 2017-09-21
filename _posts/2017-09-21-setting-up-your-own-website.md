---
layout: post
title: Setting up your own website
date: 2017-09-21
---
Last week I set up my own website (finally!). Here I'll explain why I decided to use [GitHub Pages](https://pages.github.com/).

When it comes to setting up your own website, there are tons of free solutions available. You could use a blog creation tool like WordPress or Blogspot, a slightly more techie solution like a static website generator such as Hugo or Jekyll or even go bold and write your own website from scratch with bare HTML/CSS/JavaScript.

It really depends on your skills, your time and how much control you want to have over things like customisation, availability, reliability, etc...

To help me decide I wrote down a list of requirements (I will talk about it in details below), I looked around a bit and found (what I think) is the perfect solution: [GitHub Pages](https://pages.github.com/).

From Github Pages website:
> CAROLE: CHANGE BLOCKQUOTE STYLE

> GitHub Pages is a static site hosting service.

> GitHub Pages is designed to host your personal, organization, or project pages directly from a GitHub repository.

Let's have a look at my list of requirements now and why I thought GitHub Pages gave me everything I needed:

**Easy setup**

It only took me a couple of hours to get everything up and running. I used this [guide](http://jmcglone.com/guides/github-pages/) by [Jonathan McGlone](http://jmcglone.com/) I cannot recommend it enough.


**Easily add blog post content**

I like HTML/CSS for building and designing websites. However, I really dislike it as a way to manage content. What I was looking for was a solution such as [Markdown](https://en.wikipedia.org/wiki/Markdown) which is incredibly easy to use.
And the great thing with GitHub Pages is that it allows you to do exactly that.
Behind the scene it uses [Jekyll](https://jekyllrb.com/), a static web site generator that builds your website and sends it to GitHub for hosting. Among other things it allows you to write content in Markdown and include it within the HTML with just one line {% raw %}```{{ my_include | markdownify }}```{% endraw %}. Jekyll takes care of converting it into HTML for you.

**Host CV and easily maintain content**

I wanted to be able to have a modern and customisable CV but also be able to easily add content to it (new job, experience, etc...) and if needed easily be able to move it to another platform. Again, very easily feasible thanks to Jekyll in the same way as it is for blog content.

**Freedom to style**

I am not a website designer but so far I felt like I had great freedom to do whatever I wanted with my website. Jekyll even supports using [Sass](http://sass-lang.com/) (which makes your styling sheets more maintainable than pure CSS).


**Easy deployment (one-click or similar)**

Deployment is super easy. All you have to do is make a change on the master branch of your repository (by editing directly your repository on github.com, pushing changes, merging a branch). GitHub Pages takes care of compiling and deploying your website for you and so far it has been super quick to do so (about a minute or less).

**Reliable**

I don't have huge requirements in term of traffic or be able to push changes very frequently but I want my website to become the source of my professional life on the Internet so it has to be reliable: if the website goes down I am going to look bad.
So here I am blindly trusting GitHub and purely relying on their reputation (To be honest I haven't read the terms and conditions of GitHub Pages and I have no idea what their SLA are).

However, if things go bad, I can always move away from GitHub Pages and host my Jekyll generated site on AWS or Google Cloud. This being said if GitHub Pages goes down there will be more important problems to fix as the documentation of widely used libraries is hosted there.

**Version Control**

Version control is super important as it allows you to track changes and understand them, revert to a previous version (for instance if you break something), work on more than one change in isolation (thanks to branches). Once you get used to working with version control software, you can't live without it so it was very important to me. And since the website code lives in a GitHub repository it all comes out of the box and with tools I am already familiar with.

**One golden source for my Internet life**

I wanted to minimise the number of external dependencies my "Internet life" has. As a developer there are many platforms you may want to be on: GitHub, Twitter, LinkedIn and potentially Facebook, Instagram, and probably others I forgot or don't know anything about. They all serve different purpose but it is lot to manage (for me!) and I wanted to keep things simple.

So GitHub Pages seemed like a very good one-for-all solution **for me**: I can write blog posts, host my CV, publish my portfolio, potentially have an RSS feed and push daily "thoughts" to my home page. No need to maintain billions of accounts.

 <br />

And this is why I decided to host my website on GitHub Pages, voilà ! 