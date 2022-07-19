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

Our document has an element somewhere with the ID 'root.' Render(element) will display the specified HTML inside the target. The specified HTML is <Welcome name="Sara">. Welcome is a functional component.

The next thing to break down is the use of *props* here.

https://www.freecodecamp.org/news/how-to-use-props-in-react/#:~:text=What%20are%20props%20in%20React,your%20app%20to%20be%20dynamic.

https://dmitripavlutin.com/react-props/

Props is a shorter word than properties, but barely. I'm not a fan of this existing in the lexicon, but whatever. They are important for making reusable code, much in the way that parameters allow functions to serve general uses.

The more dynamic, the better! Right?

https://learning.flatironschool.com/courses/5207/assignments/194711?module_item_id=437855

A functional component is a function that takes props as an argument and returns JSX.

Components can describe a template of JSX rather than a bunch of hard-coded information. Component reuse is dependent on certain speficicities and structural details.

https://www.geeksforgeeks.org/reactjs-defaultprops/#:~:text=The%20defaultProps%20is%20a%20React,default%20props%20for%20the%20class.


Yikes, okay. None of this is sticking.

```javascript
function ExampleFunction(props) {
    return <div>{props.value}></div?>
}
```

The placement of props makes sense. However, props.value is something new. Presumably, the simplest examples will fail to properly demonstrate the scalability of the concept, but here goes:

With, uh, a parent component example since that's all I can manage:

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

Alright, in the above example, ParentComponent is calling ChildComponent. In the h3 tag. Calling a component is similar to writing an HTML tag, you use <these guys> but you <Capitalize /> the first letter. Referencing ChildComponent here is like using a varaible. You are telling React to replace ChildComponent with whatever ChildComponent returns.

```javascript
// childComponent.js

const ChildComponent = () => {
    return (
        <p>I'm the 1st child</p>
    )
}
```

In other words, our h3 will enclose a <p> tag with inner text "I'm the 1st child".

Neato, but where do props come in? I feel like everything feels backwards for one primary reason.

Your data is the most specific when you are assigning a property to a child component. The purpose of the parent component is to tell React what to do with the specific data. Backwards, right? 

It seems *a little* backwards.

So, how might we be more dynamic in the above example?

Let's think about the purpose of the child and parent components.

The child is there for us to dynamically pass specific information. The parent is there to instruct React on what to do with the information.

Assume that I wanted to be able to create <p> tags within <h3> elements, and provide each <p> tag with different text.

This is what I would do:

```javascript
// assuming you have ParentComponent and ChildComponent in the same folder
import React, { Component } from 'react'
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

Here's why the examples are so silly, though. This is BARELY more convenient than using vanilla JavaScript. I guess the point is that the components allow for reuse, but you could already do that by using parameters and arguments in an abstract fashion.

Anyway, if I see props passed into a component, I assume that the component is going to use dot notation somewhere.

And I would then expect to see the dot notation *specified* within a parent component. For some *dynamic* business.

I guess I get it. I think I need an example that really demonstrates the potential and flexibility afforded by using props in this way to properly grasp the concept. 

Okay, cool. So any time you need some information in a parent element to have dynamic potential, you can reference child components that use props.

You have to specify what information needs to change within the parent component (which seems to defeat the entire purpose) but I'm certain there is more to it than that.
















