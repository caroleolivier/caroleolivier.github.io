---
layout: post
date: 2018-01-20
title: Constructor function and function context in JavaScript
---

Today, I spent some time looking into JavaScript constructor functions. I ended up reading (again...) about function context and it finally sunk in. Here are my findings.



#### Constructor Function

First of all, what is a constructor function? A constructor function is a function that allows you to create an instance of an object when it is called. So every time you call it, you get a new object. It looks like that:

```javascript
function Person(name, age) {
    this.name = name;
    this.age = age;
}
```

And you call it like with the `new` keyword:

```javascript
const person = new Person('John', 40);
```

The `person` variable is a JavaScript object with 2 properties: name and age. From what I understand, when a constructor function is called, this is what happens:
1. a new JavaScript object is created {}
2. `this` is set to the newly created object
3. the code of the constructor is called
4. this new object is returned

Cool, so that is simple, I should be able to remember it. 

Now, what happens when you just call `Person()`? It simply does whatever any other function would do. In this case, it means adding/setting `age` and `name` to the context of the function represented by `this`. If I am fully honest, although I have been writing JavaScript code for a little while, I never fully understood this subject. I knew the context of a function depends **on how it is called**, but there were cases where it wouldn't necessarily make sense to me. So today, I decided to really look into it.


#### Function context

The best source I found about function context is on [developer.mozilla.org](https://developer.mozilla.org) in [here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/this). I highly recommend reading the documentation.


Going back to my example, I made a few experiments.

```javascript
// index.js
Person('John', 40);

console.log(`${this.name} is ${this.age}`);

console.log(`${global.name} is ${global.age}`);

function test() {
    console.log(`${this.name} is ${this.age}`);
}
test();
```
Console output:
```bash
$ node index.js
undefined is undefined
John is 40
John is 40
```

`test()` and `Person(...)` are both called in similar way so it makes sense that `this` refers to the same thing. However, the first log statement confused me. I expected two things: first, I expected `this` to refer to the same thing everywhere (because of the way the functions are called); second, I expected `this` to be the global scope everywhere 🤔. (note that `global` represents the global scope when running a node application)
<br/>
After reading up a bit I found a couple of rules that explain this behaviour:
1. when entering a function, if the value of `this` is not set by the call, then it refers to the global scope.
2. node uses CommonJS for modules and so in the index.js file, i.e. index module, `this` refers to the context of the module.

Let's set the context of the function call explicitly and see what happens:
```javascript
Person.call(this, 'John', 40);

console.log(`${this.name} is ${this.age}`);

console.log(`${global.name} is ${global.age}`);

function test() {
    console.log(`${this.name} is ${this.age}`);
}
test.call(this);
```
Console output:
```bash
$ node index.js
John is 40
undefined is undefined
John is 40
```

Eureka!


#### Strict mode

I am briefly mentioning strict mode here, because it changes the behaviour of `this` inside function calls when the context is not explicitly set. As we saw, it is set to the global scope by default but in strict mode, it actually remains unset. So the first code sample I tested would fail.


<br/>

There are many things I haven't mentioned here like the context of a function inside a class, or the context of an arrow function. I never had any problems with these. The context of a function call on an object is (usually) the object itself because of the way we (normally) call the function, i.e. `person.something()`. And the context of an arrow function is the one of the containing code (apparently it is called lexical context).

Hopefully, from now on, I shall not be surprised by what the context of a function is.