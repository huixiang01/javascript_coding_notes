# ReduxJs Basics

---

## What is ReduxJs?

Redux is a predictable state container for JavaScript apps.

It helps you write applications that behave consistently, run in different environments (client, server, and native), and are easy to test.

You can use Redux together with [React](https://reactjs.org/), or with any other view library. It is tiny (2kB, including dependencies), but has a large ecosystem of addons available.

## Big Picture

---

![Big Picture](https://github.com/huixiang01/javascript_coding_notes/blob/master/ReduxJs%20Notes/Big%20Picture.png?raw=true)

Well, as you can see, there are various component we are going to talk about

- Actions
- Reducers
- Store
- Data Flow
- Usage with React
- Example: Todo List

Let's get started!

## Actions

>  Actions are payloads of information that send data from your application to your store, which is database in that sense.
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



