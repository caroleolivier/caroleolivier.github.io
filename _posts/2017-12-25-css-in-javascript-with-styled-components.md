---
layout: post
title: CSS-in-JavaScript with Styled Components 🎅🏻
date: 2017-12-25
---

I started a new job recently and I am learning a looooooooot of new technologies, libraries, etc... One of them is [styled-components](https://www.styled-components.com/). I'll briefly talk about styled-components at the end of this article, what I want to highlight here is a new (to me) concept **CSS-in-JavaScript**.

#### CSS-in-JavaScript

A few months ago, I wrote an [article](https://caroleolivier.github.io/blog/2017/10/06/styling-react-app) about using [CSS modules](https://github.com/css-modules/css-modules). I really liked CSS modules because it allows to locally scope CSS classes and easyly maintain stylesheets.
CSS-in-JavaScript takes this concept a bit further and embeds the stylesheet **inside** the JavaScript file. In the context of React (I am not sure one would want to use it somewhere else 🤔), it is not that revolutionary as markup is already embedded in JavaScript. So why not the style? This is the question...

It seems the community is quite divided over that topic (what?!), or at least people who write about the subject either love it or hate it. I don't know if either is right, it probably is somewhere in the middle, but I enjoy the battle and I highly recommend reading this article [Stop using CSS in JavaScript for web development](https://medium.com/@gajus/stop-using-css-in-javascript-for-web-development-fa32fb873dcc) **AND** the comments section. It is very interesting.


#### styled-components

To illustrate what CSS-in-JavaScript can look like, I'll show here a few code snippets of styled-components, apparently the trendy library in that space.
<br/>
If you want to learn about it properly, head over to styled-components' [website](https://www.styled-components.com/). If you want to learn about the motivations behind it, watch the video with one of its author [here](https://www.styled-components.com/docs/basics) (it is good although slightly dramatic, haha). The video will give you another perspective on CSS-in-JavaScript to the article I mentioned earlier. I highly recommend reading and watching both.

This is what a React component created with styled-components looks like (copied from styled-components' [website](https://www.styled-components.com/docs/basics)):

```javascript
// title.jsx
// Create a Title component that'll render an <h1> tag with some styles
const Title = styled.h1`
    font-size: 1.5em;
    text-align: center;
    color: palevioletred;
`;
```

And you can use it as a normal React component:
```jsx
// some-component.jsx
...
    render() {
        return <Title>I am a title</Title>
    }
```

The code in title.jsx basically replaces something like:
```jsx
// title.jsx
function Title(props) {
    return <h1 className="title">{props.children}</h1>
}
```
```css
// style.css
.title {
    font-size: 1.5em;
    text-align: center;
    color: palevioletred;
}
```

You can do a lot more with styled-components like access props to style conditionally, I only meant to show the idea behind it: CSS-in-JavaScript.
<br/>
Another thing worth mentioning is the use of Tagged Template Literals, a new ES6 feature. It is quite cool. I couldn't figure out how useful it was until I came across this [article](https://alligator.io/js/tagged-template-literals/). Now I get it!

<br/>

Merry Christmas everyone!
<br/>
🎅 🤶 🧦 🦌 🌟 ☃️ 🎄 🎁 👼 🕯

<br/>

PS: I accidentally decomposed 🎅🏻 into 🎅v🏻 while copying it, what is going on here???? (some new mystery behind emojis again! To be continued...)