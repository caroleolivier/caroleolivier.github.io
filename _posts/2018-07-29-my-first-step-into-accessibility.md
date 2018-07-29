---
layout: post
title: My first step into accessibility
date: 2018-07-29
---

It's been ages since I last wrote anything on my blog. It's not because I haven't learnt or done anything interesting, quite the opposite! Since I joined [Bulb](https://bulb.co.uk/) in December, I have been constantly learning, it's been a great start. But I haven't made room for writing :(
However, I have decided to remedy to this as for the next two to three months (at least), I will be spending some time focusing on accessible web content. My ambition is to write a quick report each week. List what I have learnt, done, problems I have faced, etc... In short, I'll be keeping a sort of diary I can go back to later.

And so it starts, this is week 1!

### What is accessibility?

> Accessibility refers to the design of products, devices, services, or environments for people who experience disabilities.
> [Wikipedia](https://en.wikipedia.org/wiki/Accessibility)

In my diary, I will refer to accessibility to mean *accessible web content*. This is what I want to be better at. I want to build websites that are accessible to as many people as I can, giving the available technologies.

I am lucky enough to work for a company for which accessibility is a priority, so I intend to make the most of it. This is a very exciting!

### The basic

Here are some very basic things I have learnt this week:

##### Screen readers

I have learnt how to properly use the screen reader on my MacBook (it's called Voice Over). There is a great tutorial that comes with it. There are a few screen readers technologies, the main ones are:
* Voice Over (built-in on Mac)
<br/>Great basic tutorial [here](https://www.youtube.com/watch?v=5R-6WvAihms&index=25&list=PLNYkxOF6rcICWx0C9LVWWVqvHlYJyqw7g&t=296s).
* NVDA (free software that can be installed on Windows)
<br/>Great basic tutorial [here](https://www.youtube.com/watch?v=Jao3s_CwdRU&index=23&list=PLNYkxOF6rcICWx0C9LVWWVqvHlYJyqw7g&t=0s)
* JAWS (Job Access With Speech)
<br/>(A tricky one to google if, like me, you are scared of sharks)
<br/>You can see a user using JAWS [here](https://www.youtube.com/watch?v=eOdjjRA9vBw&feature=youtu.be)

##### Semantic HTML

Using semantic HTML is crucial. It basically means that you should be using HTML element for what they are for and not disguise them for something they are not. For a button use an HTML button, not a div disguised as a button.
<br/>
This is a great [video](https://www.youtube.com/watch?v=CZGqnp06DnI&index=27&list=PLNYkxOF6rcICWx0C9LVWWVqvHlYJyqw7g&t=0s) about it.


##### Focus

Focus is crucial.
First, because some people cannot use a mouse (arthritis, broken arms, etc...). But also, this is really important for screen readers. If a button cannot be focused, many would never be able to click it!


##### Accessibility Tree

When you build a web page, think **Accessibility Tree**. The accessibility tree is the tree built by the browser and sent as input to the screen reader. So when building a web page, think about the accessibility tree first. Then if you want to visually improve your page, you can play with CSS.
<br/>
This [video](https://www.youtube.com/watch?v=z8xUCzToff8) has some great intro information about it (at around 5 minutes). You can couple it with this [article](https://developers.google.com/web/fundamentals/accessibility/semantics-builtin/the-accessibility-tree). I like what they say about the accessibility tree:
> You might visualize the accessibility tree as looking a bit like an old web page from the '90s: a few images, lots of links, perhaps a field and a button.

😀 (<= I wonder how accessible this smiley is! On my list of things to check)


### Resources I highly recommend

* [Accessibility Fundamentals](https://www.youtube.com/watch?v=z8xUCzToff8) by Rob Dodson
* [Accessibility video series](https://www.youtube.com/playlist?list=PLNYkxOF6rcICWx0C9LVWWVqvHlYJyqw7g) by Rob Dodson
<br/>If you have been clicking on the videos, I have been listing, you have probably noticed a lot of them come from this series. I am halfway in and I am really enjoying the journey. They are short, to the point and very easy to digest.
* [Inclusivity](https://medium.com/zendesk-creative-blog/designing-for-inclusivity-how-and-why-to-get-started-f52a1792e4fd)
* [Inclusive design](https://24ways.org/2016/what-the-heck-is-inclusive-design/)
* More [resources](http://design.bulb.co.uk/#/Accessibility/Automated%20Testing.md) from Bulb


### What's next?

Next week I'll carry on reading and watching videos. I will also make a start into making a page or a component more accessible. I may do this on this website or at work, but it is time to get some practical experience now!