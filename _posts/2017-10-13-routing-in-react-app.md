---
layout: post
title: Routing in a React application
date: 2017-10-13
---

Today I spent some time looking at setting up routing in my React application. I thought it would be super quick but I actually ended up reading quite a lot about it, it turns out there is a lot to know!
<br/>
In this post I will talk a bit about routing (client-side, server-side) and then how I enabled it in my React application.
<br/>
Please keep in mind that my experience with React is very, very limited.

#### Routing

First of all, what does `routing` mean?

> Routing is the process of selecting a path for traffic in a network, or between or across multiple networks.
> <br/> [Wikipedia](https://en.wikipedia.org/wiki/Routing)

And in the context of routing pages in the browser routing it means: given url `load/my/page` then load the correct page.


There are two types of web page routing: **client-side** and **server-side**. Server-side routing is (in my head) the original routing, i.e. you enter a URL, it hits a web server, the web server resolves the URL path and then it serves you the page at that path. Client-side routing is done *within* the browser by a JavaScript library (I have no idea if it can be done differently).
<br/>
Although the two solutions are the same from a user perspective, i.e. entering a URL results in changes on the screen, they are very different solutions and have different pros/cons. Server-side routing involves network traffic and reloading of an entire web page while client-side routing is contained to the browser so it is more efficient. However, with client-side routing the whole website needs to be loaded upfront (so this step is less efficient), routing is more complex to set up and above all it means using JavaScript. So dependending on your requirements and dependencies (with/without JavaScript) you may want to pick one or the other.


#### Client-side routing in React

My website is super simple so I could have gone away with using server side routing. It is not a super heavy and complicated SPA (Single Page Application). However, I was curious to learn how to use client-side routing with React. After all React is used to build SPA and client-side routing is a big part of SPA else you may as well build a static website.

##### React Router

I googled a bit and found [React Router](https://github.com/ReactTraining/react-router). The [documentation](https://reacttraining.com/react-router/web/guides/philosophy) is really good so I followed it and had it working very quickly. However, I quickly realise that client-side routing is fine if your navigation starts from the root, i.e. start at `/` and then move around `/users`, `/users/123`, etc... If you refresh or want to directly access `/users/123` it doesn't work anymore. And it makes sense! Your React app is served from the root so when you enter `/users/123` the browser calls the web server which tries to load the file at `/users/123` and can't find it. This is better explained [here](https://github.com/facebookincubator/create-react-app/blob/master/packages/react-scripts/template/README.md#serving-apps-with-client-side-routing). 

So then what do you do? Do you do a mix of client and server side routing? Well kind of. The solution to this problem is for the server to fallback and send back the root file whenever it should return a 404. So when you go to `/users/123` directly or refresh, the server returns you the root file (likely `index.html`) and then your routing JavaScript library does the rest.

##### Handmade Solution

So I was quite happy with that solution until I came across this [post](http://jamesknelson.com/push-state-vs-hash-based-routing-with-react-js/) which got me seriously thinking about what I was doing.
<br/>
James K Nelson shows that routing can easily be done using hash-based routing. Hash-based routing means that the URL part before the hash sign is sent and resolved by the web server while the part after the hash is resolved locally. He argues that having a # in your URL probably isn't that bad in comparison with introducing yet another dependency with react-router. He makes two very good points in his post: first react-router depends on React API which means that if the API changes then your application may break and second react-router must be loaded and so you grow the size of your bundle (even if by not that much). I must say I agree with him so I tried his solution and it was super easy. I also didn't need to fiddle with server side routing either (which is nice).

<br/>


So there are a lot of differents solutions out there to introduce routing in an application. For now I decided to keep a similar solution to James K Nelson's. It is simple and satisfy my current needs but, as I said before, my experience is quite limited so I may change my setup in the future as my application grows and I learn more about React. Or even change my mind about using hash-based routing.
