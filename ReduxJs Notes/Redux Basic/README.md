# ReduxJs Basics

---

## What is ReduxJs?

Redux is a predictable state container for JavaScript apps.

It helps you write applications that behave consistently, run in different environments (client, server, and native), and are easy to test.

You can use Redux together with [React](https://reactjs.org/), or with any other view library. It is tiny (2kB, including dependencies), but has a large ecosystem of addons available.

I wrote it in the way redux/react developers would usually think of. Helps you to think like dev! 

## Big Picture

---

![Big Picture](https://github.com/huixiang01/javascript_coding_notes/blob/master/ReduxJs%20Notes/Redux_Basic_Big_Picture.gif?raw=true)

Well, as you can see, there are various component we are going to talk about

- Actions
- Reducers
- Store
- Data Flow
- Usage with React
- Example: Todo List

Let's get started!



## Installation

---

But wait! You almost forgot something. You need to install first.

### Prerequisite

> All that is required for Javascript and ReactJs. Refer to them in my repository! Note that you won't need react to work with Redux. Redux can be used on Angular, Ember, jQuery, or vanilla JavaScript.

On CLI

```
npm install --save redux
```



## Actions

---

> Actions are payloads of information that send data from your application to your store, which is database in that sense.
>
> You can send them through store.dispatch()

An example of an action:

```javascript
const ADD_TODO = 'ADD_TODO'
```

```javascript
{
  type: ADD_TODO,
  text: 'Build my first Redux SUTD app'
}
```

As you can see, actions are objects. All actions must have `type` property to indicate the type of action being performed.

When actions are being called, redux will use type on the object to check what action to perform.

Usually, we would move all the actions relating to one component into one module, or one JS file in another sense. The list will get so long!

```javascript
import { ADD_TODO, REMOVE_TODO } from '../actionTypes'
```

### Action Creators

> Yup! You can create action through a function

Simply a function

```javascript
function addTodo(text) {
  return {
    type: ADD_TODO,
    text
  }
}
```

You then trigger them:

```javascript
dispatch(addTodo(text))
```

You can also do this to trigger:

```javascript
const boundAddTodo = text => dispatch(addTodo(text))
const boundCompleteTodo = index => dispatch(completeTodo(index))

//Call them!
boundAddTodo(text)
boundCompleteTodo(index)
```

**NOTE**: you cannot trigger them while creating like in traditional flux

A bad example would be:

```javascript
function addTodoWithDispatch(text) {
  const action = {
    type: ADD_TODO,
    text
  }
  dispatch(action)
}
```

**Put it together**:

```javascript
 */

export const ADD_TODO = 'ADD_TODO'
export const TOGGLE_TODO = 'TOGGLE_TODO'
export const SET_VISIBILITY_FILTER = 'SET_VISIBILITY_FILTER'

/*
 * other constants
 */

export const VisibilityFilters = {
  SHOW_ALL: 'SHOW_ALL',
  SHOW_COMPLETED: 'SHOW_COMPLETED',
  SHOW_ACTIVE: 'SHOW_ACTIVE'
}

/*
 * action creators
 */

export function addTodo(text) {
  return { type: ADD_TODO, text }
}

export function toggleTodo(index) {
  return { type: TOGGLE_TODO, index }
}

export function setVisibilityFilter(filter) {
  return { type: SET_VISIBILITY_FILTER, filter }
}
```



## Reducers

---

Reducers specify how the applications sates react to the actions sent to the store. Remember that actions only describe what happened, but not describing the "how". "How" is what reducers do.

In Redux, all the application state is stored as a single object, which is the store! It's a good idea to think of its shape before writing any code. What's the minimal representation of your app's state as an object?

For our todo app, we want to store two different things:

- The currently selected visibility filter.
- The actual list of todos.

You'll often find that you need to store some data, as well as some UI state, in the state tree. This is fine, but try to keep the data separate from the UI state.

```javascript
{
  visibilityFilter: 'SHOW_ALL',
  todos: [
    {
      text: 'Consider using Redux',
      completed: true
    },
    {
      text: 'Keep all state in a single tree',
      completed: false
    }
  ]
}
```

**Handling Actions**

Now that we've decided what our state object looks like, we're ready to write a reducer for it. The reducer is a pure function that takes the previous state and an action, and returns the next state.

```javascript
(previousState, action) => newState
```

Things that you should not do inside a reducer:

- Mutate its arguments;
- Perform side effects like API calls and routing transitions;
- Call non-pure functions, e.g. `Date.now()` or `Math.random()`.

Why? Becuase it is **reduce**-r. It passes through [`Array.prototype.reduce(reducer, ?initialValue)`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce) function.(i.e. the function that you use in JavaScript to reduce an array). We don't want things to disrupt our reducer. Hence, it should stay pure.

If you wanna know more: Advanced Walkthrough

But for beginners, simply put: **Calculate the next state and return it. End of story.**

Okay. Now, with disclaimers seen, let's start writing.

Start by specifying the initial state. Redux will call our reducer with an `undefined` state for the first time. This is our chance to return the initial state of our app:

```javascript

import {
  ADD_TODO, // Importing action from action module
  TOGGLE_TODO,
  SET_VISIBILITY_FILTER,
  VisibilityFilters
} from './actions'

//this is an action ofr initial State
const initialState = {
  visibilityFilter: VisibilityFilters.SHOW_ALL,
  todos: []
}
... // More codes in the middle...
// This is a reducer
function todoApp(state = initialState, action) { 
    // Put in the default argument
  switch (action.type) {
    case SET_VISIBILITY_FILTER: // This is the action.type being called.
      return Object.assign({}, state, {
        visibilityFilter: action.filter // changes the action into action.filter
      });
    case ADD_TODO:
      return Object.assign({}, state, {
        todos: [
          ...state.todos, // Recap: Triple dots indicates rest remains unchanged
          {
            text: action.text,
            completed: false
          }
        ]
      });
    case TOGGLE_TODO:
      return Object.assign({}, state, {
        todos: state.todos.map((todo, index) => { //Be aware of Immutablility
          if (index === action.index) {
            return Object.assign({}, todo, {
              completed: !todo.completed
            })
          }
          return todo
        })
      })
    default: // There is a default if action not being called.
      return state //rmb to write this down!
  }
}

```

Note that:

1. **We don't mutate the state.** 

   We create a copy with [`Object.assign()`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Object/assign). `Object.assign(state, { visibilityFilter: action.filter })` is also wrong: it will mutate the first argument. 

   You **must** supply an empty object as the first parameter. You can also enable the [object spread operator proposal](https://redux.js.org/recipes/using-object-spread-operator) to write `{ ...state, ...newState }` instead.

2. **We return the previous state in the default case.** 

   It's important to return the previous `state` for any unknown action.

### Splitting Reducers

Functions in reducers are kind of annoying. Why not take them out? 

```javascript
function todos(state = [], action) { // It is an array now!
  switch (action.type) {
    case ADD_TODO:
      return [
        ...state,
        {
          text: action.text,
          completed: false
        }
      ]
    case TOGGLE_TODO:
      return state.map((todo, index) => {
        if (index === action.index) {
          return Object.assign({}, todo, {
            completed: !todo.completed
          })
        }
        return todo
      })
    default:
      return state
  }
}

function todoApp(state = initialState, action) {
  switch (action.type) {
    case SET_VISIBILITY_FILTER:
      return Object.assign({}, state, {
        visibilityFilter: action.filter
      })
    case ADD_TODO:
      return Object.assign({}, state, {
        todos: todos(state.todos, action)
      })
    case TOGGLE_TODO:
      return Object.assign({}, state, {
        todos: todos(state.todos, action)
      })
    default:
      return state
  }
}
```

Note that `todos` also accepts `state`—but `state` is an array! Now `todoApp` gives `todos` just a slice of the state to manage, and `todos` knows how to update just that slice. **This is called reducer composition, and it's the fundamental pattern of building Redux apps.**

Now, why not extract a reducer managing just visibilityFilter? Then after that combine the two reducers?

Final product:

```javascript
import { combineReducers } from 'redux' // import combineReducers to combine reducers
import {
  ADD_TODO,
  TOGGLE_TODO,
  SET_VISIBILITY_FILTER,
  VisibilityFilters
} from './actions'
const { SHOW_ALL } = VisibilityFilters

function visibilityFilter(state = SHOW_ALL, action) {
  switch (action.type) {
    case SET_VISIBILITY_FILTER:
      return action.filter
    default:
      return state
  }
}

function todos(state = [], action) { 
  switch (action.type) {
    case ADD_TODO:
      return [
        ...state,
        {
          text: action.text,
          completed: false
        }
      ]
    case TOGGLE_TODO:
      return state.map((todo, index) => {
        if (index === action.index) {
          return Object.assign({}, todo, {
            completed: !todo.completed
          })
        }
        return todo
      })
    default:
      return state
  }
}

const todoApp = combineReducers({
  visibilityFilter,
  todos
})

// combineReducers({}) is similar to :
// function todoApp(state = {}, action) {
  // return {
    // visibilityFilter: visibilityFilter(state.visibilityFilter, action),
    // todos: todos(state.todos, action)
  // }
//}

export default todoApp
```

This seems much neater now! Each of the reducers is managing their own part. If there are more components, you can categorise them into modules for respective components

  

## Store

---

Recap:

> Actions: what happened to the state
>
> Reducers: how to update the state

Well, now store brings them together. It has the following properties:

- Holds the application state
- Allows access to state via `getState() `
  - Very useful to check what you have in store
- Allows state to be updated via `dispatch(action)`
- Registers listeners via `subscribe(listener)`
- Handles unregistering of listeners via the function returned by `subscribe(listener)`

*Note*: You only one store in your Redux application. Everything you want to store will be combined by `combineReducers()` function

Where is store then? You need to create it. To create store, simply` createStore(App)`

```javascript
import { createStore } from 'redux'
import todoApp from './reducers' // import all reducers combined
const store = createStore(todoApp)
```

Optional: you can put your initial state on store

```javascript
const store = createStore(todoApp, window.STATE_FROM_SERVER)
```

Okay. we need a pitstop. Npw, check whether it works!

```javascript
import {
  addTodo,
  toggleTodo,
  setVisibilityFilter,
  VisibilityFilters
} from './actions'

// Log the initial state
console.log(store.getState())

// Every time the state changes, log it
// Note that subscribe() returns a function for unregistering the listener
const unsubscribe = store.subscribe(() => console.log(store.getState()))

// Dispatch some actions
store.dispatch(addTodo('Learn about actions'))
store.dispatch(addTodo('Learn about reducers'))
store.dispatch(addTodo('Learn about store'))
store.dispatch(toggleTodo(0))
store.dispatch(toggleTodo(1))
store.dispatch(setVisibilityFilter(VisibilityFilters.SHOW_COMPLETED))

// Stop listening to state updates
unsubscribe()
```

![Store Output](https://github.com/huixiang01/javascript_coding_notes/blob/master/ReduxJs%20Notes/Store%20Output.png?raw=true)

As you can see, store is a very big object. In every dispatch(actions), store is being updated.



## Data Flow

---

Redux architecture revolves around a **strict unidirectional data flow**.

This means that all data in an application follows the same lifecycle pattern, making the logic of you app more predictable and easier to understand. It also encourages data normalization, so that you don't end up with multiple, independent copies of the same data that are unaware of one another.

If you're still not convinced, read [Motivation](https://redux.js.org/introduction/motivation) and [The Case for Flux](https://medium.com/@dan_abramov/the-case-for-flux-379b7d1982c6) for a compelling argument in favor of unidirectional data flow. Although [Redux is not exactly Flux](https://redux.js.org/introduction/prior-art), it shares the same key benefits.

The data lifecycle in any Redux app follows there 4 steps(Refer to Big Picture for reference):

	1. **You call store.dispatch(action)**

An action is a plain object describing *what happened*. For example:

```js
 { type: 'LIKE_ARTICLE', articleId: 42 }
 { type: 'FETCH_USER_SUCCESS', response: { id: 3, name: 'Mary' } }
 { type: 'ADD_TODO', text: 'Read the Redux docs.' }
```

Think of an action as a very brief snippet of news. 

“Mary liked article 42.” or “'Read the Redux docs.' was added to the list of todos.”

You can call `store.dispatch(action)` from anywhere in your app, including components and XHR callbacks, or even at scheduled intervals.

2. **The Redux store calls the reducer function you gave it**

The store will pass two arguments to the reducer: the current state tree and the action. For example, in the todo app, the root reducer might receive something like this:

```javascript
// The current application state (list of todos and chosen filter)
let previousState = {
  visibleTodoFilter: 'SHOW_ALL',
  todos: [
    {
      text: 'Read the docs.',
      complete: false
    }
  ]
}

// The action being performed (adding a todo)
let action = {
  type: 'ADD_TODO',
  text: 'Understand the flow.'
}

// Your reducer returns the next application state
let nextState = todoApp(previousState, action)
```

Note that a reducer is a **pure function**. It only *computes* the next state. It should be completely predictable: calling it with the same inputs many times should produce the same outputs. It shouldn't perform any side effects like API calls or router transitions. These should happen before an action is dispatched.

3. **The root reducer may combine the output of multiple reducers into a single state tree**

How you structure the root reducer is completely up to you. Redux ships with a `combineReducers()` helper function, useful for “splitting” the root reducer into separate functions that each manage one branch of the state tree.

Here's how `combineReducers()` works. You have two reducers, one for a list of todos, and another for the currently selected filter setting:

```javascript
function todos(state = [], action) {
  // Somehow calculate it...
  return nextState
}

function visibleTodoFilter(state = 'SHOW_ALL', action) {
  // Somehow calculate it...
  return nextState
}

let todoApp = combineReducers({
  todos,
  visibleTodoFilter
})
```

It will combine both sets of results into a single state tree.

​	4.**The Redux store saves the complete state tree returned by the root reducer**

This new tree is now the next state of your app! Every listener registered with `store.subscribe(listener)`]will now be invoked; listeners may call `store.getState()` to get the current state.

Now, the UI can be updated to reflect the new state. If you use bindings like [React Redux](https://github.com/gaearon/react-redux), this is the point at which `component.setState(newState)` is called.



## Usage with React

---

From the very beginning, we need to stress that Redux has no relation to React. You can write Redux apps with React, Angular, Ember, jQuery, or vanilla JavaScript. 

But it works very well with [React](http://facebook.github.io/react/) and [Deku](https://github.com/dekujs/deku) because they let you describe UI as a function of state, and Redux emits state updates in response to actions.

We will use React to build our simple todo app, and cover the basics of how to use React with Redux.

### Installing React Redux

[React bindings](https://github.com/reduxjs/react-redux) are not included in Redux by default. You need to install them explicitly:

```sh
npm install --save react-redux
```

## Presentational and Container Components

React bindings for Redux separate *presentational* components from *container* components. This approach can make your app easier to understand and allow you to more easily reuse components. Here's a summary of the differences between presentational and container components (but if you're unfamiliar, we recommend that you also read [Dan Abramov's original article describing the concept of presentational and container components](https://medium.com/@dan_abramov/smart-and-dumb-components-7ca2f9a7c7d0)):

|                | Presentational Components        | Container Components                           |
| :------------- | :------------------------------- | :--------------------------------------------- |
| Purpose        | How things look (markup, styles) | How things work (data fetching, state updates) |
| Aware of Redux | No                               | Yes                                            |
| To read data   | Read data from props             | Subscribe to Redux state                       |
| To change data | Invoke callbacks from props      | Dispatch Redux actions                         |
| Are written    | By hand                          | Usually generated by React Redux               |

Most of the components we'll write will be presentational, but we'll need to generate a few container components to connect them to the Redux store. This and the design brief below do not imply container components must be near the top of the component tree. If a container component becomes too complex (i.e. it has heavily nested presentational components with countless callbacks being passed down), introduce another container within the component tree as noted in the [FAQ](https://redux.js.org/faq/react-redux#should-i-only-connect-my-top-component-or-can-i-connect-multiple-components-in-my-tree).

Technically you could write the container components by hand using [`store.subscribe()`](https://redux.js.org/api/store#subscribelistener). We don't advise you to do this because React Redux makes many performance optimizations that are hard to do by hand. For this reason, rather than write container components, we will generate them using the [`connect()`](https://react-redux.js.org/api/connect#connect) function provided by React Redux.

## Example: Todo List

---

Todo List Example: https://codesandbox.io/s/github/reduxjs/redux/tree/master/examples/todos

