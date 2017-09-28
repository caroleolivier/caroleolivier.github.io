---
layout: post
title: Testing a React application
date: 2017-09-26
---

I spent the morning looking at ways to add tests to my React application. In this context I mean being able to test that 1) a React component renders without crashing; 2) it renders the correct DOM; 3) the business logic (embedded or not in a component) is correct.

Browsing around, it seems that the stack [React](https://facebook.github.io/react/), [Jest](https://facebook.github.io/jest/) and [Enzyme](http://airbnb.io/enzyme/) is quite popular.
Let's have a look at how it can help.

**Disclaimer: if you come across this article, please bear in mind that I am nowhere near an expert in these libraries so please be very critical! (also feel free to submit pull request or comment on github if you think something can be improved :))**

You can find the code I am referring to [here](https://github.com/caroleolivier/minimal-react-starter/tree/v2.0.0) at v2.0.0.

#### Running a simple test with Jest

In order to run my tests I used [Jest](https://facebook.github.io/jest/). Jest is a JavaScript testing solution developed by Facebook. I installed it using npm: `npm install --save-dev jest` and added an npm script called `test` pointing to Jest (it helps with running the tests: `npm run test`).

In order to test my setup I then wrote a trivial test:
```
    // sum.test.js
    test('adds 1 + 2 to equal 3', () => {
        expect(1 + 2).toBe(3);
    });
```
I ran `npm run test` and it worked perfectly so Jest was all set.


#### ES6 and JSX with Jest

I wanted to carry on using ES6 and React JSX with Jest so I added a dependency to [babel-jest](https://github.com/facebook/jest/tree/master/packages/babel-jest). I installed it with npm: `npm install --save-dev babel-jest`.
No configuration was needed, it is enabled by default once installed (magic...).


#### Testing DOM rendering of a Component

I tested a very simple component called Counter, the code is [here](https://github.com/caroleolivier/minimal-react-starter/blob/master/src/Counter.js) (it has a button that increments a counter every time it is clicked).
The first thing I decided to test was checking that the component renders without failures. This is how you can do it with Jest:
```
    import ReactDOM from 'react-dom';

    describe('Counter component', () => {
        test('renders without crashing', () => {
            const div = document.createElement('div');
            expect(() => ReactDOM.render(<Counter/>, div)).not.toThrow();
        });
    });
```
This test renders the Counter component in the DOM and checks that no exception is thrown. It doesn't test much but if your component has some child dependencies that are broken or you made a change that breaks the DOM it will tell you.
It is also useful when you migrate or upgrade major versions of certain libraries (e.g. React) and want to check that it does not have major impacts such as breaking your DOM.


#### Testing DOM content of a Component

Next, I tested that the actual DOM (the HTML tree) was correct when rendered for the first time. I did that using Jest [snapshot functionality](https://facebook.github.io/jest/docs/en/snapshot-testing.html): it basically allows you to check that the DOM generated for your component hasn't changed by comparing the HTML tree against a snapshot you provide to the test. You can provide the snapshot yourself or you can use the one created automatically the first time you run the test (just make sure it does not contain an error already).

This is my test:
```
    import testRenderer from 'react-test-renderer';
    import Counter from './Counter';

    describe('Counter component', () => {
        test('DOM matches the snapshot', () => {
            const component = testRenderer.create(
                <Counter/>
            );
            let tree = component.toJSON();
            expect(tree).toMatchSnapshot("rendering");
        });
    });
```
And this is the snapshot that was generated the first time I ran the test:
```
    // Jest Snapshot v1, https://goo.gl/fbAQLP

    exports[`rendering 1`] = `
    <button
    onClick={[Function]}
    >
    0
    </button>
    `;
```
This is the first time I have done that kind of testing and I wonder in practice how useful and maintanable this is. If you add more cases or if your component is more complex how developer-friendly does it become? I will need to use it on a real life project to decide but for now I tested the functionality and I found it pretty cool for a simple component like mine (maybe this is the key, it forces you to keep your component super simple ;))

Note that for this test I also added an extra npm dependency to [react-test-renderer](https://www.npmjs.com/package/react-test-renderer): `npm install --save-dev react-test-renderer` (react-test-renderer is a utility that provides renderers that don't depend on DOM).


#### Testing business logic within a Component

Next I wanted to test the business logic embedded in the component, i.e. when the button is clicked the counter is incremented by one.

For that purpose I used [Enzyme](http://airbnb.io/enzyme/). Enzyme is developed by [airbnb](https://www.airbnb.co.uk/about/about-us) and is (quoted from Enzyme's website):

>  a JavaScript Testing utility for React that makes it easier to assert, manipulate, and traverse your React Components' output. 

For the installation I followed this guide: [Working with React 15](http://airbnb.io/enzyme/docs/installation/react-15.html). First, you install two new npm dependencies `npm install --save-dev enzyme enzyme-adapter-react-15` and then you need to add the piece of code below somewhere in you project (I haven't looked yet what this is for):
```
    import { configure } from 'enzyme';
    import Adapter from 'enzyme-adapter-react-15';

    configure({ adapter: new Adapter() });
```
I added mine in a file called enzyme-setup.js and configured Jest to pick it up on startup:
```
    // jest.config.js
    module.exports = {
        setupFiles: [
            './enzyme.setup.js'
        ]
    };
```
The last step is to finally write the test. In my case I wrote a test checking that the Counter component increments its counter by one every time the button is clicked:
```
    describe('Counter component', () => {
        test('increments counter on click event', () => {
            const counterWrapper = shallow(<Counter />);
            const counter = counterWrapper.instance();

            expect(counter.state.counter).toBe(0);

            counterWrapper.simulate('click');
            expect(counter.state.counter).toBe(1);
        });
    });
```

<br/>

So to summarise, to create a React application from scratch and be able to test it I used: [React](https://facebook.github.io/react/), [Babel](https://babeljs.io), [webpack](webpack.js.org), [webpack-dev-server](https://webpack.js.org/configuration/dev-server/), [Jest](https://facebook.github.io/jest/) and [Enzyme](airbnb.io/enzyme/).

I am now ready for the next step, i.e. working on a bigger project than a single component React application :)