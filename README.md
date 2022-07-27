PHASE 1 REVIEW:

DOM Manipulation
Database Requests

PHASE 2 SUMMARY:

Mastering React

## Introducing JSX
https://reactjs.org/docs/introducing-jsx.html

JSX is a syntax extension to JavaScript. You essentially are able to bundle HTML elements with JavaScript.

```javascript
const element = <h1>Hello, world!</h1>
```

This seems useful enough, but it doesn't stop there.

```javascript
const formatName (user) => {
    return user.firstName + ' ' + user.lastName;
}

const user = {
    firstName: 'Harper'
    lastName: 'Perez'
};

const element = (
    <h1>
        Hello, {formatName(user)}!
    </h1>

)
```

What happened up here? Note that our element is no longer just an h1 tag with some inner text, it is now an h1 element with inner text that is the evaluation of a function!


The parentheses are optional, but they help with separation.


Valid JavaScript expressions wrapped in curly braces, or string literals can be used to specify attributes for HTML elements.

```javascript
const element = <img src={user.avatarUrl}></img>


const element = <a href='https://www.reactjs.org'> link </a>
```

### JSX Represents Objects

What goes on under the hood when creating elements in this way? There is a plugin called babel that compiles JSX down to React.createElement() calls.

Here are two examples that result in the same thing:

```javascript
const element = (
    <h1 className='greeting'>
        Hello, world!
    </h1>
)

```

```javascript
const element = React.createElement(
    'h1',
    {className: 'greeting'},
    'Hello, world!'
)
```

The first example is what we look at, and the second is what Babel is telling React to do with our instructions.

### Summary

JSX is a syntax extension that makes it easier to write and update HTML in React. Smarter people developed tools like Babel so that even I can wrap my head around how things work!

## Rendering Elements

This section covers one way to render elements to the DOM. Specifically, the way you would render elements to a root DOM node.

```HTML
<div id='root'></div>
```

```javascript
const root = ReactDOM.createRoot(
    document.getElementById('root')
)
const element = <h1>Hello, world!</h1>;
root.render(element)
```

Assuming the div is somewhere on the body of our HTML, we have instructed React to reference the div as with a variable name, and performed a method called .render() on that element.

This is similar to the DOM manipulation we've done before.

https://www.geeksforgeeks.org/explain-the-purpose-of-render-in-reactjs/#:~:text=React%20renders%20HTML%20to%20the,root%20component%20of%20our%20app.

Purpose of render():

- React renders HTML to the web page by using a function called render().
- The purpose of the function is to display the specified HTML code inside the specified HTML element.
- In the render() method, we can read props and state and return our JSX code to the root component of our app.
- In the render() method, we cannot change the state, and we cannot cause side effects( such as making an HTTP request to the webserver).

But, rather than render everything to the root of a page, you can use components.

## Components and Props

The purpose of components is to "split the UI into independent, reusable pieces."

Here's an example of a React element that represents a DOM tag:

```javascript
const element = <div />
```

And here is an example of a user-defined component:

```javascript
const element = <Welcome name="Sara">
```

Odd. We have something that is clearly not an HTML tag inside <> these. I'll try to break it down with the example:

```javascript
function Welcome(props) {
    return <h1>Hello, {props.name}</h1>
}

const root = ReactDOM.createRoot(document.getElementById('root'));
const element = <Welcome name="Sara" />
```

Our document has an element somewhere with the ID 'root.' Render(element) will display the specified HTML inside the target. The specified HTML is `<Welcome name="Sara">`. Welcome is a functional component.

The next thing to break down is the use of *props* here.

## What are Props, and Why Should I Care

I have way too many links shared here, because I took way too long to wrap my head around the concept of props.

https://www.freecodecamp.org/news/how-to-use-props-in-react/#:~:text=What%20are%20props%20in%20React,your%20app%20to%20be%20dynamic.

https://dmitripavlutin.com/react-props/

Props stands for properties--it's a shorter word (barely). Kind of hate it, but whatever. They are important for making reusable code, much in the way that parameters allow functions to serve general uses.

The more dynamic, the better! Right?

https://learning.flatironschool.com/courses/5207/assignments/194711?module_item_id=437855

A functional component can take props as an argument use those props to dynamically affect the JSX that is returned.

In order for components to serve as a template of JSX rather than a bunch of hard-coded information, you must use props.

https://www.geeksforgeeks.org/reactjs-defaultprops/#:~:text=The%20defaultProps%20is%20a%20React,default%20props%20for%20the%20class.

Here's a tiny example of props being used by a child component.

```javascript
function ExampleFunction(props) {
    return <div>{props.value}></div?>
}
```

The important thing, though, is that the point is to use parent components to specify properties so that the prop-using children can render different things.

```javascript
// ParentComponent.js
import React, { Component } from 'react'
import ChildComponent from './childComponent'

class ParentComponent extends Component {
    render () {
        return (
            <div>
              <h1>I'm the parent component</h1>
              <h3><ChildComponent /></h3>
            </div>
        )
    }
}

export default ParentComponent
```

Alright, in the above example, ParentComponent is calling ChildComponent. In the h3 tag. Calling a component is similar to writing an HTML tag, you use `<these guys>` but you `<Capitalize />` the first letter. Referencing ChildComponent here is like using a variable. You are telling React to replace ChildComponent with whatever ChildComponent returns.

