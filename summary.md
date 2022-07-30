## What is React?

React is a library that, for purposes of Phase 2, you will use to manage *components* and *states.* These things make it easier to render elements on a page dynamically, and writing the code to render those elements is less tedious than with vanilla JavaScript. You could just use React to do the same things you would do in vanilla JavaScript, but it excels at allowing a user to interact with a webpage.

### Quick Personal Advice
You'll still use regular old JavaScript for numerous things in React, as far as trying to use it practically and trying to pass the code challenges will go. It will be important to memorize *how* to do certain things in addition to understanding them on a fundamental level. I will go over those things when they come up.  

### JSX
You can write React in native JavaScript code, but there is a syntax called JSX (JavaScript Syntax) that allows you to be *declarative* in the way you write elements.

Basically, you say something like
```javascript
const element = <h1>Hello, world!</h1>;
```
(which is valid JSX), and JavaScript interprets this as 
```javascript
const element = /*#__PURE__*/React.createElement("h1", null, "Hello, world!");
```
Specifically, this interpretation is allowed by a transpiler called Babel, which is the thing that takes the JSX and turns it into JavaScript literal code.

JSX lets you *mix* html and JavaScript by writing things within {curly brackets} in your JSX. The curly brackets are telling JavaScript that whatever is inside needs to be interpreted as JavaScript.

```javascript
const formatName (user) => {
    return user.firstName + ' ' + user.lastName;
}

const user = {
    firstName: 'Bruce'
    lastName: 'Wayne'
};

const element = (
    <h1>
        Hello, {formatName(user)}!
    </h1>

)

// Wherever element is rendered, it would say Hello, Brurce Wayne!
```

The above example is using a function that takes an object as a parameter in {curly brackets} in the JSX.

