---
layout: post
title: React-native 1 - 0 Carole
date: 2018-01-01
---

Today, I meant to look into [react-native](https://facebook.github.io/react-native/). I know React (a little) but recently realised I had made some wrong assumptions about React Native: I thought the idea behind React and React-native was: **Learn once, write once, use everywhere**. It is NOT! The real motto is: **Learn once, use everywhere**, i.e. you learn the basics once but you can't have one code base targeting all platforms. What a disappointment... I thought with the help of some kind of web dark magic it was the case.

Anyway, since it is not the case and one needs to learn a few things that are React Native specific, off I went to learn the basic of React Native. I anticipated a few hick ups along the way but not that early:
<br/>
I aimed to go through this [tutorial](https://facebook.github.io/react-native/docs/getting-started.html) on React Native website.

##### Step 1
```
npm install -g create-react-native-app
```

=> ✔️ Successful!

##### Step 2
```
create-react-native-app HelloWorld
```

=> ❌ Unfortunately, failed! It didn't do much, just created a package.json file and then failed. The good news is that it failed with a very useful error message.

![create-react-native-app error]({{ "/assets/blog/create-react-native-app-error.png" | absolute_url }}){:height="80%" width="80%"}

I googled a bit trying to understand what was going on and why npm 5 wouldn't be supported by [create-react-native-app](https://github.com/react-community/create-react-native-app#installation). I haven't found the exact answer yet apart from `npm 5 is buggy`. The main suggestion to fix this problem is to downgrade npm to 4.x.x, but I don't really like the idea of downgrading, I think we should move forward, not backwards. So I am still debating with myself...

[UPDATED]

After more reading on GitHub, I luckily came across a very useful comment on this [issue](https://github.com/react-community/create-react-native-app/issues/424):

> when yarn is installed, CRNA already uses it by default to install packages.

So I installed [yarn](https://yarnpkg.com/en/) (a package manager similar to npm) and it created my HelloWorld project! (Well it failed at the next step but that's okay, I feel like I haven't completely lost the battle...)