```javascript
// childComponent.js

const ChildComponent = () => {
    return (
        <p>I'm the 1st child</p>
    )
}
```

In other words, our h3 will enclose a `<p>` tag with inner text "I'm the 1st child".

When you call a child component from within a parent component, you can specify properties in order to change what the child component returns. The child component must be set up to take the property in, and the call for the child from the parent component should specify the properties that should be rendered. These specific details are passed as props.

It seems *a little* backwards.

So, how might we be more dynamic in the above example?

Let's think about the purpose of the child and parent components.

Assume that I wanted to be able to create `<p>` tags within `<h3>` elements, and provide each `<p>` tag with different text.

This is what I would do:

```javascript
// assuming you have ParentComponent and ChildComponent in the same folder
import React from 'react'
import ChildComponent from './childComponent'

class ParentComponent extends Component {
    render () {
        return (
            <div>
              <h1>I'm the parent component</h1>
              <h3><ChildComponent text="I'm the 1st Child"/></h3>
              <h3><ChildComponent text="I'm the 2nd Child"/></h3>
              <h3><ChildComponent text="I'm the 3rd Child"/></h3>
            </div>
        )
    }
}

export default ParentComponent
```

I'm specifying different text for each of these child components within the parent component.

But, I have to update the behavior of the child component function in order for anything to actually render.

```javascript
// childComponent.js

const ChildComponent = (props) => {
    return (
        <p>{props.text}</p>
    )
}
```
## WHEN TO USE PROPS

If you're out here trying to *dynamically* render things then maybe you need to use some *props*.

Conceptually, you are "passing down" information to child components--which seems to just mean that you are *specifying* details in the parent. These details are "passed" to the children, who then figure out what they are supposed to do with the info.

```javascript
// ChildThatWasReferenced.js
console.log(props) //=> ''
```

By itself, this would be meaningless.

But, if a parent component passes down properties to the child component, then console.log(props) should print the things that were passed down. 

```javascript
// Parent.js
<ChildThatWasReferenced
  propertyA="123"
  propertyB="456"
  propertyC="789"
/>
```
...and back to our Child page...

```javascript
// ChildThatWasReferenced.js
console.log(props) //=> {propertyA: 123, propertyB: 456, propertyC: 789}
```

Sensible. 

So if I want to go back to the example from earlier...

```javascript
function Welcome(props) {
    return <h1>Hello, {props.name}</h1>
}

const root = ReactDOM.createRoot(document.getElementById('root'));
const element = <Welcome name="Sara" />
```

Welcome is a functional component that allows a .name property to be used in a `'Hello, {}'` string. The component returns an `<h1>` element with this string as inner text, and that element is presumably appended to the root div thing. The child component is set up to use the property that is specified within the parent element.

Gods.

In a way, the child components are kind of like function declarations, and they are invoked within a parent component that specifies properties. For a child component to allow the use of properties, you must pass props as an argument in the declaration and presumably use the property in the component.

## Day 2

Morning Drills.
```javascript
// - Create a function component called App (using arrow function syntax) 3 times!
// - App should return a div with an h1 that says "Hello React" inside
// - Be sure to export the component at the bottom
```

```javascript
// solution App.js
const App = () => {
    return (
        <div>
          <h1>Hello, React!</h1>
        </div>
    )
}
export default App
```

```
- Create a component called Counter
- It has state called "count" and a setter function that you can name whatever you like from useState
- The Counter component returns a div with an h1 that says Count is {count}
- Below the h1, put a button that increments the counter by 1 every time the button is CLICKED
```
```javascript
// solution Counter.js
import React, {useState} from 'react'
const Counter = () => {
    const [count, setCount] = useState(0)
    return (
        <div>
          <h1>Count is {count}</h1>
          <button onClick={() => setCount(count + 1)}>
          Click Me!
          </button>
        </div>
    )
}

export default Counter
```
```javascript
// example of App.js that would display Counter
// App.js
import Counter from './Counter'

const App = () => {
    return (
        <div>
          <Counter />
        </div>
    )
}

export default App
```
- Create a function component called App, exactly the same way as above, except the App component will now return a div with a component called UserProfile in it. Don't forget to import the UserProfile component at the top
- Write a separate function component called UserProfile
it takes a prop called "user"
- In UserProfile, it returns a div with an h2 that says "Profile"
- Underneath the h2, it has a p tag that has the user's email
- underneath that, it has a p tag that has the user's bio
```javascript
// App.js solution

import React from 'react'
import UserProfile from './UserProfile'

const App = () => {
    return (
        <div>
          <UserProfile />
        </div>
    )
}

export default App
```
```javascript
// UserProfile.js
import React from 'react'

const UserProfile = (props) => {
    return (
        <div>
          <h2>Profile</h2>
          <p>{props.email}</p>
          <p>{props.bio}</p>
        </div>
    )
}

export default UserProfile
```

### Props Destructuring and Default Values

We use props to make components more reusable.

Here's an example of an inflexible functional component.

```javascript
const MovieCard = () => {
    return (
        <div className="movie-card">
          <img
          src="https://upload.wikimedia.org/wikipedia/en/4/4c/Lost_in_Translation_poster.jpg">
          alt="Lost in Translation"
          />
          <h2>Lost in Translation</h2>
          <small>Genres: Romantic Comedy-Drama</small>
        </div>
    )
}
```

