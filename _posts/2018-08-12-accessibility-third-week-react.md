---
layout: post
title: Accessibility week 3 - React
date: 2018-08-12
---

As I was testing the accessibility of a page this week, I noticed something weird: an `h2` element on the page was being read: _heading level 2 3 items Previous electricity reading_ (with quite a long pause between the words in Previous electricity reading).
After some digging, I realise it was because of [React](https://reactjs.org/). In this post I'll describe what I have learnt about accessibility and React while investigating this problem.

#### The problem

This is the h2 element I am interesting in:

![Accessibility tree]({{ "/assets/blog/meterreading_h2.png" | absolute_url }}){:height="40%" width="40%"}

Let's take a look at the accessibility tree generated for this element:

![Accessibility tree]({{ "/assets/blog/meterreading_accessibility_tree.png" | absolute_url }}){:height="40%" width="40%"}

Based on this, it makes total sense that the screen reader read out _heading level 2 3 items Previous  electricity  reading_.

Now, how did we end up in this situation? This is what the HTML looks like:
```html
  <h2>
    <span>
    <!-- react-text: 225 -->
    Previous 
    <!-- /react-text -->
    <!-- react-text: 226 -->
    electricity
    <!-- /react-text -->
    <!-- react-text: 227 -->
     reading 
    <!-- /react-text -->
    </span>
  </h2>
```

There are two noticeable things:
* The HTML span elements contains 3 items
* It looks like it has something to do with React based on the weird comments we can see in the DOM.


#### A little detour: what's a span?

The `span` isn't a problem but one may wonder why there is a `span` in the DOM. It doesn't seem to do anything, so it got me curious.

> The HTML <span> element is a generic inline container for phrasing content, which does not inherently represent anything. It can be used to group elements for styling purposes (using the class or id attributes), or because they share attribute values, such as lang. It should be used only when no other semantic element is appropriate. <span> is very much like a <div> element, but <div> is a block-level element whereas a <span> is an inline element.
>
> [developer.mozilla.org](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/span)

I have used span to style indeed. But in this case the span is just wrapping the content of the `h2` element and has no purpose.
It could (and should!) be removed.

So I removed the span, but as expected it didn't fix the problem, there still were 3 items inside the `h1`: `Previous`, `electricity` and `reading` should not be in different groups, they should be one single string. What is going on?!


#### React and string concatenation

Because of the comments elements in the DOM such as `<!-- react-text: 225 -->`, I had my suspicions (it was quite hard to miss, haha). So I did some testing:

This is a simple React component:
``` jsx
const reactComponent = () => {
  <div>
    <h1>Hello {this.props.name}</h1>
    <h1>{`Hello ${this.props.name}`}</h1>
  </div>;
}
```

This is the HTML DOM:
```html
  <div>
    <h1>
      "Hello"
      "World"
    </h1>
    <h1>Hello World</h1>
  </div>
```

Ha! This is the "problem"! Well it's not really a problem but it makes sense!

So basically in the first case, we are telling React there are two groups, one is a string and the other, well React will have to figure it out. It turns out it is a string in this case, but it could also be another React component.
<br/>
While in the second case, we are telling React, I am passing you one group and it is a string.

So, basically when concatenating strings, make sure you explicitly concatenate the strings and don't rely on React to do it for you. As we can see, it is not doing that, it is creating different groups for each part of the string.
<br/>
So here we are, I am better developer because of this accessibility bug hunting!


#### What's next?

I watched a great [video](https://www.youtube.com/watch?v=5lzAj1ahRSI&index=22&list=PLNYkxOF6rcICWx0C9LVWWVqvHlYJyqw7g&t=0s) about alerts over the weekend. I am going to try to put into practice what I have learnt about them.
