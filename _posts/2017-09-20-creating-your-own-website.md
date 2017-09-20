---
layout: post
title: Creating your own website
date: 2017-09-20
---
Last week I set up my own website (finally!). Here I'll explain why I decided to use [GitHub Pages](https://pages.github.com/) functionality and how I did it.

When it comes to setting up your own website, there are tons of free solutions available. You could use a blog creation tool like WordPress or Blogspot, a slightly more techie solution like a static website generator such as Hugo or Jekyll or even go bold and write your own website with bare HTML/CSS/JavaScript.

It really depends on your skills, your time and how much control you want to have over things like customisation, availability, reliability, etc...

I made a list of reuqirements (I will talk about it in details below), I looked around a bit and found (what I think) is the perfect solution: [GitHub Pages](https://pages.github.com/). From Github Pages website:
> CAROLE: CHANGE BLOCKQUOTE STYLE

> GitHub Pages is a static site hosting service.

> GitHub Pages is designed to host your personal, organization, or project pages directly from a GitHub repository.

Let's have a look at my list of requirements now and why I think GitHub Pages gives me everything I wanted:

**Easy setup**

It only took me a couple of hours to get everything up and running. I used this [guide](http://jmcglone.com/guides/github-pages/) by [Jonathan McGlone](http://jmcglone.com/) I cannot recommend it enough.


**Easily add blog post content**

I like HTML/CSS when it comes to style a website. I really dislike it when adding pure content and one thing I wanted to avoid at any price was to manage my website content with HTML/CSS. Ideally I wanted to use [Markdown](https://en.wikipedia.org/wiki/Markdown), which I am already familiar with or a language as easy to learn and use. And this is exactly what [Jekyll](https://jekyllrb.com/) allows users to do. You can write content in Markdown and include within the HTML with just one line {% raw %}```{{ my_include | markdownify }}```{% endraw %}. Jekyll takes care of converting it into HTML for you.

**Possibility to add CV and easily maintain the content**

I wanted to be able to have a modern and customisable CV but also be able to easily add content to it (new job, experience, etc...) and if needed easily be able to move it to another platform. Again, very easily feasible thanks to Jekyll.

**Freedom to style**

I am not a website designer but so far I felt like I had great freedowm to do whatever I wanted with my website. Jekyll even supports [Sass](http://sass-lang.com/).


**Easy deployment (one-click or similar)**

Deployment is super easy. All you have to do is make a change on master (by editing directly on the website, pushing changes, merging a branch). GitHub Pages takes care of compiling and deploying your website for you and so far it has been super quick to do so, a minute or so.

**Reliable**

I don't have huge requirements in term of traffic or be able to push changes very frequently but I want my website to become the source of my professional life on the Internet so it has to be reliable: if the website goes down I am going to look bad.
I am going to be honest here, I haven't read the conditions of use, I am purely relying on GitHub reputation.

However, if things go bad, I can always move away from GitHub Pages and host my Jekyll generated site on AWS or Google Cloud. This being said if GitHub Pages goes down we will see more important problems as the documentation of widely used libraries is hosted there.

**Version Control**

Version control is super important as it allows you to track changes and understand them, revert to a previous version (for instance if you break something), work on more than one change in isolation (thanks to branch). Once you get used to work with version control software, you can't live without it.

**One golden source for my Internet life**

I wanted to minimise the number of external dependencies my "Internal life" had. As a developer there are many platforms you want  to be on: GitHub, Twitter, LinkedIn and potentially Facebook, Instagram, and others I forgot or don't know anything about. They all serve different purpose but it is lot to manage (for me!) and I wanted to keep things simple.

So GitHub Pages seemed like a very good one-for-all solution **for me**: I can write blog posts, host my CV, publish my portfolio, potentially have an RSS feed and push daily "thoughts" to my home page.


And this is why I decided to host my website on GitHub Pages, voilà ! 