This component is hard-coded to display information about a great 2003 film, but isn't usable for much else. If we wanted to display an image and info for multiple  movies, we *could* keep making new components, or we could make this component dynamic.

This is how that might look:

```javascript
// App.js
import MovieCard from './MovieCard'
const App = () => {
    const title = "Lost in Translation"
    const posterUrl = "https://upload.wikimedia.org/wikipedia/en/4/4c/Lost_in_Translation_poster.jpg"
    const genresArr = [Romantic Comedy-Drama, Drama]

    return (
        <div className="App">
          <MovieCard title={title} posterSrc={posterUrl} genres={genresArr} />
        </div>

    )
}
```
```javascript
// child component MovieCard.js
const MovieCard = (props) => {
    return (
        <div className="movie-card">
          <img src={props.posterSrc} alt={props.title} />
          <h2>{props.title }</h2>
          <small>{props.genres.join(" , ") }</small>
        </div>
    )
}
```

Not actually that different. The main thing is that specific details are replaced by the props keyword with the named data for the value(s) coming from the parent component.

Naturally, since we barely have a grasp of using props in the first place, we should learn how to destructure props so that they can be represented as variables instead of `props.whatever`.

```javascript
// Without Destructuring
const MovieCard = (props) => {
    return (
        <div className="movie-card">
          <img src={props.posterSrc} alt = {props.title} />
          <h2> {props.title} </h2>
          <small> {props.genres.join(", ")} </small>
        </div>
    )
}
```

```javascript
// With Destructuring
const MovieCard = ({title, posterSrc, genres}) => {
    return (
        <div className="movie-card">
          <img src={posterSrc} alt={title} />
          <h2>{title}</h2>
          <small>{genres.join(", ")}</small>
        </div>
    )
}
```

In the top example, we pass props as an argument to MovieCard. This is what we've seen so far. In the bottom example, we destructure props into `title`, `posterSrc`, and `genres`.

Not too hard to understand. I assume our props object needs to actually have these values in order to do anything with them. But, this means that if we name properties in a parent component, we can pass the key names into the child components and refer to them that way, instead of props.title, etc.

### Conditional Rendering

https://reactjs.org/docs/conditional-rendering.html

Remember those If-Else statements from JavaScript? Guess what, you can't use them inside JSX. Instead, you are supposed to use ternary operators, which dont share that issue.

https://react-cn.github.io/react/tips/if-else-in-JSX.html

You can use If statements outside of the returned JSX (like before the return block) if a ternary operator is not the right solution. Same goes for switch statements.

```javascript
render() {
    const isLoggedIn = this.state.isLoggedIn
    return (
        <div>
          {isLoggedIn
            ? <LogoutButton onClick={this.handleLogoutClick} />
            : <LoginButton onClick={this.handleLoginClick} />
            }
        </div>
    )
}
```

Straightforward. I'll leave out examples of using other conditonal rendering before the return statement, since that's just vanilla JavaScript.

### React Fragments

React components must return a single JSX element. Wrapping multiple elements in an outer container such as a div allows you to avoid making a separate component for every related element, but sometimes the wrapper element serves no purpose. If that is the case, you can use `<React.Fragment> <JSX> <React.Fragment>` to prevent an outer element from being rendered.

You can even use `<React.Fragment> tags to wrap things *inside* other tags in order to have a non-rendered, key-bearing wrapper. We'll probably learn precisely when to do this later, but for now, it's enough to think of it as a way of reducing unnecessary elements from existing on a page.

```javascript
const Bookshelf = (props) => {
    return (
        <section>
          {props.books.map((book) => (
            <React.Fragment key ={book.id}>
              <h1>{book.title}</h1>
              <h2>{book.author}</h2>
            </React.Fragment>
          ))}
        </section>
    )
}
```

### Day 3 Drills

```javascript
// Write a component called NewsFeed that returns a div that has 2 child components called Post. Be sure to export and import your files properly.
// Write a component called Post that takes accepts 2 props called "likes" (a number) and "image_url" (a string). The Post component should return a div that has an img tag and a paragraph tag that implements the given props in a manner that makes sense.
```

```javascript
// Newsfeed.js
import React from 'react'
import Post from './Post'

const NewsFeed = () => {
    return (
        <div>
          <Post likes={""} img_url={""} />
          <Post likes={""} img_url={""} />
        </div>
    )
}

export default NewsFeed
```

```javascript
// Post.js
import React from 'react'

const Post = ({likes, img_url}) => {
    return (
        <div>
          <img src={img_url}>
          <p> {likes == 1
          ? `${likes} like`
          : `${likes} likes`
          }</p>
        </div>
    )
}

