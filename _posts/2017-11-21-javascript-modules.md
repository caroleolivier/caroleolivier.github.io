---
layout: post
title: JavaScript Modules
date: '2017-11-21'
---

Today, I looked a bit closer at JavaScript Modules. I have been using modules since I started using JavaScript but I must admit I have never looked into the subject deeply. Using [Babel](https://babeljs.io/) and [webpack](https://webpack.js.org/) things have been working fine and I just assumed webpack built a sort of dependency tree and Babel helped it understanding the latest ES module syntax (import/export).
<br/>
This post is about what I learnt and understood about modules in the context of my setup (babel + webpack). To be honest this is from me to me, if you want to learn about modules and you happen to read this post, be critical.

#### JavaScript Modules

First of all, what is a module? The idea behind module is to isolate a piece of code so that it is easier to maintain, remove, modify, use. Modules are available natively in many languages but may be called differently: C# namespace, Java package, python modules, etc...

In the JavaScript language, modules have been available natively only since 2015 (wow!) with the ECMAScript 6 [standard](http://www.ecma-international.org/ecma-262/6.0/ECMA-262.pdf). Not being able to break down code into modules is madness (in my opinion) and the community has come up with other ways to add modules to the language. (I am not really sure why modules were not added before, I don't know what the process is to create a new ECMAScript standard).
<br/>
The most famous module standards (pre ES6) are [AMD](https://github.com/amdjs/amdjs-api/blob/master/AMD.md)
(a browser targeted standard whose most famous implementation is [requireJS](http://requirejs.org/)) and [CommonJS Modules](http://wiki.commonjs.org/wiki/Modules/1.1) (a standard defined by the group [CommonJS](https://en.wikipedia.org/wiki/CommonJS)).

So depending on the context, people may refer to ES modules (ESM), CommonJS (CJS) modules or AMD (I don't think there is even a consistency in acronym 🤔).



#### Using Modules in practice

I think I have an understanding of what modules are or at least can refer to. What wasn't clear to me was how they were dealt with in my setup. I use [webpack 2](https://webpack.js.org/) and [Babel](https://babeljs.io/) and since modules are ES6 features I was wondering whether they will be converted to something (what??) by Babel or whether webpack will deal with them directly (but how??). I was super confused.

So, let's have a look at a piece of code like below:

```javascript
// simple.js
export let str = "Hello World";

// main.js
import { str } from './simple';
console.log(str);
```

##### Babel

To experiment with Babel I installed [babel-cli](https://babeljs.io/docs/usage/cli/) and configure it using a .babelrc config file. Then I run `npx babel-cli main.js simple.js -o output.js`.
```json
// .babelrc
{
    "presets": [
        ["es2015"]
    ]
}
```
```javascript
// output.js
"use strict";

Object.defineProperty(exports, "__esModule", {
  value: true
});
var str = exports.str = "Hello World";
'use strict';

var _simple = require('./simple');

console.log(_simple.str);
```

Ah that's very interesting! ES6 export statements are added as properties to an object called `exports` while ES6 import use a function called `require`. However, none of them is defined in the output file so where are they coming from?! And this is where knowing a bit about the history of JavaScript modules is useful. Babel's job is to transpile to ES5 and since ES5 has no native modules, it converts ES Modules to one of the pre ES6 modules standard (e.g. CommonJS, AMD). By default, it uses the CommonJS modules standard and if you check the standard [here](http://wiki.commonjs.org/wiki/Modules/1.1) you will see that `exports` and `require` are indeed defined!
<br/>  
Note that it is possible to configure Babel to use other standards (see [here](https://babeljs.io/docs/plugins/preset-es2015/#optionsmodules) for the complete list):

```json
// .babelrc
{
    "presets": [
        ["es2015", { "modules": "amd"}]
    ]
}
```
```javascript
// output.js
define(["exports"], function (exports) {
  "use strict";

  Object.defineProperty(exports, "__esModule", {
    value: true
  });
  var str = exports.str = "Hello World";
});
define(['./simple'], function (_simple) {
  'use strict';

  console.log(_simple.str);
});
```

So Babel transpiles ES6 modules to "something" well defined (AMD, CommonJS, etc...), this is great. However, where is the implementation of that "something" coming from? In the default configuration, `exports` and `require` must be defined somewhere, else whatever is executing my JavaScript (browser, node environment) won't be able to run it (except if there is some runtime magic I don't know about which would only half surprise me).
<br/>
So let's have a look at webpack, the other tool I use in my setup.


##### webpack

Carrying on with my experimentation, the next thing I did was using webpack to bundle the output file generated by Babel `webpack --entry /path/to/babel/output.js --output-filename out/bundle.js` and this is what was generated:

```javascript
// bundle.js
/******/ (function(modules) { // webpackBootstrap
/******/ 	// The module cache
/******/ 	var installedModules = {};
/******/
/******/ 	// The require function
/******/ 	function __webpack_require__(moduleId) {
/******/
/******/ 		// Check if module is in cache
/******/ 		if(installedModules[moduleId]) {
/******/ 			return installedModules[moduleId].exports;
/******/ 		}
/******/ 		// Create a new module (and put it into the cache)
/******/ 		var module = installedModules[moduleId] = {
/******/ 			i: moduleId,
/******/ 			l: false,
/******/ 			exports: {}
/******/ 		};
/******/
/******/ 		// Execute the module function
/******/ 		modules[moduleId].call(module.exports, module, module.exports, __webpack_require__);
/******/
/******/ 		// Flag the module as loaded
/******/ 		module.l = true;
/******/
/******/ 		// Return the exports of the module
/******/ 		return module.exports;
/******/ 	}
/******/
/******/
/******/ 	// expose the modules object (__webpack_modules__)
/******/ 	__webpack_require__.m = modules;
/******/
/******/ 	// expose the module cache
/******/ 	__webpack_require__.c = installedModules;
/******/
/******/ 	// define getter function for harmony exports
/******/ 	__webpack_require__.d = function(exports, name, getter) {
/******/ 		if(!__webpack_require__.o(exports, name)) {
/******/ 			Object.defineProperty(exports, name, {
/******/ 				configurable: false,
/******/ 				enumerable: true,
/******/ 				get: getter
/******/ 			});
/******/ 		}
/******/ 	};
/******/
/******/ 	// getDefaultExport function for compatibility with non-harmony modules
/******/ 	__webpack_require__.n = function(module) {
/******/ 		var getter = module && module.__esModule ?
/******/ 			function getDefault() { return module['default']; } :
/******/ 			function getModuleExports() { return module; };
/******/ 		__webpack_require__.d(getter, 'a', getter);
/******/ 		return getter;
/******/ 	};
/******/
/******/ 	// Object.prototype.hasOwnProperty.call
/******/ 	__webpack_require__.o = function(object, property) { return Object.prototype.hasOwnProperty.call(object, property); };
/******/
/******/ 	// __webpack_public_path__
/******/ 	__webpack_require__.p = "";
/******/
/******/ 	// Load entry module and return exports
/******/ 	return __webpack_require__(__webpack_require__.s = 0);
/******/ })
/************************************************************************/
/******/ ([
/* 0 */
/***/ (function(module, exports, __webpack_require__) {

"use strict";


Object.defineProperty(exports, "__esModule", {
  value: true
});
var str = exports.str = "Hello World";
'use strict';

var _simple = __webpack_require__(1);

console.log(_simple.str);


/***/ }),
/* 1 */
/***/ (function(module, __webpack_exports__, __webpack_require__) {

"use strict";
Object.defineProperty(__webpack_exports__, "__esModule", { value: true });
/* harmony export (binding) */ __webpack_require__.d(__webpack_exports__, "str", function() { return str; });
let str = "Hello World";

/***/ })
/******/ ]);
```

And voilà! The missing piece of the puzzle is here. webpack is indeed the one taking care of defining or replacing `exports` and `require`. I will let you have fun reading webpack output (I debugged through the code in the browser, it was easier).


#### Conclusion

So here is what I understand about modules now (and remember to double check what I am writting): JavaScript modules exist natively only since [ES6](http://www.ecma-international.org/ecma-262/6.0/ECMA-262.pdf) (2015) so they are very recent. However, modules were added to the languages thanks to third party libraries and standards such as [AMD](https://github.com/amdjs/amdjs-api/blob/master/AMD.md) and [CommonJS Modules](http://wiki.commonjs.org/wiki/Modules/1.1). For web applications, since browsers do not widely support ES6 yet, ES6 modules must be transpiled to ES5 and some module implementation. In practice, when using [Babel](https://babeljs.io/) and [webpack](https://webpack.js.org/), Babel first transpiles ES6 modules to CommonJS modules (by default but can be changed) and webpack later converts it to its own module implementation.

And this is how modules are being taken care of in my setup, phew!


#### Interesting articles

A few articles that helped me understand JavaScript modules:
* [exploringJS modules](http://exploringjs.com/es6/ch_modules.html)
* [tree shaking](https://medium.freecodecamp.org/tree-shaking-es6-modules-in-webpack-2-1add6672f31b)
* [the state of JavaScript modules](https://medium.com/webpack/the-state-of-javascript-modules-4636d1774358)