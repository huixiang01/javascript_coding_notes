# Redux Advanced

---

Okay! Now you have foundations on Redux in your hands! If you have not read my Redux Basics Notes, please do! It is gonna be confusing if you don't.

Alright. Here, we are gonna talk about:

- [Async Actions](https://redux.js.org/advanced/async-actions)
- [Async Flow](https://redux.js.org/advanced/async-flow)
- [Middleware](https://redux.js.org/advanced/middleware)
- [Usage with React Router](https://redux.js.org/advanced/usage-with-react-router)
- [Example: Reddit API](https://redux.js.org/advanced/example-reddit-api)
- [Next Steps](https://redux.js.org/advanced/next-steps)



## Async Actions

---

In Redux notes, everything is synchronous. Every time the action was dispatched, the state was updated immediately.

In this guide, we will build a different, asynchronous application. It will use the Reddit API to show the current headlines for a selected subreddit. How does asynchronicity fit into Redux flow?

### Actions

There is two crucial moments for asynchronous API:

- The moment you start calling the API
- The moment you receive an answer(i.e. timeout)

Both usually require a large change in the application state; to do that, you need to dispatch normal actions that will be processed by reducers synchronously. Usually, for any API request you'll want to dispatch at least different kinds of actions:

- An action informing the reducers that the **request began**
  - The reducers may handle this action by toggling in `isFetching` flag in the state. This way the UI knows it's time to show a spinner.
- An action informing the reducers that the request **finished successfully**.
  - The reducers may handle this action by merging the new data into the state they manage and resetting `isFetching`. The UI would hide the spinner, and display the fetched data
- An action informing the reducers that the **request failed**.
  - The reducers may handle this action by resetting `isFetching`. Additionally, some reducers may want to store the error message so the UI can display it.

You may use a dedicated `status` field in your actions:

```javascript
{ type: 'FETCH_POSTS' }
{ type: 'FETCH_POSTS', status: 'error', error: 'Oops' }
{ type: 'FETCH_POSTS', status: 'success', response: { ... } }
```

Or you can define separate types for them(recommended):

```js
{ type: 'FETCH_POSTS_REQUEST' }
{ type: 'FETCH_POSTS_FAILURE', error: 'Oops' }
{ type: 'FETCH_POSTS_SUCCESS', response: { ... } }
```

Choosing whether to use a single action type with flags, or multiple action types, is up to you. It's a convention you need to decide with your team. Multiple types leave less room for a mistake, but this is not an issue if you generate action creators and reducers with a helper library like [redux-actions](https://redux-actions.js.org/).

Whatever convention you choose, stick with it throughout the application.

### Synchronous Action Creators

Let's start by defining the several synchronous action types and action creators we need in our example app. Here, the user can select a subreddit to display:

**`actions.js` (Synchronous)**

```js
export const SELECT_SUBREDDIT = 'SELECT_SUBREDDIT'

export function selectSubreddit(subreddit) {
  return {
    type: SELECT_SUBREDDIT,
    subreddit
  }
}
```

They can also press a “refresh” button to update it:

```js
export const INVALIDATE_SUBREDDIT = 'INVALIDATE_SUBREDDIT'

export function invalidateSubreddit(subreddit) {
  return {
    type: INVALIDATE_SUBREDDIT,
    subreddit
  }
}
```

These were the actions governed by the user interaction. We will also have another kind of action, governed by the network requests. We will see how to dispatch them later, but for now, we just want to define them.

When it's time to fetch the posts for some subreddit, we will dispatch a `REQUEST_POSTS` action:

```js
export const REQUEST_POSTS = 'REQUEST_POSTS'

function requestPosts(subreddit) {
  return {
    type: REQUEST_POSTS,
    subreddit
  }
}
```

It is important for it to be separate from `SELECT_SUBREDDIT` or `INVALIDATE_SUBREDDIT`. While they may occur one after another, as the app grows more complex, you might want to fetch some data independently of the user action (for example, to prefetch the most popular subreddits, or to refresh stale data once in a while). You may also want to fetch in response to a route change, so it's not wise to couple fetching to some particular UI event early on.

Finally, when the network request comes through, we will dispatch `RECEIVE_POSTS`:

```js
export const RECEIVE_POSTS = 'RECEIVE_POSTS'

function receivePosts(subreddit, json) {
  return {
    type: RECEIVE_POSTS,
    subreddit,
    posts: json.data.children.map(child => child.data),
    receivedAt: Date.now()
  }
}

```

This is all we need to know for now. The particular mechanism to dispatch these actions together with network requests will be discussed later.

## Designing the State Shape

Just like in the basic tutorial, you'll need to design the shape of your application's state before rushing into the implementation. With asynchronous code, there is more state to take care of, so we need to think it through.

This part is often confusing to beginners, because it is not immediately clear what information describes the state of an asynchronous application, and how to organize it in a single tree.

We'll start with the most common use case: **lists**. Web applications often show lists of things. For example, a list of posts, or a list of friends. You'll need to figure out what sorts of lists your app can show. You want to store them separately in the state, because this way you can cache them and only fetch again if necessary.

Here's what the state shape for our “Reddit headlines” app might look like:

```js
{
  selectedSubreddit: 'frontend',
  postsBySubreddit: {
    frontend: {
      isFetching: true,
      didInvalidate: false,
      items: []
    },
    reactjs: {
      isFetching: false,
      didInvalidate: false,
      lastUpdated: 1439478405547,
      items: [
        {
          id: 42,
          title: 'Confusion about Flux and Relay'
        },
        {
          id: 500,
          title: 'Creating a Simple Application Using React JS and Flux Architecture'
        }
      ]
    }
  }
}

```

There are a few important bits here:

- We store each subreddit's information separately so we can cache every subreddit. When the user switches between them the second time, the update will be instant, and we won't need to re-fetch unless we want to. Don't worry about all these items being in memory: unless you're dealing with tens of thousands of items, and your user rarely closes the tab, you won't need any sort of cleanup.
- For every list of items, you'll want to store `isFetching` to show a spinner, `didInvalidate` so you can later toggle it when the data is stale, `lastUpdated` so you know when it was fetched the last time, and the `items` themselves. In a real app, you'll also want to store pagination state like `fetchedPageCount` and `nextPageUrl`.

## Handling Actions

Before going into the details of dispatching actions together with network requests, we will write the reducers for the actions we defined above. 

```javascript
import { combineReducers } from 'redux'
import {
  SELECT_SUBREDDIT,
  INVALIDATE_SUBREDDIT,
  REQUEST_POSTS,
  RECEIVE_POSTS
} from '../actions'

function selectedSubreddit(state = 'reactjs', action) {
  switch (action.type) {
    case SELECT_SUBREDDIT:
      return action.subreddit
    default:
      return state
  }
}

function posts(
  state = {
    isFetching: false,
    didInvalidate: false,
    items: []
  },
  action
) {
  switch (action.type) {
    case INVALIDATE_SUBREDDIT:
      return Object.assign({}, state, {
        didInvalidate: true
      })
    case REQUEST_POSTS:
      return Object.assign({}, state, {
        isFetching: true,
        didInvalidate: false
      })
    case RECEIVE_POSTS:
      return Object.assign({}, state, {
        isFetching: false,
        didInvalidate: false,
        items: action.posts,
        lastUpdated: action.receivedAt
      })
    default:
      return state
  }
}

function postsBySubreddit(state = {}, action) {
  switch (action.type) {
    case INVALIDATE_SUBREDDIT:
    case RECEIVE_POSTS:
    case REQUEST_POSTS:
      return Object.assign({}, state, {
        [action.subreddit]: posts(state[action.subreddit], action)
      })
    default:
      return state
  }
}

const rootReducer = combineReducers({
  postsBySubreddit,
  selectedSubreddit
})

export default rootReducer
```

In this code, there are two interesting parts:

- We use ES6 computed property syntax so we can update `state[action.subreddit]` with `Object.assign()` in a concise way. This:

  ```js
  return Object.assign({}, state, {
    [action.subreddit]: posts(state[action.subreddit], action)
  })
  ```
  
is equivalent to this:
  
```js
  let nextState = {}
  nextState[action.subreddit] = posts(state[action.subreddit], action)
  return Object.assign({}, state, nextState)
  ```
  
- We extracted `posts(state, action)` that manages the state of a specific post list. This is just [reducer composition](https://redux.js.org/basics/reducers#splitting-reducers)! It is our choice how to split the reducer into smaller reducers, and in this case, we're delegating updating items inside an object to a `posts` reducer. The [real world example](https://redux.js.org/introduction/examples#real-world) goes even further, showing how to create a reducer factory for parameterized pagination reducers.

Remember that reducers are just functions, so you can use functional composition and higher-order functions as much as you feel comfortable.

## Async Action Creators

Finally, how do we use the synchronous action creators we [defined earlier](https://redux.js.org/advanced/async-actions#synchronous-action-creators) together with network requests? The standard way to do it with Redux is to use the [Redux Thunk middleware](https://github.com/gaearon/redux-thunk). It comes in a separate package called `redux-thunk`. We'll explain how middleware works in general [later](https://redux.js.org/advanced/middleware); for now, there is just one important thing you need to know: by using this specific middleware, an action creator can return a function instead of an action object. This way, the action creator becomes a [thunk](https://en.wikipedia.org/wiki/Thunk).

When an action creator returns a function, that function will get executed by the Redux Thunk middleware. This function doesn't need to be pure; it is thus allowed to have side effects, including executing asynchronous API calls. The function can also dispatch actions—like those synchronous actions we defined earlier.

We can still define these special thunk action creators inside our `actions.js` file:

#### `actions.js` (Asynchronous)

```js
import fetch from 'cross-fetch'

export const REQUEST_POSTS = 'REQUEST_POSTS'
function requestPosts(subreddit) {
  return {
    type: REQUEST_POSTS,
    subreddit
  }
}

export const RECEIVE_POSTS = 'RECEIVE_POSTS'
function receivePosts(subreddit, json) {
  return {
    type: RECEIVE_POSTS,
    subreddit,
    posts: json.data.children.map(child => child.data),
    receivedAt: Date.now()
  }
}

export const INVALIDATE_SUBREDDIT = 'INVALIDATE_SUBREDDIT'
export function invalidateSubreddit(subreddit) {
  return {
    type: INVALIDATE_SUBREDDIT,
    subreddit
  }
}

// Meet our first thunk action creator!
// Though its insides are different, you would use it just like any other action creator:
// store.dispatch(fetchPosts('reactjs'))

export function fetchPosts(subreddit) {
  // Thunk middleware knows how to handle functions.
  // It passes the dispatch method as an argument to the function,
  // thus making it able to dispatch actions itself.

  return function(dispatch) {
    // First dispatch: the app state is updated to inform
    // that the API call is starting.

    dispatch(requestPosts(subreddit))

    // The function called by the thunk middleware can return a value,
    // that is passed on as the return value of the dispatch method.

    // In this case, we return a promise to wait for.
    // This is not required by thunk middleware, but it is convenient for us.

    return fetch(`https://www.reddit.com/r/${subreddit}.json`)
      .then(
        response => response.json(),
        // Do not use catch, because that will also catch
        // any errors in the dispatch and resulting render,
        // causing a loop of 'Unexpected batch number' errors.
        // https://github.com/facebook/react/issues/6895
        error => console.log('An error occurred.', error)
      )
      .then(json =>
        // We can dispatch many times!
        // Here, we update the app state with the results of the API call.

        dispatch(receivePosts(subreddit, json))
      )
  }
}
```

How do we include the Redux Thunk middleware in the dispatch mechanism? We use the [`applyMiddleware()`](https://redux.js.org/api/applymiddleware) store enhancer from Redux, as shown below:

#### `index.js`

```js
import thunkMiddleware from 'redux-thunk'
import { createLogger } from 'redux-logger'
import { createStore, applyMiddleware } from 'redux'
import { selectSubreddit, fetchPosts } from './actions'
import rootReducer from './reducers'

const loggerMiddleware = createLogger()

const store = createStore(
  rootReducer,
  applyMiddleware(
    thunkMiddleware, // lets us dispatch() functions
    loggerMiddleware // neat middleware that logs actions
  )
)

store.dispatch(selectSubreddit('reactjs'))
store.dispatch(fetchPosts('reactjs')).then(() => console.log(store.getState()))

```

The nice thing about thunks is that they can dispatch results of each other:

#### `actions.js` (with `fetch`)

```js
import fetch from 'cross-fetch'

export const REQUEST_POSTS = 'REQUEST_POSTS'
function requestPosts(subreddit) {
  return {
    type: REQUEST_POSTS,
    subreddit
  }
}

export const RECEIVE_POSTS = 'RECEIVE_POSTS'
function receivePosts(subreddit, json) {
  return {
    type: RECEIVE_POSTS,
    subreddit,
    posts: json.data.children.map(child => child.data),
    receivedAt: Date.now()
  }
}

export const INVALIDATE_SUBREDDIT = 'INVALIDATE_SUBREDDIT'
export function invalidateSubreddit(subreddit) {
  return {
    type: INVALIDATE_SUBREDDIT,
    subreddit
  }
}

function fetchPosts(subreddit) {
  return dispatch => {
    dispatch(requestPosts(subreddit))
    return fetch(`https://www.reddit.com/r/${subreddit}.json`)
      .then(response => response.json())
      .then(json => dispatch(receivePosts(subreddit, json)))
  }
}

function shouldFetchPosts(state, subreddit) {
  const posts = state.postsBySubreddit[subreddit]
  if (!posts) {
    return true
  } else if (posts.isFetching) {
    return false
  } else {
    return posts.didInvalidate
  }
}

export function fetchPostsIfNeeded(subreddit) {
  // Note that the function also receives getState()
  // which lets you choose what to dispatch next.

  // This is useful for avoiding a network request if
  // a cached value is already available.

  return (dispatch, getState) => {
    if (shouldFetchPosts(getState(), subreddit)) {
      // Dispatch a thunk from thunk!
      return dispatch(fetchPosts(subreddit))
    } else {
      // Let the calling code know there's nothing to wait for.
      return Promise.resolve()
    }
  }
}
```

This lets us write more sophisticated async control flow gradually, while the consuming code can stay pretty much the same:

#### `index.js`

```js
store
  .dispatch(fetchPostsIfNeeded('reactjs'))
  .then(() => console.log(store.getState()))
```

[Thunk middleware](https://github.com/gaearon/redux-thunk) isn't the only way to orchestrate asynchronous actions in Redux:

- You can use [redux-promise](https://github.com/acdlite/redux-promise) or [redux-promise-middleware](https://github.com/pburtchaell/redux-promise-middleware) to dispatch Promises instead of functions.
- You can use [redux-observable](https://github.com/redux-observable/redux-observable) to dispatch Observables.
- You can use the [redux-saga](https://github.com/yelouafi/redux-saga/) middleware to build more complex asynchronous actions.
- You can use the [redux-pack](https://github.com/lelandrichardson/redux-pack) middleware to dispatch promise-based asynchronous actions.
- You can even write a custom middleware to describe calls to your API, like the [real world example](https://redux.js.org/introduction/examples#real-world) does.

It is up to you to try a few options, choose a convention you like, and follow it, whether with, or without the middleware.

## Connecting to UI

Dispatching async actions is no different from dispatching synchronous actions, so we won't discuss this in detail. See [Usage with React](https://redux.js.org/basics/usage-with-react) for an introduction into using Redux from React components. See [Example: Reddit API](https://redux.js.org/advanced/example-reddit-api) for the complete source code discussed in this example.

# Async Flow

---

Without [middleware](https://redux.js.org/advanced/middleware), Redux store only supports [synchronous data flow](https://redux.js.org/basics/data-flow). This is what you get by default with [`createStore()`](https://redux.js.org/api/createstore).

You may enhance [`createStore()`](https://redux.js.org/api/createstore) with [`applyMiddleware()`](https://redux.js.org/api/applymiddleware). It is not required, but it lets you [express asynchronous actions in a convenient way](https://redux.js.org/advanced/async-actions).

Asynchronous middleware like [redux-thunk](https://github.com/gaearon/redux-thunk) or [redux-promise](https://github.com/acdlite/redux-promise) wraps the store's [`dispatch()`](https://redux.js.org/api/store#dispatchaction) method and allows you to dispatch something other than actions, for example, functions or Promises. Any middleware you use can then intercept anything you dispatch, and in turn, can pass actions to the next middleware in the chain. For example, a Promise middleware can intercept Promises and dispatch a pair of begin/end actions asynchronously in response to each Promise.

When the last middleware in the chain dispatches an action, it has to be a plain object. This is when the [synchronous Redux data flow](https://redux.js.org/basics/data-flow) takes place.



# Middleware

---

You've seen middleware in action in the [Async Actions](https://redux.js.org/advanced/async-actions) example. If you've used server-side libraries like [Express](http://expressjs.com/) and [Koa](http://koajs.com/), you were also probably already familiar with the concept of *middleware*. In these frameworks, middleware is some code you can put between the framework receiving a request, and the framework generating a response. For example, Express or Koa middleware may add CORS headers, logging, compression, and more. The best feature of middleware is that it's composable in a chain. You can use multiple independent third-party middleware in a single project.

Redux middleware solves different problems than Express or Koa middleware, but in a conceptually similar way. **It provides a third-party extension point between dispatching an action, and the moment it reaches the reducer.** People use Redux middleware for logging, crash reporting, talking to an asynchronous API, routing, and more.

This article is divided into an in-depth intro to help you grok the concept, and [a few practical examples](https://redux.js.org/advanced/middleware#seven-examples) to show the power of middleware at the very end. You may find it helpful to switch back and forth between them, as you flip between feeling bored and inspired.

## Understanding Middleware

While middleware can be used for a variety of things, including asynchronous API calls, it's really important that you understand where it comes from. We'll guide you through the thought process leading to middleware, by using logging and crash reporting as examples.

### Problem: Logging

One of the benefits of Redux is that it makes state changes predictable and transparent. Every time an action is dispatched, the new state is computed and saved. The state cannot change by itself, it can only change as a consequence of a specific action.

Wouldn't it be nice if we logged every action that happens in the app, together with the state computed after it? When something goes wrong, we can look back at our log, and figure out which action corrupted the state.

![img](https://i.imgur.com/BjGBlES.png)

How do we approach this with Redux?

### Attempt #1: Logging Manually

The most naïve solution is just to log the action and the next state yourself every time you call [`store.dispatch(action)`](https://redux.js.org/api/store#dispatchaction). It's not really a solution, but just a first step towards understanding the problem.

> ##### Note
>
> If you're using [react-redux](https://github.com/reduxjs/react-redux) or similar bindings, you likely won't have direct access to the store instance in your components. For the next few paragraphs, just assume you pass the store down explicitly.

Say, you call this when creating a todo:

```js
store.dispatch(addTodo('Use Redux'))
```

To log the action and state, you can change it to something like this:

```js
const action = addTodo('Use Redux')

console.log('dispatching', action)
store.dispatch(action)
console.log('next state', store.getState())

```

This produces the desired effect, but you wouldn't want to do it every time.

### Attempt #2: Wrapping Dispatch

You can extract logging into a function:

```js
function dispatchAndLog(store, action) {
  console.log('dispatching', action)
  store.dispatch(action)
  console.log('next state', store.getState())
}

```

You can then use it everywhere instead of `store.dispatch()`:

```js
dispatchAndLog(store, addTodo('Use Redux'))

```

We could end this here, but it's not very convenient to import a special function every time.

### Attempt #3: Monkeypatching Dispatch

What if we just replace the `dispatch` function on the store instance? The Redux store is just a plain object with [a few methods](https://redux.js.org/api/store), and we're writing JavaScript, so we can just monkeypatch the `dispatch` implementation:

```js
const next = store.dispatch
store.dispatch = function dispatchAndLog(action) {
  console.log('dispatching', action)
  let result = next(action)
  console.log('next state', store.getState())
  return result
}

```

This is already closer to what we want! No matter where we dispatch an action, it is guaranteed to be logged. Monkeypatching never feels right, but we can live with this for now.

### Problem: Crash Reporting

What if we want to apply **more than one** such transformation to `dispatch`?

A different useful transformation that comes to my mind is reporting JavaScript errors in production. The global `window.onerror` event is not reliable because it doesn't provide stack information in some older browsers, which is crucial to understand why an error is happening.

Wouldn't it be useful if, any time an error is thrown as a result of dispatching an action, we would send it to a crash reporting service like [Sentry](https://getsentry.com/welcome/) with the stack trace, the action that caused the error, and the current state? This way it's much easier to reproduce the error in development.

However, it is important that we keep logging and crash reporting separate. Ideally we want them to be different modules, potentially in different packages. Otherwise we can't have an ecosystem of such utilities. (Hint: we're slowly getting to what middleware is!)

If logging and crash reporting are separate utilities, they might look like this:

```js
function patchStoreToAddLogging(store) {
  const next = store.dispatch
  store.dispatch = function dispatchAndLog(action) {
    console.log('dispatching', action)
    let result = next(action)
    console.log('next state', store.getState())
    return result
  }
}

function patchStoreToAddCrashReporting(store) {
  const next = store.dispatch
  store.dispatch = function dispatchAndReportErrors(action) {
    try {
      return next(action)
    } catch (err) {
      console.error('Caught an exception!', err)
      Raven.captureException(err, {
        extra: {
          action,
          state: store.getState()
        }
      })
      throw err
    }
  }
}

```

If these functions are published as separate modules, we can later use them to patch our store:

```js
patchStoreToAddLogging(store)
patchStoreToAddCrashReporting(store)

```

Still, this isn't nice.

### Attempt #4: Hiding Monkeypatching

Monkeypatching is a hack. “Replace any method you like”, what kind of API is that? Let's figure out the essence of it instead. Previously, our functions replaced `store.dispatch`. What if they *returned* the new `dispatch` function instead?

```js
function logger(store) {
  const next = store.dispatch

  // Previously:
  // store.dispatch = function dispatchAndLog(action) {

  return function dispatchAndLog(action) {
    console.log('dispatching', action)
    let result = next(action)
    console.log('next state', store.getState())
    return result
  }
}

```

We could provide a helper inside Redux that would apply the actual monkeypatching as an implementation detail:

```js
function applyMiddlewareByMonkeypatching(store, middlewares) {
  middlewares = middlewares.slice()
  middlewares.reverse()

  // Transform dispatch function with each middleware.
  middlewares.forEach(middleware => (store.dispatch = middleware(store)))
}

```

We could use it to apply multiple middleware like this:

```js
applyMiddlewareByMonkeypatching(store, [logger, crashReporter])
```

However, it is still monkeypatching.
The fact that we hide it inside the library doesn't alter this fact.

### Attempt #5: Removing Monkeypatching

Why do we even overwrite `dispatch`? Of course, to be able to call it later, but there's also another reason: so that every middleware can access (and call) the previously wrapped `store.dispatch`:

```js
function logger(store) {
  // Must point to the function returned by the previous middleware:
  const next = store.dispatch

  return function dispatchAndLog(action) {
    console.log('dispatching', action)
    let result = next(action)
    console.log('next state', store.getState())
    return result
  }
}

```

It is essential to chaining middleware!

If `applyMiddlewareByMonkeypatching` doesn't assign `store.dispatch` immediately after processing the first middleware, `store.dispatch` will keep pointing to the original `dispatch` function. Then the second middleware will also be bound to the original `dispatch` function.

But there's also a different way to enable chaining. The middleware could accept the `next()` dispatch function as a parameter instead of reading it from the `store` instance.

```js
function logger(store) {
  return function wrapDispatchToAddLogging(next) {
    return function dispatchAndLog(action) {
      console.log('dispatching', action)
      let result = next(action)
      console.log('next state', store.getState())
      return result
    }
  }
}

```

It's a [“we need to go deeper”](http://knowyourmeme.com/memes/we-need-to-go-deeper) kind of moment, so it might take a while for this to make sense. The function cascade feels intimidating. ES6 arrow functions make this [currying](https://en.wikipedia.org/wiki/Currying) easier on eyes:

```js
const logger = store => next => action => {
  console.log('dispatching', action)
  let result = next(action)
  console.log('next state', store.getState())
  return result
}

const crashReporter = store => next => action => {
  try {
    return next(action)
  } catch (err) {
    console.error('Caught an exception!', err)
    Raven.captureException(err, {
      extra: {
        action,
        state: store.getState()
      }
    })
    throw err
  }
}

```

**This is exactly what Redux middleware looks like.**

Now middleware takes the `next()` dispatch function, and returns a dispatch function, which in turn serves as `next()` to the middleware to the left, and so on. It's still useful to have access to some store methods like `getState()`, so `store` stays available as the top-level argument.

### Attempt #6: Naïvely Applying the Middleware

Instead of `applyMiddlewareByMonkeypatching()`, we could write `applyMiddleware()` that first obtains the final, fully wrapped `dispatch()` function, and returns a copy of the store using it:

```js
// Warning: Naïve implementation!
// That's *not* Redux API.
function applyMiddleware(store, middlewares) {
  middlewares = middlewares.slice()
  middlewares.reverse()
  let dispatch = store.dispatch
  middlewares.forEach(middleware => (dispatch = middleware(store)(dispatch)))
  return Object.assign({}, store, { dispatch })
}

```

The implementation of [`applyMiddleware()`](https://redux.js.org/api/applymiddleware) that ships with Redux is similar, but **different in three important aspects**:

- It only exposes a subset of the [store API](https://redux.js.org/api/store) to the middleware: [`dispatch(action)`](https://redux.js.org/api/store#dispatchaction) and [`getState()`](https://redux.js.org/api/store#getState).
- It does a bit of trickery to make sure that if you call `store.dispatch(action)` from your middleware instead of `next(action)`, the action will actually travel the whole middleware chain again, including the current middleware. This is useful for asynchronous middleware, as we have seen [previously](https://redux.js.org/advanced/async-actions). There is one caveat when calling `dispatch` during setup, described below.
- To ensure that you may only apply middleware once, it operates on `createStore()` rather than on `store` itself. Instead of `(store, middlewares) => store`, its signature is `(...middlewares) => (createStore) => createStore`.

Because it is cumbersome to apply functions to `createStore()` before using it, `createStore()` accepts an optional last argument to specify such functions.

#### Caveat: Dispatching During Setup

While `applyMiddleware` executes and sets up your middleware, the `store.dispatch` function will point to the vanilla version provided by `createStore`. Dispatching would result in no other middleware being applied. If you are expecting an interaction with another middleware during setup, you will probably be disappointed. Because of this unexpected behavior, `applyMiddleware` will throw an error if you try to dispatch an action before the set up completes. Instead, you should either communicate directly with that other middleware via a common object (for an API-calling middleware, this may be your API client object) or waiting until after the middleware is constructed with a callback.

### The Final Approach

Given this middleware we just wrote:

```js
const logger = store => next => action => {
  console.log('dispatching', action)
  let result = next(action)
  console.log('next state', store.getState())
  return result
}

const crashReporter = store => next => action => {
  try {
    return next(action)
  } catch (err) {
    console.error('Caught an exception!', err)
    Raven.captureException(err, {
      extra: {
        action,
        state: store.getState()
      }
    })
    throw err
  }
}

```

Here's how to apply it to a Redux store:

```js
import { createStore, combineReducers, applyMiddleware } from 'redux'

const todoApp = combineReducers(reducers)
const store = createStore(
  todoApp,
  // applyMiddleware() tells createStore() how to handle middleware
  applyMiddleware(logger, crashReporter)
)

```

That's it! Now any actions dispatched to the store instance will flow through `logger` and `crashReporter`:

```js
// Will flow through both logger and crashReporter middleware!
store.dispatch(addTodo('Use Redux'))

```

## Seven Examples

If your head boiled from reading the above section, imagine what it was like to write it. This section is meant to be a relaxation for you and me, and will help get your gears turning.

Each function below is a valid Redux middleware. They are not equally useful, but at least they are equally fun.

```js
/**
 * Logs all actions and states after they are dispatched.
 */
const logger = store => next => action => {
  console.group(action.type)
  console.info('dispatching', action)
  let result = next(action)
  console.log('next state', store.getState())
  console.groupEnd()
  return result
}

/**
 * Sends crash reports as state is updated and listeners are notified.
 */
const crashReporter = store => next => action => {
  try {
    return next(action)
  } catch (err) {
    console.error('Caught an exception!', err)
    Raven.captureException(err, {
      extra: {
        action,
        state: store.getState()
      }
    })
    throw err
  }
}

/**
 * Schedules actions with { meta: { delay: N } } to be delayed by N milliseconds.
 * Makes `dispatch` return a function to cancel the timeout in this case.
 */
const timeoutScheduler = store => next => action => {
  if (!action.meta || !action.meta.delay) {
    return next(action)
  }

  const timeoutId = setTimeout(() => next(action), action.meta.delay)

  return function cancel() {
    clearTimeout(timeoutId)
  }
}

/**
 * Schedules actions with { meta: { raf: true } } to be dispatched inside a rAF loop
 * frame.  Makes `dispatch` return a function to remove the action from the queue in
 * this case.
 */
const rafScheduler = store => next => {
  const queuedActions = []
  let frame = null

  function loop() {
    frame = null
    try {
      if (queuedActions.length) {
        next(queuedActions.shift())
      }
    } finally {
      maybeRaf()
    }
  }

  function maybeRaf() {
    if (queuedActions.length && !frame) {
      frame = requestAnimationFrame(loop)
    }
  }

  return action => {
    if (!action.meta || !action.meta.raf) {
      return next(action)
    }

    queuedActions.push(action)
    maybeRaf()

    return function cancel() {
      queuedActions = queuedActions.filter(a => a !== action)
    }
  }
}

/**
 * Lets you dispatch promises in addition to actions.
 * If the promise is resolved, its result will be dispatched as an action.
 * The promise is returned from `dispatch` so the caller may handle rejection.
 */
const vanillaPromise = store => next => action => {
  if (typeof action.then !== 'function') {
    return next(action)
  }

  return Promise.resolve(action).then(store.dispatch)
}

/**
 * Lets you dispatch special actions with a { promise } field.
 *
 * This middleware will turn them into a single action at the beginning,
 * and a single success (or failure) action when the `promise` resolves.
 *
 * For convenience, `dispatch` will return the promise so the caller can wait.
 */
const readyStatePromise = store => next => action => {
  if (!action.promise) {
    return next(action)
  }

  function makeAction(ready, data) {
    const newAction = Object.assign({}, action, { ready }, data)
    delete newAction.promise
    return newAction
  }

  next(makeAction(false))
  return action.promise.then(
    result => next(makeAction(true, { result })),
    error => next(makeAction(true, { error }))
  )
}

/**
 * Lets you dispatch a function instead of an action.
 * This function will receive `dispatch` and `getState` as arguments.
 *
 * Useful for early exits (conditions over `getState()`), as well
 * as for async control flow (it can `dispatch()` something else).
 *
 * `dispatch` will return the return value of the dispatched function.
 */
const thunk = store => next => action =>
  typeof action === 'function'
    ? action(store.dispatch, store.getState)
    : next(action)

// You can use all of them! (It doesn't mean you should.)
const todoApp = combineReducers(reducers)
const store = createStore(
  todoApp,
  applyMiddleware(
    rafScheduler,
    timeoutScheduler,
    thunk,
    vanillaPromise,
    readyStatePromise,
    logger,
    crashReporter
  )
)
```