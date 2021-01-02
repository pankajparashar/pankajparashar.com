---
title: Building a Todo app with React.js
date: 2015-03-06 00:00:00 Z
layout: post
excerpt: This is my first attempt with React.js to build a basic Todo app. React has
  gained massive traction as a JavaScript library for building user interfaces largely
  because it is built by Facebook and their engineers have challenged the age-old
  best practice for separation of concerns
---

> This article has now been updated to use the modern version of React as of 2020. The new version uses [React Hooks](https://reactjs.org/docs/hooks-intro.html) and the [React Context API](https://reactjs.org/docs/context.html) to build the Todo app.

In this article, we'll build an extremely simple app using Facebook's [react.js](http://facebook.github.io/react/). If you are unfamiliar with this library, then I would strongly recommend reading a [basic tutorial](http://facebook.github.io/react/docs/tutorial.html) using React.

## Thinking in components

The fundamental way of building a React.js app is to break down your app into bunch of useful components and then work your way backwards to build them separately. Once the individual components are ready, we can wire them up to exchange data between the components. For instance, our Todo app can be decomposed into the following components and hierarchies,

```
TODO APP
  TODO HEADER
  TODO CONTAINER
    TODO LIST ITEM #1
    TODO LIST ITEM #2
    ...
    TODO LIST ITEM #N => TODO FORM
```

## Wiring dependencies

React ofcourse needs the `react.js` library and the JSX Transformer for sugar syntax. Before, we proceed we'll add these dependencies to our document.

```
<script src="https://unpkg.com/react@16/umd/react.development.js" crossorigin></script>
<script src="https://unpkg.com/react-dom@16/umd/react-dom.development.js" crossorigin></script>
```

## Basic Skeleton

In React hooks, components are written using Functions. We need two child components `TodoHeader` and `TodoContainer`, which will be composed in the parent component `TodoApp` and mount this component in the document node.

```
function TodoHeader(){}
function TodoContainer(){}

function TodoApp(){
  return (
    <div>
      <TodoHeader/>
      <TodoContainer/>
    </div>
  );
}

React.render(<TodoApp/>, document.body);
```

## Setting up React Context

We'll create an empty context and use this context inside the components, pass the value using `<Context>.Provider` to its child components.

```
const TodoContext = React.createContext();

function TodoApp(){
  const todoList = React.useState([
    {content: 'Todo item #1', isCompleted: true},
    {content: 'Todo item #2', isCompleted: false},		
  ]);	

  return (
    <TodoContext.Provider value={todoList}>
      <TodoHeader/>
      <TodoContainer/>
    </TodoContext.Provider>
  );
}
```

## Setting up React Consumer

We can read the value passed from the parent component using `React.useContext`. Since, the value passed is a React hook, we can destructure the array in the same line. Additionally, our `TodoContainer` also defines its own utility methods to add, remove or update the Todo item in the Todo list. 

```
function TodoContainer(){
  const [todos, setTodos] = React.useContext(TodoContext);
  const [value, setValue] = React.useState('');

  const addTodoItem = (e) => {}
  const removeTodoItem = (i) => {}
  const updateTodoItem = (e, i) => {}

  return (
    <form>
      <ul>
        {todos.map((todo, i) => (
          <li key={i}>
            <input type="checkbox" 
                   checked={todo.isCompleted} 
                   onClick={e => updateTodoItem(e, i)}/>
            <input type="text" 
                   value={todo.content} 
                   disabled={todo.isCompleted} 
                   onChange={e => updateTodoItem(e, i)}/>
            <button onClick={() => removeTodoItem(i)}>-</button>
          </li>
        ))}
        <li>
          <input type="checkbox" disabled/>
          <input type="text" 
                 placeholder="New todo + press enter" 
                 onKeyDown={e => addTodoItem(e)} 
                 onChange={(e) => {setValue(e.target.value)}}/>
          <button type="submit">+</button>
        </li>
      </ul>
    </form>
  );
}
```

## Add Todo Item

Our todo app allows user to add a new todo into the list either by pressing the enter button on the keyboard or by clicking the `+` button in the form. Additionally, we need to reset the form on each successful addition of todo. We should also do some validation to ensure empty todos are not added into the list.

```
const addTodoItem = (e) => {
  if((e.type=="submit" || e.key=="Enter")){
    e.preventDefault();	
    if(value!=""){
      setTodos([...todos, {content:value, isCompleted:false}]);
      setValue("");
    }
    if(e.key=="Enter"){ e.target.value=""; }
    if(e.type=="submit"){ e.target.reset(); }			
  }
};
```

## Remove Todo Item

When the user clicks on the `-` button, we pass the index of the todo item to the event handler and then remove the element from the todo array.

```
const removeTodoItem = (i) => {
  const newTodos = [...todos];
  newTodos.splice(i,1);
  setTodos(newTodos);
};
```

## Update Todo item

As long as the todo item is not marked as completed, we can allow the user to simply update the text for the todo item. If the user clicks on the checkbox, we toggle the status of `isCompleted` accordingly.

```
const updateTodoItem = (e, i) => {
  const newTodos = [...todos];
  if(e.target.type == 'text'){
    newTodos[i].content = e.target.value;
  }
  else if(e.target.type == 'checkbox'){
    newTodos[i].isCompleted = !newTodos[i].isCompleted;
  }
  setTodos(newTodos);		
};
```

The complete working demo can be found [here](https://codepen.io/pankajparashar/full/abdKReN) on Codepen. Ofcourse there's lot to improve but the code is modular enough to handle any kind of enhancement.