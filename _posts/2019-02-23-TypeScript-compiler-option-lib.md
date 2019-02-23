---
layout: post
title: TypeScript compiler options&#58; lib
date: 2019-02-23
---

I have been using TypeScript for a little while now, but I must admit I have never really looked at what all the compiler options really are. In this post I'll talk about the `lib` option, what it means and how it works.

```javascript
// tsconfig.json
{
  "compilerOptions": {
    "lib": ["es2016"]
  }
}
```

### What is the lib option?

According to TypeScript [documentation](https://www.typescriptlang.org/docs/handbook/compiler-options.html), the `lib` option is:

> List of library files to be included in the compilation.
<br />
> Possible values are: 
<br />
> ES5 
<br />
> ES6 
<br />
> ES2015 
<br />
> etc...

Honestly, I wasn't really sure what it meant. What library files? Where are they? So I dug a bit more.

If you install TypeScript with `yarn` or `npm`, and you explore the `node_modules/typescript/lib` folder, it comes with a bunch of type definitions:

![typescript lib list non exhaustive]({{ "/assets/blog/typescript_libs.png" | absolute_url }}){:height="70%" width="70%"}

So basically, when you add things like `es2016` or `es2017` to the `lib` array, all you're doing is telling the compiler to include certain types and use them during compilation. And those types are available because they come with TypeScript installation :)

For instance, let's take the `includes()` method on arrays.
```javascript
const array = ["giraffe"];
array.includes("giraffe");
```

Since it was part of the ES2016 specification, if you configure TypeScript to use `es2015`, i.e. `"lib": ["es2015"]`, this code will fail.

![typescript error]({{ "/assets/blog/typescript_includes_error.png" | absolute_url }}){:height="60%" width="60%"}

This is because the type definition for `includes` is in the ES2016 file.
<br/>
You can check this by exploring the type files in the lib folder (locally if you have TypeScript installed or on GitHub [here](https://github.com/Microsoft/TypeScript/blob/master/lib/lib.es2016.array.include.d.ts)). You should see a file called `lib.es2016.array.include.d.ts`, this is the file with the type definition for `includes`.

So this solves the mystery around how `lib` is used.

### Why is it useful?

This comes handy when you're developing web applications and you want to make sure your code is compatible with the browsers you are targeting.

For instance, `includes` is not supported by Internet Explorer, see [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/includes). So if it is important to you, it might be useful to only include types from the ES2015 specification, i.e. configure `lib` with `es2015`.


### How to solve specification compatibility problems?

So, we know that `includes` is not compatible with IE. What should/can you do next?

#### Not use it :)

In the case of `includes`, you could implement the method yourself or use an external library like `lodash`.
<br/>
This means missing out on the latest shiny JavaScript features which is sad for a developer.
<br/>
And so this is where polyfills become handy.


#### Use polyfills

> A polyfill is a piece of code (usually JavaScript on the Web) used to provide modern functionality on older browsers that do not natively support it.
<br/>
> [MDN](https://developer.mozilla.org/en-US/docs/Glossary/Polyfill)

In our example, it means adding a piece of code that will contain the implementation of `includes`.
<br/>
Transpilers like [Babel](https://babeljs.io) do provide polyfilling capability via the use of plugin or you can import external libraries that do it for you like [core-js](https://www.npmjs.com/package/core-js).

One important thing to bear in mind here is that you need to do 2 things:
1. Add the TypeScript type definition
2. Add the polyfill

If you don't do 1. your code doesn't compile. If you don't do 2. your code doesn't run. You could easily forget one or the other, as they are independent.
<br/>
This is the really weird thing for me with TypeScript. My C# brain sometimes struggles to get around this. The type definition is used at compile time by the compiler. The polyfills are used at run-time by the browser 😕

But now at least I understand how to use `lib`!
