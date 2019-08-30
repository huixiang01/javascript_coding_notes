# Lifecycles

When react renders, it will go through 5 stages:

- **Initialization:** This is the stage where the component is constructed with the given Props and default state. This is done in the constructor of a Component Class.
- **Mounting:** Mounting is the stage of rendering the JSX returned by the render method itself.
- **Updating:** Updating is the stage when the state of a component is updated and the application is repainted.
- **Unmounting:** As the name suggests Unmounting is the final step of the component lifecycle where the component is removed from the page.
- **Error Handling**: Sometimes code doesn’t run or there’s a bug somewhere

![LifeCycle Diagram](C:\Users\tongh\Desktop\javascript_coding_notes\ReactJs Notes\2. LifeCycles\LifeCycle Diagram.JPG)



There are a lot of times that you won't need to use this. But when there is some traffic mitigation to be done, you should use them!

## Hands On!

---

### render()

> The most used lifecycle method. You will see it in all React classes
>
> Happens between mounting and updating of your component

```javascript
class Hello extends React.Component {
    constructor(props) {
        super(props);
        this.state = {
            name: "SUTD!"
        };
    };
    render() {
        return ( 
          <div>
            <h1>Hello {this.state.name} </h1>
          </div >
        );
    };
};

ReactDOM.render( <Hello /> ,
    document.getElementById('root')
);
```

React requires that your *render()* is pure. Pure functions are those that do not have any side-effects and will always return the same output when the same inputs are passed. 

This means that you can not *setState()* within a *render().*

If you need to modify state that would have to happen in the other lifecycle methods, therefore keeping *render()* pure.

### componentDidMount()

After you have rendered, you are mounted and ready to go. This is where componentDidMount() comes in.

This is a good place to call APIs if you need to load data from a remote endpoint

Different from the render() method, it allows the usage of setState(). Calling it here will cause another round of rendering but before the UI in browser shows up.

```javascript
import React, { Component } from 'react';

class Hello extends React.Component {
    constructor(props) {
        super(props);
        this.state = {
            name: "World!"
        };
    };
   getData(){
    setTimeout(() => {
      console.log('Our data is fetched');
      this.setState({
        name: 'World?'
      })
    }, 1000)
  }
    
  componentDidMount(){
    this.getData();
  }
    render() {
        return ( 
          <div>
            <h1>Hello {this.state.name} </h1>
          </div >
        );
    };
};

ReactDOM.render( <Hello /> ,
    document.getElementById('root')
);

```

**Warning**: It is recommended that you use this with caution since it could lead to performance issues. The best practice is to ensure that your states are assigned in the *constructor().* The reason React allows the *setState()* within this lifecycle method is for special cases like tooltips, modals, and similar concepts when you would need to measure a DOM node before rendering something that depends on its position.

FYI, ignore the error boundary. If you want to know more about it, go to: https://reactjs.org/docs/error-boundaries.html

### componentDidUpdate()

Occurs as soon as the updating happens. The most common use case for the *componentDidUpdate()* method is updating the DOM in response to prop or state changes.

You can call *setState()* in this lifecycle, but keep in mind that you will need to wrap it in a condition to check for state or prop changes from previous state. Incorrect usage of *setState()* can lead to an infinite loop.

Take a look at the example below that shows a typical usage example of this lifecycle method.

```javascript
componentDidUpdate(prevProps) {
 //Typical usage, don't forget to compare the props
 if (this.props.userName !== prevProps.userName) {
   this.fetchData(this.props.userName);
 }
}
```

Notice in the above example that we are comparing the current props to the previous props. This is to check if there has been a change in props from what it currently is. In this case, there won’t be a need to make the API call if the props did not change.

### componentWillUnmount()

As the name suggests this lifecycle method is called just before the component is unmounted and destroyed. If there are any cleanup actions that you would need to do, this would be the right spot.

This component will never be re-rendered and because of that we cannot call *setState()* during this lifecycle method.

```javascript
componentWillUnmount() {
 window.removeEventListener('resize', this.resizeListener)
}
```

