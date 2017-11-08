---
layout: post
date: 2017-11-08
title: Adding emoji support to my website
---

Today, I looked into an important problem: the lack of support for GitHub like emojis in my website. 

My website, the one you are on right now, is compiled by [Jekyll](https://jekyllrb.com/) and hosted on [GitHub](https://github.com/). By default this setup doesn't support GitHub like emojis in Markdown files :scream:. By that I mean that writing `:+1:` doesn't automatically convert to :+1:.
<br/>It was about time :clock130: I fix that problem.

##### Emojis Unicode

Bear in mind that it is still possible to use emojis by directly adding the unicode character to the text, e.g. [🔥]. If you don't see a fire between the [] it means your browser (or whatever you are using to view this text) doesn't support [Unicode 6.0](https://emojipedia.org/unicode-6.0/). However, it is not very easy to use, unless you have an emoji keyboard or know the emoji unicode table  by heart :heart:.


##### GitHub emojis

I followed this GitHub :octocat: [tutorial](https://help.github.com/articles/emoji-on-github-pages/) but I couldn't run my site locally anymore as I didn't have the [jemoji](https://github.com/jekyll/jemoji) plugin installed.
<br/>
This actually got me digging and looking into this other GitHub [tutorial](https://help.github.com/articles/setting-up-your-github-pages-site-locally-with-jekyll/) about setting up your GitHub Pages locally. I comtemplated following it for a while, i.e. use [Bundler](http://bundler.io/) to manage my website dependencies, but I didn't want :x: to learn about a new software today and be distracted from my main task: GitHub emojis support.

So I simply used [gem](https://rubygems.org/) to install the missing plugin `jemoji` and that was it!

:cat:
:dog:
:mouse:
:hamster:
:rabbit:
:wolf:
:frog:
:tiger:
:koala:
:bear:
:pig:
:pig_nose:
:cow:
:boar:
:monkey_face:
:monkey:
:horse:
:racehorse:
:camel:
:sheep:
:elephant:
:panda_face:
:snake:
:bird:
:baby_chick:
:hatched_chick:
:hatching_chick:
:chicken:
:penguin:

I haven't looked at the source code of jemoji, but inspecting my webpage after "compilation", it looks like jemoji transforms strings like `:elephant:` to HTML image elements.
```
:elephant:
```
becomes
```html
<img class="emoji" title=":elephant:" alt=":elephant:"
src="https://assets-cdn.github.com/images/icons/emoji/unicode/1f418.png" height="20" width="20">
```
So adding 30 or so emojis like I did above result into 30 (very important) HTTP calls :phone: over the network.


##### Conclusion

This is really cool :+1: I can now write professional looking posts :bowtie: and really convey my emotions behind each line :smiley: :sob: else how could you possibly know how I feel? 🤔 (<= not a GitHub emoji yet :cry:)

(How creepy is this emoji? :neckbeard:)


##### Disclaimer

I needed a break today, it happens to everyone.
