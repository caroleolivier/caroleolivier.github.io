---
layout: post
title: Styling a React application
date: 2017-10-06
---

In the last couple of weeks I looked at [how to write a simple React application from scratch](https://caroleolivier.github.io/blog/2017/09/22/creating-a-react-app-from-scratch) and [how to test it](https://caroleolivier.github.io/blog/2017/09/26/testing-a-react-app). 
Today I looked at how to style it.

I have limited experience of styling websites (I have only used CSS and SASS in the past) so I googled the subject and found many, many solutions. I won't walk you through all of them as this is not the subject of this post, I will only describe the solution I chose and how I set it up. If you would like to know more I highly recommend this [post](http://andrewhfarmer.com/how-to-style-react/) by Andrew Farmer, it is very informative and very well written.
<br/>
So after reading up a bit about the different solutions I settled for [CSS Modules](https://github.com/css-modules/css-modules) together with [SASS](http://sass-lang.com/) and here is how I set it up in my web application.


#### CSS Modules

I am new to [CSS Modules](https://github.com/css-modules/css-modules) but I really like the idea behind it:
> A CSS Module is a CSS file in which all class names and animation names are scoped locally by default.

CSS classes are scoped globally by default so when your application grows it quickly becomes a nightmare to maintain and keep track of which classes is used where. With CSS modules (and the help of your building engine) you can import CSS files within a JavaScript file and CSS classes will automatically be scoped locally to that file (if I am not clear you will see in the setup section what I mean).
<br/>
In the context of React this means that you can easily scope a CSS file to a React component. This goes really well with React philosophy where "componentisation" and isolation is a big deal. By default with React we already define together both the structure (JSX) and logic (JavaScript) of a component so by using CSS Modules we are adding the style to the mix and completes the definition of the component and I think that's really cool!


Obviously we are very far from the popular paradigm where the visualisation and logic of a UI component are split but with React we are very far from that anyway so if you are using React you are very likely onboard with that approach :)


##### CSS Modules Setup

If you have followed my previous posts, you'll know that I am using [webpack]() as a bundler. It is super easy to configure CSS modules with webpack. All you have to do is install the webpack loader `css-loader` and its dependency `style-loader` with [npm]() and then add a new rule to webpack config for processing CSS files:

```
// webpack.config.js
{
    test: /\.css$/,
    use: [
        {
            loader: 'style-loader'
        },
        {
            loader: 'css-loader',
            options: {
                modules: true,
                localIdentName: '[path][name]__[local]--[hash:base64:5]'
            }
        }
    ]
}
```

With that configuration in place you can create CSS files and directly import them into React components:

```
// style.css
.myClass {
    // ...
}

// component.js
import style from 'path/to/style.css';
render() {
    return <div className={style.myClass}></div>
}
```

I find it pretty cool :)

#### SASS

Now, CSS is good but if you use it for a little while you'll quickly understand why people use preprocessors like [SASS](http://sass-lang.com/). They allow you to write more maintanable style sheets thanks to features such as variables, import and nesting (and others). (If you are new to web development and don't have much experience with (S)CSS start with bare CSS and see where it takes you, that way it will be easier to understand why I am adding yet another dependency).

Adding support to SASS with webpack is again super easy. All you have to do is install the loader `sass-loader` (and its dependency `node-sass`) with npm and add it to the list of loaders in webpack config:
```
// webpack.config.js
loaders: [
    {
        test: /\.scss$/, // changed from css to scss
        use: [
        ...,
        {
            loader: 'sass-loader'
        }
    }
]
```

Be careful with the order of the loaders. The SASS loader should be last which means it will be processed first. I couldn't find where it is specified in the documentation of webpack 2 but you can find some information in this [stackoverflow post](https://stackoverflow.com/questions/32234329/what-is-the-loader-order-for-webpack).

#### Jest and style sheets

If you have read my [post](https://caroleolivier.github.io/blog/2017/09/26/testing-a-react-app) about testing a React application you may know that I am using [Jest](https://facebook.github.io/jest/) as test runner. If you want to import (S)CSS files into React components you need to make a couple of more changes else Jest can't even run your test anymore, it fails on the import of (S)CSS files.
<br/>
What you need to do is described on Jest's website [here](https://facebook.github.io/jest/docs/en/webpack.html#mocking-css-modules): add a dev npm dependency to `identity-obj-proxy` and configure Jest to use it:
```
// jest.config.js
moduleNameMapper: {
    '\\.(css|scss)$': 'identity-obj-proxy'
}
```
Obviously this will fix running your tests not the tests themselves!

<br/>


And that's it, I am all set for styling my React application. CSS Modules allows me to easily scope styles to a single component and SASS helps me to write nice maintainable CSS. If you want to see a simple example of how it works, you can check out the code [here](https://github.com/caroleolivier/minimal-react-starter/tree/v4.0.0).
