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

### What are Props, and Why Should I Care

I have way too many links shared here, because I took way too long to wrap my head around the concept of props.

https://www.freecodecamp.org/news/how-to-use-props-in-react/#:~:text=What%20are%20props%20in%20React,your%20app%20to%20be%20dynamic.

https://dmitripavlutin.com/react-props/

Props stands for properties--it's a shorter word but barely. Kind of hate it, but whatever.. They are important for making reusable code, much in the way that parameters allow functions to serve general uses.

The more dynamic, the better! Right?

https://learning.flatironschool.com/courses/5207/assignments/194711?module_item_id=437855

A functional component is a can takes props as an argument use those props to dynamically affect the JSX that is returned.

In order for a components to serve as a template of JSX rather than a bunch of hard-coded information, you must use props.

https://www.geeksforgeeks.org/reactjs-defaultprops/#:~:text=The%20defaultProps%20is%20a%20React,default%20props%20for%20the%20class.

Here's a tiny example.

```javascript
function ExampleFunction(props) {
    return <div>{props.value}></div?>
}
```

The placement of props makes sense. However, `props.value` is something we haven't explicitly seen before. Presumably, the simplest examples will fail to properly demonstrate the scalability of the concept, but here goes:

```javascript
class ParentComponent extends Component {
    render() {
        return (
            <ChildComponent name="First Child" />
        )
    }
}

const ChildComponent = (props) => {
    return <p>{props.name}</p>
}
```

```javascript
// assuming you have ParentComponent and ChildComponent in the same folder
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

Alright, in the above example, ParentComponent is calling ChildComponent. In the h3 tag. Calling a component is similar to writing an HTML tag, you use `<these guys>` but you `<Capitalize />` the first letter. Referencing ChildComponent here is like using a varaible. You are telling React to replace ChildComponent with whatever ChildComponent returns.

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

The child is there for us to dynamically pass specific information. The parent is there to instruct React on what to do with the information.

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

I only like, get 60% of what is going on here. Or, it's that I understand, but that there is a sizable disconnect.

## WHEN TO USE PROPS

If you're out here trying to *dynamically* render things then maybe you need to use some *props*.

Okay, cool. So any time you need some information in a parent element to have dynamic potential, you can reference child components that use props.

You have to specify what information needs to change within the parent component (which seems to defeat the entire purpose) but I'm certain there is more to it than that.

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

Sensible. More so than it was to begin with.

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
```
- Create a function component called App (using arrow function syntax) 3 times!
- App should return a div with an h1 that says "Hello React" inside
- Be sure to export the component at the bottom
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

```
Write a component called NewsFeed that returns a div that has 2 child components called Post. Be sure to export and import your files properly.
Write a component called Post that takes accepts 2 props called "likes" (a number) and "image_url" (a string). The Post component should return a div that has an img tag and a paragraph tag that implements the given props in a manner that makes sense.
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

This isn't exactly as simple as the first part, because sibling components cannot directly pass data to each other. The workaround for this is **storing the color of the `Child` in the state of the `Parent` component. The `Parent` component will handle the passing of data to the children.

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
      onClick={onChangeColor}
      className='child'
      style={{ backgroundColor: color }}
      />
  )
}

export default Child;
```