export default NewsFeed
```
I'm leaving out the drawing, but the point is that NewsFeed.js is a parent, Post.js is a child, and the props specified in NewsFeed.js are dynamically rendered according to the return value of Post.js.

## Event Handling in React

React allows you to attach event handlers directly to DOM elements via attributes like onClick, onChange, and onSubmit. Note that you cannot give these attributes to React components directly.

```javascript
<button onClick={() => setCount(count + 1)}>
```

Remember this thing? It honestly doesn't need to be more complicated than that.

```javascript
<DOMelement onEvent={(event) => function }>
```

You can write the function then and there with arrow syntax or invoke a callback function. It pays to be abstract, so write the function outside where possible.


# State State State State

https://learning.flatironschool.com/courses/5207/assignments/194752?module_item_id=437889

https://beta.reactjs.org/learn/managing-state

What is state in the fewest words possible? State is data that responds to changes or user input. Props are dynamic in that you can specify properties from a parent element and pass that information to child components, but states allow child components to maintain and update information without involving the parent.

Because components are separated, there is probably some kind of processing benefit to this.

How does it work?

We only barely need to understand what's going on under the hood thanks to the existence of a React Hook called useState. Yep, React essentially does all of the work.

```javascript
import React, { useState } from 'react'

const FunctionalComponent = () => {
    const [value, setterFunction] = useState(defaultValue)

    return (

    )
}
```

Ok, that's the basics. A supremely simple example can be found in the Day 2 drills:

```javascript
// solution Counter.js
import React, {useState} from 'react'
const Counter = () => {
    const [count, setCount] = useState(0)
    return (
        <div>
          <h1>Count is {count}</h1>
          <button onClick={() => setCount(count + 1)}>
          Click Me!
          </button>
        </div>
    )
}

export default Counter
```
```javascript
// example of App.js that would display Counter
// App.js
import Counter from './Counter'

const App = () => {
    return (
        <div>
          <Counter />
        </div>
    )
}

export default App
```

There is a reason to create a function that incorporates the setter function rather than invoking it directly. We'll adjust some code later and explain why.

If you think back to vanilla JavaScript, we would have needed to create/get the button element and the element that would display the count info as inner text, attach an event listener, increment the value, and then update the element with the count info.

The cool stuff happens here:

```javascript
<button onClick={() => setCount(count + 1)}>
```

Let's break down what would happen upon the first click:

1. `setCount(count + 1)` evaluates to `setCount(1)` which tells React's internal state must update count to 1.
2. The internal state is updated.
3. The entire `Counter` component is re-rendered.
4. `useState`'s value is now 1.
5. `count` is "set" to 1.
6. `count` is rendered as 1.

Honestly, you could summarize some of those steps. But this is the gist of state: you are able to tell React to render a default value, and that if the setter function is invoked, the page needs to be re-rendered with a new value for the state-dependent variable.

## Setting State is Apparently Asynchronous

JavaScript saves time by running synchronous code before running asynchronous code. This could cause an issue with console messages and other synchronous behaviors. You avoid problems by using callback syntax, in a very un-pretty way (IMO).

Example:
```javascript
const Counter = () => {
    const [currentCount, setCount] = useState(0)

    const increment = () => {
        setCount((currentCount) => currentCount + 1)
        setCount((currentCount) => currentCount + 1)
    }
}
```
Ugh, nasty. Hate to look at it. But apparently using this callback syntax means that React will not look at the state of currentCount and add 1, it will look at the previous state of currentCount and add 1. This, I guess, gets around the idea that the React doesn't wait to update the current state but it WILL wait to get the value of the previous state. 

And apparently you can use something called useEffect instead. But we aren't there yet!

### Notes on State

According to canvas, you should only use state sparingly. Only for values that are expected to change over the course of a component's life. I am doubtful that we know enough to properly demonstrate this concept, so, I'm moving on.


### Lifting State Up

Is your state uplifted? Mine isn't.

If you have multiple components that rely on one state, you can lift the state to the lowest common parent component, or closest common ancestor in order for the state to be accessible.

Anya showed me a great example of lifted state in action. Any components that are expected to render or set state need to have access to state or the setter function. Child components can get access by having it passed down from a parent component. You don't need to establish state anywhere it isn't being used, and the economical choice is to declare state in the closest common ancestor.

It isn't hard. It's basically the exact same as passing properties.

## Information Flow

A child component can also send data up to the parent component by sending a callback function as props from parent to child. "Ownership" of the callback function belongs to the parent component, and data/state changes flow up to the owner, rather than affecting the child that invokes the callback function.

Word salad.

As usual, let us look at an example. The goal is to have this relationship:

```
App
└───Parent
    ├───Child
    └───Child
```
and allow clicking on either child to change the color of the parent component.

First, a state variable called color will live inside the parent component.

```javascript
// Parent.js
import React, { useState } from "react";
import { getRandomColor } from "./randomColorGenerator.js";
import Child from "./Child";

const Parent = () => {
    const randomcolor = getRandomColor()
    const [color, setColor] = useState(randomColor) // initial value for color state

    const handleChangeColor = () => {
        const newRandomColor = getRandomColor()
        setColor(newRandomColor) // update color state to a new value
    }

    return (
        <div className="parent" style={{ backgroundColor: color }}>
           <Child onChangeColor={handleChangeColor} />
           <Child onChangeColor={handleChangeColor} />
        </div>
    )
}

export default Parent
```

A quick rundown. Parent is a component function that will probably be called in an App.js component. Instead of `useState(getRandomColor())` we say `useState(randomColor)` where `randomColor = getRandomColor()` because callback syntax to avoid async problems. *Any time you need to set state based on the current value of state, you should use the callback syntax.* 

Here's what the child components should look like:

```javascript
// Child.js
import React from "react";

const Child = ({ onChangeColor }) => {
  console.log(onChangeColor)
  return (
    <div 
      onClick={onChangeColor}
      className='child'
      style={{ backgroundColor: '#FFF' }}
      />
  )
}

