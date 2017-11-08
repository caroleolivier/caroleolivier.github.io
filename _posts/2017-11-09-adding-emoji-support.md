---
layout: post
date: 2017-11-08
title: Adding emoji support to my website
---

Today, I looked into an important problem: the lack of emoji support of my website. By that I mean writing `:+1:` should convert to :+1:. This must be fixed.

My website, the one you are on right now, is compiled by [Jekyll](https://jekyllrb.com/) and hosted on [GitHub](https://github.com/). By default this setup doesn't support emojis :scream: so it was about time :clock130: to fix it.

I followed this GitHub :octocat: [tutorial](https://help.github.com/articles/emoji-on-github-pages/) but I couldn't run my site locally anymore as I didn't have the [jemoji](https://github.com/jekyll/jemoji) plugin installed.
<br/>
This actually got me digging and looking into this other GitHub [tutorial](https://help.github.com/articles/setting-up-your-github-pages-site-locally-with-jekyll/) about setting up your GitHub Pages locally. I comtemplated following it for a while, i.e. use [Bundler](http://bundler.io/) to manage my website dependencies, but I didn't want :x: to learn about a new software today and be distracted from my main task: support emojis.

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

(Yes, I like animals, even snakes)


Cool :+1: I can now write professional looking posts :bowtie: and really convey my emotion behind each line :smiley: :sob: else how could you possibly know what I feel? 🤔

(How creepy is this emoji? :neckbeard:)

##### Notes

[🤔] isn't a "GitHub" emoji compiled by the Jemoji plugin and added as an image to the HTML. This is the thinker smiley approved as part of [Unicode 8.0](https://emojipedia.org/unicode-8.0/) in 2015. Its code name: U+1F914. If you can't see the emoji between the [ ] above, it means your browser version doesn't support the Unicode 8.0 standard. You may want to update it, you are really missing out...
