# Intro to ReactJs Coding Notes

Coding notes for ReactJs ^_^

Written for OpenSUTD since there is no one written that.

I strongly suggest you to write the codes out rather than copy paste.

I write this code to understand each part of the code and as concisely as possible

If you want to have a guided version, look it up on: https://www.taniarascia.com/getting-started-with-react/

## What is ReactJs?

------

> React is a declarative, efficient, and flexible JavaScript library for building user interfaces. 
>
> Used for Web development only!!! Don't say "ReactJs is for database management hor!" I get that a lot...
>
> It lets you compose complex User Interface(UIs) from small and isolated pieces of code called “components”.

```javascript
class ShoppingList extends React.Component {
  render() {
    return (
      <div className="shopping-list">
        <h1>Shopping List for {this.props.name}</h1>
        <ul>
          <li>Instagram</li>
          <li>WhatsApp</li>
          <li>Oculus</li>
        </ul>
      </div>
    );
  }
}

// Example usage: <ShoppingList name="Mark" />
```

This is one example of the ReactJs jsx. extension file. It encapsulates html features in it!

### ReactJs Feature

- **JSX** − JSX is JavaScript syntax extension. It isn't necessary to use JSX in React development, but it is recommended.
- **Components** − React is all about components. You need to think of everything as a component. This will help you maintain the code when working on larger scale projects.
- **Unidirectional data flow and Flux** − React implements one-way data flow which makes it easy to reason about your app. Flux is a pattern that helps keeping your data unidirectional.

### Advantages

- Uses **virtual DOM** which is a JavaScript object. This will **improve apps performance**, since JavaScript virtual DOM is faster than the regular DOM.
- **Can be used on client and server side** as well as with other frameworks.
- Component and data patterns **improve readability,** which helps to maintain larger apps.



## Pre-requisite

------

- **JavaScript**: You learn it up on my JavaScript coding notes:
- **HTML & CSS**: Yup! You need it. They are the trio to make web dev possible!
  - Fyi, if you think that HTML is a programming language then I suggest you read/ research before embarking on learning ReactJs. Teehee!
- **LOTS OF RESILENCE**
  - Actually, that applies for all programming stuff... Especially the debugging. And don't complain about it, but rather learn from them!

Okay! Let's get started then!!

## Installation

You would need to install 

- node.js: https://nodejs.org/en/   // Please install this first! And globally!

- ReactJs & React-Dom:

   Type in CLI:

```
npm install react react-dom --save
```

- Webpack:

```
npm install webpack webpack-dev-server webpack-cli --save
```

- Babel:

```
npm install babel-core babel-loader babel-preset-env 
   babel-preset-react html-webpack-plugin --save-dev
```

What I did just install?

Webpack: An open-source JavaScript module bundler. A tool for bundling application source code in convenient chunks and for loading that code from a server into a browser.

Babel:  A free and open-source JavaScript compiler that is mainly used to convert ECMAScript 2015+ code into a backwards compatible version of JavaScript that can be run by older JavaScript engines.

Strongly Recommended:

> GUI: Visual Code Studio: https://code.visualstudio.com/
>
> React Chrome Extension: https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi?hl=en

## Big Picture

---