Common cleanup activities performed in this method include, clearing timers, cancelling api calls, or clearing any caches in storage.

## Uncommon React Lifecycle Methods

We now have a good idea of all the commonly used React lifecycle methods. Besides that, there are other lifecycle methods that React offers which are sparingly used or not used at all.

### shouldComponentUpdate()

This lifecycle can be handy sometimes when you don’t want React to render your state or prop changes.

Anytime *setState()* is called, the component re-renders by default. The *shouldComponentUpdate()* method is used to let React know if a component is not affected by the state and prop changes.

Keep in mind that this lifecycle method should be sparingly used, and it exists only for certain performance optimizations. You cannot update component state in *shouldComponentUpdate()* lifecycle.

**Caution:** Most importantly, do not always rely on it to prevent rendering of your component, since it can lead to several bugs.

```javascript
shouldComponentUpdate(nextProps, nextState) {
 return this.props.title !== nextProps.title || 
  this.state.input !== nextState.input }
```

As shown in the example above, this lifecycle should always return a boolean value to the question, “***Should I re-render my component?***”

### static getDerivedStateFromProps()

This is one of the newer lifecycle methods introduced very recently by the React team.

This will be a safer alternative to the previous lifecycle method *componentWillReceiveProps().*

It is called just before calling the *render*() method.

This is a *static* function that does not have access to “*this*“.  *getDerivedStateFromProps()* returns an object to update *state* in response to *prop* changes. It can return a *null* if there is no change to state.

This method also exists only for rare use cases where the state depends on changes in props in a component.

```javascript
static getDerivedStateFromProps(props, state) {
    if (props.currentRow !== state.lastRow) {
      return {
        isScrollingDown: props.currentRow > state.lastRow,
        lastRow: props.currentRow,
      };
    }
    // Return null to indicate no change to state.
    return null;
  }
```

Keep in mind that this lifecycle method is fired on **every** render.

An example use case where this method may come in handy would be a <Transition> component that compares its previous and next children to decide which ones to animate in and out.

### getSnapshotBeforeUpdate()

*getSnapshotBeforeUpdate()* is another new lifecycle method introduced in React recently. This will be a safer alternative to the previous lifecycle method *componentWillUpdate**().*

```javascript
getSnapshotBeforeUpdate(prevProps, prevState) {
    // ...
  }
```

It is called right before the DOM is updated. The value that is returned from *getSnapshotBeforeUpdate()* is passed on to *componentDidUpdate().*

Keep in mind that this method should also be used rarely or not used at all.

Resizing the window during an async rendering is a good use-case of when the *getSnapshotBeforeUpdate()* can be utilized.

## Recap

- React component lifecycle has three categories – Mounting, Updating and Unmounting.
- The render() is the most used lifecycle method.
  - It is a pure function.
  - You cannot set state in render()
- The componentDidMount() happens as soon as your component is mounted.
  - You can set state here but with caution.
- The componentDidUpdate() happens as soon as the updating happens.
  - You can set state here but with caution.
- The componentWillUnmount() happens just before the component unmounts and is destroyed.
  - This is a good place to cleanup all the data.
  - You cannot set state here.
- The shouldComponentUpdate() can be used rarely.
  - It can be called if you need to tell React not to re-render for a certain state or prop change.
  - This needs to be used with caution only for certain performance optimizations.
- The two new lifecycle methods are getDerivedStateFromProps() and getSnapshotBeforeUpdate().
  - They need to be used only occasionally.
  - Not many examples are out there for these two methods and they are still being discussed and will have more references in the future.

![1567163571263](C:\Users\tongh\AppData\Roaming\Typora\typora-user-images\1567163571263.png)



## More Information

---

Look up on: https://reactjs.org/docs/react-component.html

## References

---

https://reactjs.org/docs/react-component.html

https://www.geeksforgeeks.org/reactjs-lifecycle-components/

https://programmingwithmosh.com/javascript/react-lifecycle-methods/

https://devhints.io/react