export default Child;
```

So, with all of our pages linked and exported and imported, we should see that clicking on the child components indeed causes the parent function to change state. This is an example of inverse flow or whatever. We passed a property called `onChangeColor`, whose value is a function called `handleChangeColor` to the child component. In the child component, we destructure/rename the function to `onChangeColor`. On click, the function `onChangeColor` (which is the `handleChangeColor` function) fires, which causes the setter function to set a new random color for the parent.

A bit exhausting, but, not that hard to really *get*. You pass the setter function to the child component, and when you invoke the setter function in the child, it updates the state of the parent.

Fun stuff. Now let's really dive into it.

I'm going to rewrite Parent.js and Child.js (slightly) to accomplish this deliverable:

```
- When either Child component is clicked, it should change its own background color to a random color, and the other Child component should change to that same color.
```

This isn't exactly as simple as the first part, because sibling components cannot directly pass data to each other. The workaround for this is **storing the color of the `Child` in the state of the `Parent` component.** The `Parent` component will handle the passing of data to the children.

```javascript
// Parent.js
import React, { useState } from "react";
import { getRandomColor } from "./randomColorGenerator.js";
import Child from "./Child";

const Parent = () => {
    const randomColor = getRandomColor()
    const [color, setColor] = useState(randomColor)
    const [childrenColor, setChildrenColor] = useState('#FFF')

    const handleChangeColor = (newChildColor) => {
        const newRandomColor = getRandomColor()
        setColor(newRandomColor)
        setChildrenColor(newChildColor)
    }

    return (
        <div className="parent" style={{ backgroundColor: color }}>
           <Child color={childrenColor} onChangeColor={handleChangeColor} />
           <Child color={childrenColor} onChangeColor={handleChangeColor} />
        </div>
    )
}

export default Parent
```

```javascript
// Child.js
import React from "react";
import { getRandomColor } from "./randomColorGenerator.js";

const Child = ({ onChangeColor, color }) => {
  const handlelick = () => {
    const newColor = getRandomColor()
    onChangeColor(newColor)
  }
  return (
    <div
      onClick={handlelick}
      className='child'
      style={{ backgroundColor: color }}
    />
  )
}

export default Child;
```

So, uh, it compiles, but that is for sure confusing.

```javascript
// randomColorGenerator.js

// Nothing in this file needs to be altered (but it is your solution so feel free to!)
export function getRandomColor() {
  // this function generates a random hex color from the letters below
  const letters = "123456789AB"; // <-- cutting off top end so we get lighter colors
  let color = "#";
  for (let i = 0; i < 3; i++) {
    color += letters[Math.floor(Math.random() * 11)];
  }
  return color;
}
```

This is a randomly generated string, essentially.

Next step should be applying many of these concepts. Probably practicing code challenges/labs and trying to build ONE thing.

### A note on Information Flow

You *COULD* pass the entire setter function down to child components and achieve the desired results. To avoid giving that level of control to the children, though, you can pass a function that invokes the setter function in the way that it is intended to update the state--you don't necessarily want the child to interact with state directly. If all they need is to be able to update the state in the way state is handled by the parent, you should use a callback function!!!!!


## Day 4 Drills

```javascript
// Write a React component called App returns a div with an h1 that has the innerText of "Game Show", a button with the inner text of "Click me" and an h3 that starts empty, and an input with the type of "text". App has a 2 pieces of state: The first one is a counter variable called "score" that increments by 1 point every time the button element is clicked. The second state is a text variable called "username" that will update the h3 element's inner text to reflect whatever the user enters into the input element.
```

```javascript
import React, { useState } from 'react';
import './App.css';

const App = () => {
  const [score, setScore] = useState(0)
  const [username, updateUser] = useState('')

  const handleScore = () => {
    setScore(score + 1)
  }

  const handleUser = (e) => {
    updateUser(e.target.value)
  }

  return (
    <div>
      <h1>Game Show</h1>
      <button onClick={handleScore}>Click Me!</button>
      <h4>Score: {score}</h4> // optional
      <h3>{
      username
      ? username
      : ''
      }</h3>
      <input type='text' onChange={(e) => handleUser(e)}></input>
    </div>
  );
}

export default App;
```

Look at me, I used handler functions.

### Controlled Components

Controlled components derive vlaues from state.

Here is an example of a controlled form:

```javascript
import React, { useState } from 'react'

const Form = () => {
  const [firstName, setFirstName] = useState('John')
  const [lastName, setLastName] = useState('Henry')

  return (
    <form>
      <input type='text' value={firstName} />
      <input type='text' value={lastName} />
      <button type='submit'>Submit</button>
    </form>
  )
}
```
The inputs in this example determine their value attributes according to state. Our two states are hard-coded as two strings, with no way to be updated. If we are to update state when input values are changed, you might expect that we should render the states on the page.

To do so in a React-friendly way, we should make a bunch of separate components, and import them into the parent, etc etc etc.

```javascript
// ParentComponent.js
import react, { useState } from 'react'
import Form from './Form'
```

The handler function can look like this:

```javascript
const handleSubmit = (e) => {
  e.preventDefault()
  const newItem = {
    id: uuid(),
    name: itemName,
    category: itemCategory,
  }
  console.log(newItem)
}