[This is what Babel does under the hood, but you don't even REALLY need to know what's happening. Just how things evaluate.](https://babeljs.io/repl/#?browsers=defaults%2C%20not%20ie%2011%2C%20not%20ie_mob%2011&build=&builtIns=false&corejs=3.21&spec=false&loose=false&code_lz=MYewdgzgLgBApgGzgWzmWBeGAeAFgRgD4AJRBEAGhgHcQAnBAEwEJsB6AwgbgChRJY_KAEMAlmDh0YWRiGABXVOgB0AczhQAokiVQAQgE8AkowAUAcjogQUcwEpeAJTjDgUACIB5ALLK6aRklTRBQ0KCohMQk6Bx4gA&debug=false&forceAllTransforms=false&shippedProposals=false&circleciRepo=&evaluate=false&fileSize=false&timeTravel=false&sourceType=module&lineWrap=true&presets=react&prettier=false&targets=&version=7.18.9&externalPlugins=&assumptions=%7B%7D)

It's easier to declare elements in HTML format and add JavaScript here and there than to do all that `createElement` || `querySelector` || `append` nonesne.
You can set up a project as a react application by running `npx create-react-app` `project-name` in the desired parent directory. cd into `project-name` and run npm start--you will have a React development server up and running.

[Here's what's going on under the hood, courtesy of FreeCodeCamp](https://www.freecodecamp.org/news/create-react-app-npm-scripts-explained/#:~:text=Create%20React%20App%20(CRA)%20is,run%20it%20on%20the%20browser.)
## Why use react?

React makes it easier to render elements and update those elements dynamically compared to vanilla JavaScript. Thanks to what Babel does, you can write *declarative code* that will be transpiled into *imperative code*--and declarative code is easy to look at. It's also (allegedly) great at re-rendering only components who need to be re-rendered, allowing the page to *react* to events and user input in a seamless fashion. Even if you weren't taking advantage of this aspect of React, though, JSX and Babel make handling DOM elements smoother than JavaScript and HTML alone.

Think about it this way:

Your `index.html` file will be practically empty, save for a lone `div` in the body:

```html
<div id="root"></div>
```

Your `index.js` will also be practically be empty!

Except for this:

```javascript
import React from "react";
import ReactDOM from "react-dom";
import App from "./components/App";
import "semantic-ui-css/semantic.min.css";
import "./index.css";

ReactDOM.render(<App />, document.getElementById("root"));
```

`import` is not specific to React, but it may not have seen much use up to this point. It plays a significant role in connecting React components and enabling the use of React hooks. In the case of the `.index.js` file, the imports are ensuring that react and its pieces are accessible, and that a component called `App.js` is rendered on the DOM. 

The rendering, unsurprisingly, occurs with the render() method called on the last line:

```javascript
ReactDOM.render(<App />, document.getElementById("root"));
```

We saw the DOM element with `id='root'`, that was the sole `div` added to the HTML file.

### What does this all mean?

React is ready to go. You have connected a **component** called App to `index.html`, and anything you write in App will render in the DOM.

Sort of.

It's impossible to go any further without doing a deep dive into components. The initial takeaway, though, should be that a component will return an element that renders on the DOM. It can return as many elements as you want, as long as they are wrapped in one DOM element.

```javascript
const Component = () => {
    return (
        <div>
            This is fine.
        </div>
    )
}
```
```javascript
const AnotherComponent = () => {
    return (
        <div>
            <ul>
                <li>This is fine.</li>
                <li>This is also fine.</li>
                <li>
                    <span>T</span><span>HI</span><span>S</span>
                    IS ALSO FINE.
                </li>
            </ul>
        </div>
    )
}
```

## Components

You will generally write React components as functions with arrow syntax. Conventionally, they will have uppercase names. They will occupy their own separate files. If your component is called Form, your file should be called `Form.js`.

```javascript
const Banner = () => {
    return (
        <div className='Banner'>
            <h1>This is React!</h1>
        </div>
    )
}

export default Banner
```

Components **must** return a single JSX element. 

```javascript
const Banner = () => {
    return (
        <div className='Banner'>
            <h1>This is React!</h1>
        </div>
        <div>
            <h1>React will throw an error since you have two sibling divs!</h1>
        </div>
    )
}

export default Banner
```

**One** JSX element.

So, the first example will `return` this:

```javascript
<div className='Banner'>
    <h1>This is React!</h1>
</div>
```

And if we import `<Banner />` in another component, and call it in *that* component's return, we'll render `Banner`'s content in the exact spot that it is called.

Here is what that looks like:

```javascript
import Banner from './Banner'
const App = () => {
    return (
        <div className='App'>
            <Banner />
            <ul>
                <li>React</li>
                <li>Should</li>
                <li>Make</li>
                <li>Some</li>
                <li>Sense</li>
            </ul>
        </div>
    )
}
export default App
```

Two important things that will prevent future errors:

If you want to call `Banner` in a component like `App`, you need to do two things:

1. Make sure you `export default Banner` in `Banner`
2. Make sure you `import 'Banner' from 'RELATIVE-FILE-PATH'`

...relative file path means that it's like linking your css and javascript to your HTML.

Functional components are like variables. The functional component has to return a JSX element, and calling the functional component will render that JSX element in the spot it is called.

So if we were to write the above example *without* using a component, it would look like this:

```javascript
const App = () => {
    return (
        <div className='App'>
            <div className='Banner'>
               <h1>This is React!</h1>
           </div>
            <ul>
                <li>React</li>
                <li>Should</li>
                <li>Make</li>
                <li>Some</li>
                <li>Sense</li>
            </ul>
        </div>
    )
}
export default App
```
Note the inserted `<div>` and `<h1>` where <Banner /> used to be.

Components don't get that much more complicated. You establish them in their own files, import them in the file you want to display their contents, and call them where you want the component's returned JSX to show up.

Practice writing JSX, and importing and rendering child components.
## Props

Components are pretty neat by themselves. It's kind of like having a function in vanilla JavaScript that returns an HTML element, and setting that element's innerHTML to a combination of HTML and JavaScript.

But React cares about making components reusable. 

We already know how to make functions reusable.

```javascript 

const helloFunction = () => {
    console.log('Hello, World!')
}

helloFunction()
// This is not reusable. We can only say Hello, World

const sayFunction = (string) => {
    console.log(string)
}

sayFunction('HELLO, WORLD')
// This is more reusable. We can say whatever we pass into the function.
```

Functional components are literal functions. So if we want to make them a bit more dynamic, we can use parameters and pass arguments to get different results.

Let's look at our basic `<Banner />` component.

```javascript
// Banner.js

const Banner = ({ text }) => { // You HAVE to note the passed prop in as a parameter here
    return (
        <div className='Banner'>
           <h1>{text}</h1> {/*This is where we make use of the prop*/}
        </div>
    )
}
```
Look here. The text content of the `<h1>` element is no longer hard-coded to be anything. In fact, the only way for anything to render in that space, as written, is to pass text to Banner.

But, there's one issue. We don't call `Banner(text)`, we call `<Banner />`. 

This just requires slightly different syntax than a normal function.

It goes like this:

```javascript
// in App.js
<Banner text='This is called a prop' />
```

```javascript
// in Banner.js

// This is what happens with the passed 'text' property--it ends up serving as inner text
<div className='Banner'>
  <h1>This is called a prop</h1>
</div>
```
This is scratching the surface. You can pass more props. And your props can be any data type--numbers, booleans, objects--which enables you to perform JavaScript methods on the properties.

Let's do one slightly complex example as a demonstration.

```javascript
// App.js
import List from './List'


const App = () => {
    return (
        <div className='App'>
          <List />
          <NewFruitForm />
       </div>
    )
}
export default App
```

```javascript
// List.js

const fruits = ['Apple', 'Banana', 'Walnut']

import ListItem from './ListItem'
const List = () => {
    return (
        <div className='List-Item'>
           </ul>
              {fruits.map((fruit, index) => {
                return (
                    <ListItem 
                       key={index}
                       fruit={fruit} 
                    />
                )
              })}
           <ul>
        </div>
    )
}
export default ListItem
```
```javascript
const ListItem = ({ fruit }) => {
    return (
        <li>{fruit}</li>
    )
}
export default ListItem
```

Let's unpack what happened here.

`App` is here to show us the component structure. When we take a look at `List`, we see that there is an array declared called fruits. This is within the component file, but above the `return` line--this is something to break down later. 

We take one additional step in `List` by mapping over the array and then *returning* `<ListItem />`--and since .map() iterates over our array, we would expect it to return three `<ListItem />` elements.

Inside `ListItem` we just do this: `<li>{fruit}</li>` which is really nothing special.

This is something to practice a couple times until it makes sense.