![Big Picture](https://github.com/huixiang01/javascript_coding_notes/blob/master/ReactJs%20Notes/1.%20Intro%20to%20ReactJs/1_8vfMJjmH-Z3uxEP_RGKTBg.png?raw=true)

You can see the front-end interaction with ReactJs. But no worries! We would talk about the middle part, which is ReactJs.

## Setup to Localhost

---

Choose your local repository through your CLI by using cd, cd.. , cd\

And set up a template of ReactJs in CLI using this command

```javascript
npx create-react-app react-tutorial
```

Make sure that you have 5.2 or higher in Node.js

```javascript
node -v
```

Once that finishes installing, move to the newly created directory and start the project.

```javascript
cd react-tutorial
npm start
```

Once you run this command, a new window will popup at `localhost:3000` with your new React app.

## Intro to ReactJs

---

Let's say you wanna make a tic tac toe!

Go to App.jsx to change the codes.

```javascript
import React from 'react' // import react modules
import ReactDOM from 'react-dom'
import './index.css' //this essentially does nothing for this notes but good to have this in place.

function Square(props) {
  return (
    <button className="square" onClick={props.onClick}>
      {props.value}
    </button>
  );
}
// This is an object-oriented function its **properties** from its parents by using props.
// Note: props is an built-in function. Whhenever we talk about props, it is carried down from parents.
// onClick is built-in fucntion for components for what happens when clicked
// You can choose the properties you want, just like normal classes!

class Board extends React.Component {
  renderSquare(i) { // A declared fucntion to render stuff
    return (
      <Square
        value={this.props.squares[i]}
        onClick={() => this.props.onClick(i)} // Be aware of what is this referring to! Initially, it is confusing, but you will eventually get used to it if you practise more!
      />
    );
  }

  render() { // render is to display onto browser
    return (
      <div>
        <div className="board-row"> // Good to declare className cus you may use it in CSS
          {this.renderSquare(0)} // insert the values here through a fucntion
          {this.renderSquare(1)}
          {this.renderSquare(2)}
        </div>
        <div className="board-row">
          {this.renderSquare(3)}
          {this.renderSquare(4)}
          {this.renderSquare(5)}
        </div>
        <div className="board-row">
          {this.renderSquare(6)} 
          {this.renderSquare(7)}
          {this.renderSquare(8)}
        </div>
      </div>
    );
  }
}

class Game extends React.Component {
  constructor(props) { //standard procedure to inherit class
    super(props);
    this.state = { 
        // state is a private value for Game. Hence, do not put this.state = this.props
        // The current state that you define will trigger a function or show up the values on broswer, etc. It is up to your imagination
        // Try not to put the whole storage into state when there is back-end integration. Only put what you need! I rather call APIs to save time rendering. Check out React Dev Tools to see how much time you spent rendering.
      history: [
        {
          squares: Array(9).fill(null)
        }
      ],
      stepNumber: 0,
      xIsNext: true
    };
  }

  // various functions to declare
  handleClick(i) {
    const history = this.state.history.slice(0, this.state.stepNumber + 1);
      // Applying Immutability in this.state.history. We would just copy paste out into a const variable
    const current = history[history.length - 1];
    const squares = current.squares.slice();
    if (calculateWinner(squares) || squares[i]) {
      return;
    }
    squares[i] = this.state.xIsNext ? "X" : "O"; 
      // shorthand if/else are good in making codes neat.
      // As you can see, this.state can be used as odd/even counter
    this.setState({
      history: history.concat([
        {
          squares: squares
        }
      ]),
      stepNumber: history.length,
      xIsNext: !this.state.xIsNext //True to False
    });
  }
    // this.state is changed through this.setSate({key:value})
    
		
  jumpTo(step) {
    this.setState({
      stepNumber: step,
      xIsNext: (step % 2) === 0
    });
  }

  render() {
    const history = this.state.history; 
    const current = history[this.state.stepNumber];
    const winner = calculateWinner(current.squares);

    const moves = history.map((step, move) => {
      const desc = move ?
        'Go to move #' + move :
        'Go to game start';
      return (
        <li key={move}>
          <button onClick={() => this.jumpTo(move)}>{desc}</button>
        </li>
      );
    });

    let status;
    if (winner) {
      status = "Winner: " + winner;
    } else {
      status = "Next player: " + (this.state.xIsNext ? "X" : "O");
    }

    return (
      <div className="game">
        <div className="game-board">
          <Board
            squares={current.squares}
            onClick={i => this.handleClick(i)} // Displaying the value through onClick function
          />
        </div>
        <div className="game-info">
          <div>{status}</div>
          <ol>{moves}</ol>
        </div>
      </div>
    );
  }
}

// ========================================

ReactDOM.render(<Game />, document.getElementById("root"));
// render everything onto the browser using Game class, and onto the "root" in html

function calculateWinner(squares) {
  const lines = [
    [0, 1, 2],
    [3, 4, 5],
    [6, 7, 8],
    [0, 3, 6],
    [1, 4, 7],
    [2, 5, 8],
    [0, 4, 8],
    [2, 4, 6]
  ]; // state the requirement to win
  for (let i = 0; i < lines.length; i++) {
    const [a, b, c] = lines[i];
    if (squares[a] && squares[a] === squares[b] && squares[a] === squares[c]) {
      return squares[a];
    }
  } // Use loop function to check.
  return null;
}
```

This should be a display the end-product!

![StartingTTT](https://github.com/huixiang01/javascript_coding_notes/blob/master/ReactJs%20Notes/1.%20Intro%20to%20ReactJs/StartingTTT.JPG?raw=true)

![In-processTTT](https://github.com/huixiang01/javascript_coding_notes/blob/master/ReactJs%20Notes/1.%20Intro%20to%20ReactJs/EndingTTT.JPG?raw=true)

![EndingTTT](https://github.com/huixiang01/javascript_coding_notes/blob/master/ReactJs%20Notes/1.%20Intro%20to%20ReactJs/EndingTTT.JPG?raw=true)





