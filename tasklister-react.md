Destructure tasks 100 times over the weekend.

```javascript
import React, { useState } from 'react';
import './App.css';
import Banner from './components/Banner'
import TaskForm from './components/TaskForm';
import TaskList from './components/TaskList'

const App = () => {
  const [tasks, setTasks] = useState([])

  const handleSubmit = (text) => {
    if (text) {
      setTasks([...tasks, text])
      console.log('Form submitted')
    }
  }

  return (
    <div className="App">
      <Banner bannerText={'Task Lister React™'}/>
      <TaskForm handleSubmit={handleSubmit}/>
      <TaskList tasks={tasks}/>
    </div>
  );
}

export default App;
```
```javascript
// Banner.js
const Banner = ({bannerText}) => {
    return (
        <h1>
            {bannerText}
        </h1>
    )
}

export default Banner
```
```javascript
// Taskform.js
import { useState } from 'react'

const TaskForm = ({handleSubmit}) => {
    const [text, setText] = useState('')

    const handleChange = (e) => {
        setText(e.target.value)
    }

    return (
        <div className='Task-Form'>
            <form onSubmit={(e) => {
                e.preventDefault()
                handleSubmit(text)
                e.target.reset()
                setText('')
            }}>
                <label>Task Description: </label>
                <input type='text' onChange={(e) => {
                    handleChange(e)
                }} />
                <input type='submit' value='Create New Task' />
            </form>
        </div>
    )
}

export default TaskForm
```
```javascript
// TaskList.js
import React from "react";
import Task from "./Task";

const TaskList = ({ tasks }) => {
    return (
        <div>
            <h3>My To-dos</h3>
            <ul>
                {tasks.map((element, index) => {
                    return (
                        <Task key={index} text={element} />
                    )
                })}
            </ul>
        </div>
    )
}

export default TaskList
```
```javascript
// Task.js
import React from "react";

const Task = ( { text }) => {
    return (
        <li>{text}</li>
    )
}

export default Task
```

