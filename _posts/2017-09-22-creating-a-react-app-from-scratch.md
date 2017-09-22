---
layout: post
title: Creating a React app from scratch
date: 2017-09-22
---
A couple of days ago, I went through this React [tutorial](https://facebook.github.io/react/docs/hello-world.html). I wrote a few words about it in a previous [post](https://caroleolivier.github.io/blog/2017/09/15/a-tast-of-react). In the [installation step](https://facebook.github.io/react/docs/installation.html) I used the library [Create React App](https://github.com/facebookincubator/create-react-app) to create the skeleton of a working app and avoid spending too much time setting everything up. Today I went back to this step and set up everything myself. I wanted to really understand what is needed to get a React app up and running, especially understand what dependencies I introduce when I choose React as a web development library.

As usual, I searched the web to find a tutorial and as usual I found some good ones. I settled for this very good [7-step walkthrough guide](http://andrewhfarmer.com/build-your-own-starter/#0-intro). I changed a couple of things but it is very complete if you want to start from scratch. You can find my repo with the code [here](https://github.com/caroleolivier/minimal-react-starter).

Below are the dependencies I added to get my React app working (additionally to installing React obviously!).
Note that I added all the dependencies using [npm](https://www.npmjs.com/).


##### Babel

First I installed [Babel](https://babeljs.io/). Babel is a transpiler that allows you to use the latest JavaScript version by transpiling down your code to ES5, an older version widely supported by browsers. So if you want to use the latest shiny JavaScript features, it is very useful.

In the case of React app, it is even more useful as it can also transpile [JSX](https://facebook.github.io/react/docs/introducing-jsx.html) to JavaScript. (You can develop React app without JSX but I don't really see any good reasons not to). All you have to do is install and configure the [react preset](https://www.npmjs.com/package/babel-preset-react).


##### webpack

Then, I installed [webpack](https://webpack.js.org). From webpack's website:
> webpack is a module bundler for modern JavaScript applications. When webpack processes your application, it recursively builds a dependency graph that includes every module your application needs, then packages all of those modules into a small number of bundles - often only one - to be loaded by the browser.

So webpack takes your code base, resolves the dependencies and outputs one single .js file containing everything. If you are developping a web application, all you have to do is to include that single JavaScript file in your HTML somewhere and the browser will load it.

webpack is also very configurable and provides a bunch of very useful features like [modules](https://webpack.js.org/configuration/module/) which allow you to define how to load modules or files, e.g. load my code using Babel; [plugins](https://webpack.js.org/plugins/) which allow you to run extra bits of code on the bundle like minimising it; and many others I am not familiar with (yet!). Its configuration can be a bit overwhelming but the documentation is really good and you can find plenty of information online.


##### webpack-dev-server

Finally, in order to test my application locally, I installed a web server.
In the tutorial I referenced at the beginning of this post they use [ExpressJS](http://expressjs.com/). I am not familiar with that web server at all and decided to stick to the one I know and like: [webpack-dev-server](https://webpack.js.org/guides/development/#using-webpack-dev-server).
You have to install it as an extra dependency but it integrates very well with webpack. If you have never used it I recommend checking it out.

<br/>

So writing an app with React from scratch was pretty easy (well thanks to Babel and webpack) and not very different to writing a normal web app. It only took me 2 hours to install everything and that's because I took my time!
If you are new to web development and are jumping straight into React, I highly recommend investing some time reading about webpack and Babel, understanding what they do and how to configure them.
