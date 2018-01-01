---
layout: post
title: Getting up and running with React Native
date: 2018-01-02
---

Today, I meant to look into [react-native](https://facebook.github.io/react-native/). I know React (a little) but recently realised I had made some wrong assumptions about React Native: I thought the idea behind React and React-native was: **Learn once, write once, use everywhere**. It is NOT! The real motto is: **Learn once, use everywhere**, i.e. you learn the basics once but you can't have one code base targeting all platforms. What a disappointment... I thought with the help of some kind of web dark magic it was the case.

Anyway, since it is not the case and one needs to learn a few things that are React Native specific, off I went to learn the basic of React Native. I anticipated a few hick ups along the way but not that early:
<br/>
I aimed to go through this [tutorial](https://facebook.github.io/react-native/docs/getting-started.html) on React Native website.

##### Step 1: install the tool create-react-native-app
```
npm install -g create-react-native-app
```

=> ✔️ Successful!

##### Step 2: create a React Native app project
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


##### Step 3: start the server

```
yarn start
```

=> ❌ Unfortunately, it failed too. But, again, with a very useful message.

![yarn start error]({{ "/assets/blog/react-native-app-yarn-start-error.png" | absolute_url }}){:height="80%" width="80%"}

I chose to install watchman (arbitrarily, I don't know what solution is best). However, when installing it, it errored:

![brew install watchman error]({{ "/assets/blog/react-native-app-yarn-start-error.png" | absolute_url }}){:height="80%" width="80%"}

According to this [thread](https://stackoverflow.com/questions/29319378/cant-link-pcre-thru-brew-in-max-os-yosemite) on Stack Overflow, brew requires the content of the folder /user/local to be owned by you.
```
sudo chown -R $(whoiam) /usr/local/*
brew link pcre
```
And that solved the problem, I was able to start the server, yay!

Note that if you try to run `sudo chown -R $(whoiam) /usr/local` as suggested in the Stack Overflow thread, it fails on Mac High Sierra with `operation not permitted` (see [here](https://github.com/Homebrew/brew/issues/3228)). It looks like just the content, not the directory itself, must be owned by you (and can be own by you anyway).

After that I managed to start the server and this is what I got:

![yarn start success]({{ "/assets/blog/react-native-yarn-start.png" | absolute_url }}){:height="80%" width="80%"}


##### Step 4: install the Expo client

I had never heard of the [Expo](https://expo.io) application before. Apparently, it allows the development of React Native application without having to install any Android or IOS SDK. It sounds like a great tool for users like me who knows React a little but don't know anything about developing mobile apps.

I installed Expo on my Android phone and was able to see the bundled code in my app!
