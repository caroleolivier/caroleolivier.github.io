---
layout: post
title: Using TypeScript with React, Webpack, Babel and Jest
date: 2017-11-28
---

Yesterday I added support for [TypeScript](https://www.typescriptlang.org/) to my [minimal-react-starter](https://github.com/caroleolivier/minimal-react-starter/releases/tag/v6.0.0) project. In this post, I will share my experience and a few tricks.
<br/>
Please note this is not a tutorial about [TypeScript](https://www.typescriptlang.org/), [React](https://reactjs.org/), [Babel](https://babeljs.io), [webpack](webpack.js.org) or [Jest](https://facebook.github.io/jest/). This post is aimed at people who already know these technologies, and are curious to know how to stitch them together to get a working React app written in TypeScript.
<br/> My previous articles about [writing a React application from scratch](https://caroleolivier.github.io/blog/2017/09/22/creating-a-react-app-from-scratch) and [how to test a React application](https://caroleolivier.github.io/blog/2017/09/26/testing-a-react-app) may help.

### TypeScript

A few words about TypeScript. This is the editorial part of the post ^^.
<br/>
My background as a developer is C# and I looove this language. I really love statically-typed languages. I love their compile time safetiness and I love the intellisense that comes with them. So when I moved to JavaScript and Python I was in for a shock. I felt like I had gone backwards in terms of productivity. Don't interpret my statement as "I don't like dynamically-typed languages", this is not the case, I think they are great, but usually for the type of work I do, code written with statically-typed languages are safer and more maintainable. As a (personal) rule of thumb: system applications => statically-typed languages; prototypes => dynamically-typed languages.

Hence, it is not surprising I am exploring alternatives to JavaScript for developing web applications, and TypeScript seems to be the most popular option at the moment. TypeScript is **optionally statically-typed** 🤔: because it is a superset of JavaScript, one can (yay!) but doesn't not have to, use type annotations to enable type checking at compile time. Therefore, it is not as safe as C# for instance, but it means I can migrate my JavaScript code iteratively to TypeScript and also keep using JavaScript libraries.

I won't go into more detail here, but I recommend reading the TypeScript Wikipedia [page](https://en.wikipedia.org/wiki/TypeScript), it has some very good information (for instance the history part is very interesting!).


### Using TypeScript

The project I am working on uses [webpack](https://webpack.js.org/) as a JavaScript bundler and [Babel](https://babeljs.io) as a JavaScript transpiler to transpile JavaScript ES6 to ES5. If I add TypeScript to the mix it means that somehow TypeScript code must be converted to ES5 JavaScript and bundled with the rest of the code.

The key missing tools are the TypeScript compiler and a TypeScript loader for webpack like [ts-loader](https://github.com/TypeStrong/ts-loader). You can find plenty of information on the Internet on how to configure them, I would recommend starting with the documentation of the [TypeScript compiler](https://www.typescriptlang.org/docs/handbook/compiler-options.html) and [ts-loader](https://github.com/TypeStrong/ts-loader) (if you use ts-loader, you can also use [awesome-typescript-loader](https://github.com/s-panferov/awesome-typescript-loader)).
<br/>
I would say the main "problem" is probably deciding what tool is responsible for what. For instance, should tsc, the TypeScript compiler, compile to ES5 or should it compile to ES6 and then Babel would compile to ES5? Should JSX be transpiled to JavaScript by tsc or Babel? I am not an expert but I feel like there are many, many different setups. You can take a look at my changes in my GitHub [repository](https://github.com/caroleolivier/minimal-react-starter/commit/6da9bc00adc8576b0826e812148fb5bdc21ecda5), otherwise below is a summary of the changes:

```json
// package.json
{
    ...
    "devDependencies": {
        "ts-loader": "^3.1.1",
        "typescript": "^2.6.1",
        ...
      },
    "dependencies": {
        "@types/react": "^15.6.7",
        "@types/react-dom": "^15.5.6",
        ...
    }
}
```
```json
// tsconfig.json
{
    "compilerOptions": {
        "target": "es5",
        "jsx": "preserve"
    }
}
```

```"jsx": "preserve"``` means I decided to have the TypeScript compiler compile code to ES5 but to not compile JSX (completely arbitrary, I have no real reason for this choice). 

```json
// webpack.config.json
module.exports = {
    ...
    module: {
        rules: [
            {
                test: /\.tsx?$/,
                exclude: /node_modules/,
                use: [
                    {
                        loader: 'babel-loader'
                    },
                    {
                        loader: 'ts-loader'
                    }
                ]
            },
        ...
    }
}
```





### Testing with TypeScript

Next I looked at how to test a React Component written in TypeScript. I stuck to TypeScript to write the tests too (as opposed to using JavaScript) as, ultimately, I want all my code base to be written in TypeScript.

I currently use [Jest](https://facebook.github.io/jest/) as a test runner. Jest loads files with transformers to transform (what?!) source files into something [node](https://nodejs.org/en/) understands. For TypeScript there is an existing loader called [ts-jest](https://github.com/kulshekhar/ts-jest). It makes loading and running TypeScript tests super easy. Again, there is plenty of information on the web to see how to configure Jest for TypeScript so I won't go into detail. I would suggest starting with [Jest](http://facebook.github.io/jest/docs/en/configuration.html#content) and [ts-jest](https://github.com/kulshekhar/ts-jest) documentation. I would also recommend taking a close look at ts-jest limitation [section](https://www.npmjs.com/package/ts-jest#known-limitations-for-ts-compiler-options). I got caught by:
> You can't use "jsx": "preserve" for now (see progress of this issue);

```"jsx": "preserve"``` is part of the TypeScript compiler configuration (and is used by ts-jest). It means "do not compile JSX", i.e. preserve it. Unluckily, I chose this mode and it doesn't work with Jest. Jest allowed only one transformer to be called per file, so ts-jest must be configured to compile to ES5 when it processes TypeScript files.

You can take a look at my changes in my GitHub [repository](https://github.com/caroleolivier/minimal-react-starter/commit/f04477c0783e7216f661f53b20c9a40b1f259a00), otherwise below is a summary of the changes:
```json
// package.json
{
    ...
    "devDependencies": {
        "ts-jest": "^21.2.3",
        ...
      },
    "dependencies": {
        "@types/jest": "^21.1.8",
        ...
    }
}
```
```json
//tsconfig.json
{
    "compilerOptions": {
        "target": "es5",
        "jsx": "preserve"
    }
}
```
``` javascript
// jest.config.js
module.exports = {
    ...
    testRegex: '/*.test.(ts|tsx|js)$',
    moduleFileExtensions: ['js', 'jsx', 'json', 'ts', 'tsx'],
    transform: {
        '^.+\\.tsx?$': 'ts-jest',
        '^.+\\.jsx?$': 'babel-jest'
    }
}
```

<br/>

Now that this is in place I can finally use TypeScript in my project! Setting up the whole thing was super interesting, and now that I know how the plumbing works, I feel more comfortable moving on to the next stage: actually learning TypeScript!