```

The benefit of all of this is that your input values are actually being determined by state, which gets updated by user input. Can't react much faster than that.

### Princple of Least Privilege

Only give access to data from a parent component to child components that need the data. Give the most specific version of that data available to avoid possible privilege issues. Avoid giving "carte blanche" access or "giving your child your unlocked phone."

## Day 5 Recap

Will recap video as soon as it is posted.

## Day 6

### Updating Arrays and Objects with useState

You must create a copy of the array/object when attempting to create a new state based on modifications to that array/object. If you try to mutate an array/object directly, and then set the value of state to be the array/object, React will interpret the reference that the setter function is making as unchanged from the state value and will not re-render anything. *Or something*. Saying setState(state) signals that no updates are required.

In other words, this example doesn't work:

```javascript
const defaultState = {
  key: ''
}
const [state, setState] = useState(defaultState)

const updateState = () => {
  state[key] = 'new value'
  setState(state)
}
```

But, this would work:
```javascript
const actuallyUpdateState = () => {
  setState({
    ...state,
    [key: 'new value']
  })
}
```

Here are generic examples for updating both arrays and objects.

```javascript
// Adding to an array
const handleAdd = (todo) => {
  setTodos([...todos, todo]);
}

//Removing from an array

const handleRemove = (todo) => {
  const newTodos = todos.filter((t) => t !== todo);
  setTodos(newTodos);
}


// Updating an array

const handleUpdate = (index, todo) => {
  const newTodos = [...todos];
  newTodos[index] = todo;
  setTodos(newTodos);
}
```

And for objects:

```javascript
// Adding to an object
const handleAdd = (todo) => {
  setTodos({...todos, [todo.id]: todo});
}

// Removing from an object
const handleRemove = (todo) => {
  const newTodos = {...todos}
  delete newTodos[todo.id]
  setTodos(newTodos);
}

// Updating an object
const handleUpdate = (todo) => {
  setTodos({...todos, [todo.id]: todo});
}
```

### Updating State Values Self-Referentially

```javascript
// Potentially problematic example included:
import React, { useState } from "react";

const initialFormState = {
  name: "",
  about: "",
  phase: "",
  link: "",
  image: ""
}
const ProjectForm = ({ onAddProject }) => {
  const [formData, setFormData] = useState(initialFormState)

  const handleOnChange = (e) => {
    const { name, value } = e.target;

// This is bad:
    setFormData({
      ...formData,
      [name]: value
    })

  }

// This would be better:
/*setFormData(formData => {
      return {
        ...formData,
        [name]: value
      }
    })

*/

  const handleSubmit = (e) => {
    e.preventDefault();
    console.log(formData);
    onAddProject(formData);
    setFormData(initialFormState);
  }

  return (
    <section>
      <form className="form" autoComplete="off" onSubmit={handleSubmit}>
        <h3>Add New Project</h3>

        <label htmlFor="name">Name</label>
        <input
          type="text"
          id="name"
          name="name"
          value={formData.name}
          onChange={handleOnChange}
        />

        <label htmlFor="about">About</label>
        <textarea
          id="about"
          name="about"
          value={formData.about}
          onChange={handleOnChange}
        />

        <label htmlFor="phase">Phase</label>
        <select
          name="phase"
          id="phase"
          value={formData.phase}
          onChange={handleOnChange}
        >
          <option>Select One</option>
          <option value="1">Phase 1</option>
          <option value="2">Phase 2</option>
          <option value="3">Phase 3</option>
          <option value="4">Phase 4</option>
          <option value="5">Phase 5</option>
        </select>

        <label htmlFor="link">Project Homepage</label>
        <input
          type="text"
          id="link"
          name="link"
          value={formData.link}
          onChange={handleOnChange}
        />

        <label htmlFor="image">Screenshot</label>
        <input 
          type="text" 
          id="image" 
          name="image"
          value={formData.image}
          onChange={handleOnChange}
        />

        <button type="submit">Add Project</button>
      </form>
    </section>
  );
};

export default ProjectForm;
```

There's a lot more going on in this example than the one concept about which I commented, but the point is to compare the two uses of the setter function.

If you're immediately setting state with the setter function (which seems like a desired outcome) and not using a handler function, you should do a callback function.

```javascript
setState(objectState => {
  return (
    ...objectState,
    [keyToChange]: valueToBeSet
  )
})
```

It, uh, has to do with async functions queuing up, I guess.


## Using Controlled Components, Fetch Requests and Re-renders

```javascript
// within App.js
import { useState, useEffect } from "react";

import Header from "./components/Header";
import ProjectForm from "./components/ProjectForm";
import ProjectList from "./components/ProjectList";

const App = () => {
  const [projects, setProjects] = useState([]);
  const [isDarkMode, setIsDarkMode] = useState(true);
  const [selectedPhase, setSelectedPhase] = useState("");
  const [searchQuery, setSearchQuery] = useState("");

  const onAddProject = (newProject) => {
    setProjects(projects => {
      return [...projects, newProject]
    })
  }

  const onToggleDarkMode = () => {
    setIsDarkMode(isDarkMode => !isDarkMode)
  }

  useEffect(() => {
    console.log('side effect')
    let url;
    if (selectedPhase && searchQuery) {
      url = `http://localhost:4000/projects?phase=${selectedPhase}&q=${encodeURI(searchQuery)}`;
    } else if (searchQuery) {
      url = `http://localhost:4000/projects?q=${encodeURI(searchQuery)}`;
    } else if (selectedPhase) {
      url = `http://localhost:4000/projects?phase=${selectedPhase}`;
    } else {
      url = "http://localhost:4000/projects";
    }
    fetch(url)
      .then((res) => res.json())
      .then((projects) => setProjects(projects));
  }, [selectedPhase, searchQuery])

  console.log('rendering');
  return (
    <div className={isDarkMode ? "App" : "App light"}>
      <Header isDarkMode={isDarkMode} onToggleDarkMode={onToggleDarkMode} />
      <ProjectForm onAddProject={onAddProject}  />
      <ProjectList
        projects={projects}
        setSelectedPhase={setSelectedPhase}
        searchQuery={searchQuery}
        setSearchQuery={setSearchQuery}
      />
    </div>
  );
};

export default App;
```
```javascript
// ProjectForm
import { useState } from 'react'

const initialFormState = {
  name: '',
  about: '',
  phase: '',
  link: '',
  image: '',
}

const ProjectForm = ({onAddProject}) => {
  const [formData, setFormData] = useState(initialFormState)
  
  const handleOnChange = (e) => {
    const {name, value} = e.target
    setFormData(formData => {
      return {
        ...formData,
        [name]: value
      }
    })
  }

  const handleSubmit = (e) => {
    e.preventdefault()
    fetch({/*projectsUrl*/}, {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json'
      },
      body: JSON.stringify(formData)
    })
    .then(req => req.json())
    .then(newProject => {
      onAddProject(newProject)
    })
    setFormData(initialFormState)
  }
  return (
    <section>
      <form className='form' autoComplete='off' onSubmit={handleSubmit}>
        <h3>Add New Project</h3>
        <label htmlFor='name'>Name</label>
        <input
        type='text'
        id='name'
        name='name'
        value={formData.name}
        onChange={handleOnChange}/>
        <label htmlFor='about'>About</label>
        <input
        type='text'
        id='about'
        name='about'
        value={formData.about}
        onChange={handleOnChange}/>
        <label htmlFor='phase'>Phase</label>
        <select
          name='phase'
          id='phase'
          value={formData.phase}
          onChange={handleOnChange}
        >
          <option>Select One</option>
          <option value={1}>Phase 1</option>
          <option value={2}>Phase 2</option>
          <option value={3}>Phase 3</option>
          <option value={4}>Phase 4</option>
          <option value={5}>Phase 5</option>
        </select>
        <label htmlFor="link">Project Homepage</label>
        <input
          type="text"
          id="link"
          name="link"
          value={formData.link}
          onChange={handleOnChange}
        />
        <label htmlFor="image">Screenshot</label>
        <input 
          type="text" 
          id="image" 
          name="image"
          value={formData.image}
          onChange={handleOnChange}
        />
        <button type="submit">Add Project</button>
      </form>
    </section>
  );
};

export default ProjectForm

```

```javascript
// ProjectList.js
import ProjectListItem from "./ProjectListItem";
import { useState, useEffect } from 'react';

const ProjectList = ({
  projects,
  setSelectedPhase,
  searchQuery,
  setSearchQuery
}) => {
  const [searchInputText, setSearchInputText] = useState("");

  const searchResults = projects.filter((project) => {
    return project.name.toLowerCase().includes(searchQuery.toLowerCase());
  });

  const projectListItems = searchResults.map((project) => (
    <ProjectListItem key={project.id} {...project} />
  ));

  const handleOnChange = (e) => setSearchInputText(e.target.value);

  useEffect(() => {
    const scheduledUpdate = setTimeout(() => {
      setSearchQuery(searchInputText);
    }, 300)
    console.log('effect running');
    return () => {
      console.log('cleanup running');
      clearTimeout(scheduledUpdate);
    }
  }, [setSearchQuery, searchInputText])

  console.log('rendering');
  return (
    <section>
      <h2>Projects</h2>
      {/* <h1>Count: {count}</h1> */}
      <div className="filter">
        <button onClick={() => setSelectedPhase("")}>All</button>
        <button onClick={() => setSelectedPhase("5")}>Phase 5</button>
        <button onClick={() => setSelectedPhase("4")}>Phase 4</button>
        <button onClick={() => setSelectedPhase("3")}>Phase 3</button>
        <button onClick={() => setSelectedPhase("2")}>Phase 2</button>
        <button onClick={() => setSelectedPhase("1")}>Phase 1</button>
      </div>
      <input type="text" placeholder="Search..." onChange={handleOnChange} />

      <ul className="cards">{projectListItems}</ul>
    </section>
  );
};

export default ProjectList;
```

Dense stuff, honestly. Would be difficult to replicate quickly without copying the components 10 times...

The meat of things lies within the `handleSumbmit` function. It takes care of the fetch request and invoking the re-render based on the result of the fetch request.

```javascript
const handleSubmit = (e) => {
  e.preventDefault();
  // makes a POST request using a state variable
  fetch({/*url*/}, {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json'
    },
    body: JSON.stringify(formData)
  })
  // turns the result of the promise back into JS
  .then(req => req.json())
  // invokes the handler function for adding a new project which updates the projects state (an array)
  // this leads to a rerender
  .then(newProject => {
    onAddProject(newProject)
  })
  // resets the form values
  setFormData(initialFormState)
}
```

## Updating Arrays in React

```javascript

import { useState } from 'react'

const ExampleComponent = () => {
  const [array, setArray] = useState([])

  const handleAddElement = (newElement) => {
    const updatedArray = [...array, newElement]
    setArray(updatedArray)
  }

  const handleRemoveElement = (idToRemove) => {
    const updatedArray = array.filter((target) => target.id !== idToRemove)
    setArray(updatedArray)
  }

  const handleUpdateElement = (updatedElement) => {
    const updatedArray = array.map((element) => {
      if (element.id === updatedElement.id) {
        return updatedElement
      } else {
        return element
      }
    })
    setArray(updatedArray)
  }
}
```

Once more, for posterity.

```javascript

import { useState } from 'react'

const ExampleComponent = () => {
  const [array, setArray] = useState([])

  const handleAddElement = (newElement) => {
    const updatedArray = [...array, newElement]
    setArray(updatedArray)
  }

  const handleRemoveElement =  (idToRemove) => {
    const updatedArray = array.filter((target) => target.id !== idToRemove)
    setArray(updatedArray)
  }

  const handleUpdateElement = (updatedElement) => {
    const updatedArray = array.map((element) => {
      if (element.id === updatedElement.id) {
        return updatedElement
      } else {
        return element
      }
    })
    setArray(updatedArray)
  }
}

```
Honestly, I don't even know if the above code works! There could be a single error that I keep repeating in my practice. That's not fun. I'll test everything later.

For now, though, let's write everything two more times.

```javascript
import { useState } from 'react'

const ExampleComponent = () => {
  const [array, setArray] = useState([])

  const handleAddElement = (newElement) => {
    const updatedArray = [...array, newElement]
    setArray(updatedArray)
  }

  const handleRemoveElement = (idToRemove) => {
    const updatedArray = array.filter((target) => target.id !== idToRemove)
    setArray(updatedArray)
  }

  const handleUpdateElement = (updatedElement) => {
    const updatedArray = array.map((element) => {
      if (element.id === updatedElement.id) {
        return updatedElement
      } else {
        return element
      }
    })
    setArray(updatedArray)
  }
}
```

```javascript
import { useState } from 'react'

const ExampleComponent = () => {
  const [array, setArray] = useState([])

  const handleAddElement = (newElement) => {
    const updatedArray = [...array, newElement]
    setArray(updatedArray)
  }

  const handleRemove = (idToRemove) => {
    const updatedArray = array.filter((target) => target.id !== idToRemove)
    setArray(updatedArray)
  }

  const handleUpdateElement = (updatedElement) => {
    const updatedArray = array.map((element) => {
      if (element.id === updatedElement.id) {
        return updatedElement
      } else {
        return element
      }
    })
    setArray(updatedArray)
  }
}

```

Looks good enough. Time to move on.

### Using Inputs As Search Forms

Conceptually, you are limiting what is rendered to the page to items whose name inlcudes the value of the search box.

In order to do this, you need the following:

A state variable that determines what renders
A callback function that filters the array of possible elements by the value of the state variable
A child component with an input to serve as the listener for that callback function
A child component to display the elements

Here's how this works out in the plant mock:

```javascript

// Note that plants is an array of plants we manage in state/pull from the database
// State Variable:

const [searchTerm, setSearchTerm] = useState('')

// Filter function

const searchedPlants = plants.filter((plant => {
  return plant.name.toLowerCase().includes(searchTerm.toLowerCase())
}))

// Pass these as props:

  <Search searchTerm={searchTerm} setSearchTerm={setSearchTerm} />
  <PlantList plants={searchedPlants} />

```

```javascript
// Here's what's in our Search.js

const Search = ({searchTerm, setSearchTerm}) => {
  return (
    <div className="searchbar">
      <label htmlFor="search">Search Plants:</label>
      <input
        type="text"
        id="search"
        placeholder="Type a name to search..."
        value={searchTerm}
        onChange={(e) =>  {
          console.log("Searching...")
          setSearchTerm(e.target.value)
        }}
      />
    </div>
  );
}

export default Search;
```

Not a difficult read, you just need to know what to set up. We use controlled components again. The input updates searchTerm on change, which in turn updates the value of the input. setSearchTerm will trigger a re-render of PlantList on each of the input changes, and will render the value of searchTerm as the current input value.

This is particularly important for the variable searchedPlants, which is what actually uses the search term to change the array of rendered plants. The plants prop that PlantsList receives will be updated along with the re-renders.

So, to go over it again, here's what happens:

1. searchTerm is a state managed in PlantList.
2. searchedPlants is a filtered version of plants that will update on every re-render caused by setSearchTerm
3. searchTerm and setSearchTerm are passed to Search.js
4. searchedPlants is passed into PlantList. User input in Search updates the value of search, forcing a re-render and passing an up-to-date searchedPlants array.
5. Within Search.js, we establish that a text input field is a controlled component by setting its value to searchTerm and giving it on onChange event to setSearchTerm as e.target.value
6. Change events in the Search.js input invoke the setSearchTerm function which means we will re-render the page, affecting the displayed plants since the array that is passed down to PlantList is filtered by searchTerm.

Yuck.




