# Learn_React
[Learn React](https://react.dev/learn) - https://react.dev/learn

## Overview

This page will give you an introduction to the 80% of React concepts that you will use on a daily basis.

You will learn
* How to create and nest components
* How to add markup and styles
* How to display data
* How to render conditions and lists
* How to respond to events and update the screen
* How to share data between components

## Creating and nesting components

React apps are made out of components. A component is a piece of the UI (user interface) that has its own logic and appearance. 

```
function MyButton() {
  return (
    <button>I'm a button</button>
  );
}
```

Now that you’ve declared MyButton, you can nest it into another component:

```
export default function MyApp() {
    return (
        <div>
            <h1>Welcome to my app</h1>
            <MyButton />
        </div>
    ) ;
}
```

The result:

```
function MyButton() {
    return (
      <button>
        I'm a button
      </button>
    );
  }
  
export default function MyApp() {
    return (
        <div>
        <h1>Welcome to my app</h1>
        <MyButton />
        </div>
    );
}
```
## Writing markup with JSX

JSX is stricter than HTML. You have to close tags like &lt;br /&gt;. Your component also can’t return multiple JSX tags. You have to wrap them into a shared parent, like a <div>...</div> or an empty <>...</> wrapper:

```
function AboutPage() {
    return (
        <>
            <h1>About</h1>
            <p>Hello there. <br />How do you do?</p>
        </>
    )
}
```

Then you write the CSS rules for it in a separate CSS file:

```
.avatar {
  border-radius: 50%;
}
```

React does not prescribe how you add CSS files. In the simplest case, you’ll add a <link> tag to your HTML. 

## Adding styles

In React, you specify a CSS class with className. It works the same way as the HTML class attribute:

```
<img className="avatar" />
```

## Displaying data

JSX lets you put markup into JavaScript. Curly braces let you “escape back” into JavaScript so that you can embed some variable from your code and display it to the user. For example, this will display user.name:

```
return (
  <h1>
    {user.name}
  </h1>
);
```

You can also “escape into JavaScript” from JSX attributes, but you have to use curly braces _instead_ of quotes. 

```
return (
  <img className="avatar" src={user.imageUrl} />
);
```

You can put more complex expressions inside the JSX curly braces too, for example, string concatenation:

```
const user = {
  name: 'Hedy Lamarr',
  imageUrl: 'https://i.imgur.com/yXOvdOSs.jpg',
  imageSize: 90,
};

export default function Profile() {
  return (
    <>
      <h1>{user.name}</h1>
      <img
        className="avatar"
        src={user.imageUrl}
        alt={'Photo of ' + user.name}
        style={{
          width: user.imageSize,
          height: user.imageSize
        }}
      />
    </>
  );
}

```

In the above example, style={{}} is not a special syntax, but a regular {} object inside the style={ } JSX curly braces. You can use the style attribute when your styles depend on JavaScript variables.

## Conditional rendering

In React, there is no special syntax for writing conditions. Instead, you’ll use the same techniques as you use when writing regular JavaScript code. For example, you can use an if statement to conditionally include JSX:

```
let content;
if (isLoggedIn) {
  content = <AdminPanel />;
} else {
  content = <LoginForm />;
}
return (
  <div>
    {content}
  </div>
);
```

If you prefer more compact code, you can use the conditional ? operator. Unlike if, it works inside JSX:

```
<div>
  {isLoggedIn ? (
    <AdminPanel />
  ) : (
    <LoginForm />
  )}
</div>
```

When you don’t need the else branch, you can also use a shorter logical && syntax:

```
<div>
  {isLoggedIn && <AdminPanel />}
</div>
```


## Rendering lists

You will rely on JavaScript features like for loop and the array map() function to render lists of components.

For example, let’s say you have an array of products:

```
const products = [
  { title: 'Cabbage', id: 1 },
  { title: 'Garlic', id: 2 },
  { title: 'Apple', id: 3 },
];
```

Inside your component, use the map() function to transform an array of products into an array of <li> items:

```
    const listItems = products.map(product =>
    <li key={product.id}>
        {product.title}
    </li>
    );

    return (
    <ul>{listItems}</ul>
    );
```

Notice how <li> has a key attribute. For each item in a list, you should pass a string or a number that uniquely identifies that item among its siblings. Usually, a key should be coming from your data, such as a database ID. React uses your keys to know what happened if you later insert, delete, or reorder the items.

```
const products = [
    { title: 'Cabbage', isFruit: false, id: 1 },
    { title: 'Garlic', isFruit: false, id: 2 },
    { title: 'Apple', isFruit: true, id: 3 },
  ];
  
  export default function ShoppingList() {
    const listItems = products.map(product =>
      <li
        key={product.id}
        style={{
          color: product.isFruit ? 'magenta' : 'darkgreen'
        }}
      >
        {product.title}
      </li>
    );
  
    return (
      <ul>{listItems}</ul>
    );
  }
```
## Responding to events

You can respond to events by declaring event handler functions inside your components:

```
function MyButton() {
  function handleClick() {
    alert('You clicked me!');
  }

  return (
    <button onClick={handleClick}>
      Click me
    </button>
  );
}
```
Notice how onClick={handleClick} has no parentheses at the end! Do not call the event handler function: you only need to pass it down. React will call your event handler when the user clicks the button.

## Updating the screen

Often, you’ll want your component to “remember” some information and display it. For example, maybe you want to count the number of times a button is clicked. To do this, add state to your component.

First, import useState from React:

```
import { useState } from 'react';
```
Now you can declare a state variable inside your component:
```
function MyButton() {
  const [count, setCount] = useState(0);
  // ...
```

You’ll get two things from useState: the current state (count), and the function that lets you update it (setCount). You can give them any names, but the convention is to write [something, setSomething].

The first time the button is displayed, count will be 0 because you passed 0 to useState(). When you want to change state, call setCount() and pass the new value to it. Clicking this button will increment the counter:

```
function MyButton() {
  const [count, setCount] = useState(0);

  function handleClick() {
    setCount(count + 1);
  }

  return (
    <button onClick={handleClick}>
      Clicked {count} times
    </button>
  );
}
```

React will call your component function again. This time, count will be 1. Then it will be 2. And so on.

If you render the same component multiple times, each will get its own state. Click each button separately:

```
import { useState } from 'react';

export default function MyApp() {
  return (
    <div>
      <h1>Counters that update separately</h1>
      <MyButton />
      <MyButton />
    </div>
  );
}

function MyButton() {
  const [count, setCount] = useState(0);

  function handleClick() {
    setCount(count + 1);
  }

  return (
    <button onClick={handleClick}>
      Clicked {count} times
    </button>
  );
}
```

Notice how each button “remembers” its own count state and doesn’t affect other buttons.

## Using Hooks

Functions starting with use are called Hooks. useState is a built-in Hook provided by React. You can find other built-in Hooks in the [API reference](https://react.dev/reference/react). You can also write your own Hooks by combining the existing ones.

Hooks are more restrictive than other functions. You can only call Hooks at the top of your components (or other Hooks). If you want to use useState in a condition or a loop, extract a new component and put it there.


## Sharing data between components

In the previous example, each MyButton had its own independent count, and when each button was clicked, only the count for the button clicked changed:

<img src="001_QuickStart/i_SharingDataBetweenComponents/sharing_data_child.webp" alt="Sharing data child" width="400px"/> <img src="001_QuickStart/i_SharingDataBetweenComponents/sharing_data_child_clicked.webp" alt="Sharing data child" width="400px"/> 

However, often you’ll need components to share data and always update together.

To make both MyButton components display the same count and update together, you need to move the state from the individual buttons “upwards” to the closest component containing all of them.

In this example, it is MyApp:

<img src="001_QuickStart/i_SharingDataBetweenComponents/sharing_data_parent.webp" alt="Sharing data child" width="400px"/> <img src="001_QuickStart/i_SharingDataBetweenComponents/sharing_data_parent_clicked.webp" alt="Sharing data child" width="400px"/> 

Now when you click either button, the count in MyApp will change, which will change both of the counts in MyButton. Here’s how you can express this in code.

First, move the state up from MyButton into MyApp:

```
export default function MyApp() {
  const [count, setCount] = useState(0);

  function handleClick() {
    setCount(count + 1);
  }

  return (
    <div>
      <h1>Counters that update separately</h1>
      <MyButton />
      <MyButton />
    </div>
  );
}

function MyButton() {
  // ... we're moving code from here ...
}
```

Then, pass the state down from MyApp to each MyButton, together with the shared click handler. You can pass information to MyButton using the JSX curly braces, just like you previously did with built-in tags like <img>:

```
export default function MyApp() {
  const [count, setCount] = useState(0);

  function handleClick() {
    setCount(count + 1);
  }

  return (
    <div>
      <h1>Counters that update together</h1>
      <MyButton count={count} onClick={handleClick} />
      <MyButton count={count} onClick={handleClick} />
    </div>
  );
}
```

The information you pass down like this is called props. Now the MyApp component contains the count state and the handleClick event handler, and passes both of them down as props to each of the buttons.

Finally, change MyButton to read the props you have passed from its parent component:

```
function MyButton({ count, onClick }) {
  return (
    <button onClick={onClick}>
      Clicked {count} times
    </button>
  );
}
```
When you click the button, the onClick handler fires. Each button’s onClick prop was set to the handleClick function inside MyApp, so the code inside of it runs. That code calls setCount(count + 1), incrementing the count state variable. The new count value is passed as a prop to each button, so they all show the new value. This is called “lifting state up”. By moving state up, you’ve shared it between components.

```
import { useState } from 'react';

export default function MyApp() {
  const [count, setCount] = useState(0);

  function handleClick() {
    setCount(count + 1);
  }

  return (
    <div>
      <h1>Counters that update together</h1>
      <MyButton count={count} onClick={handleClick} />
      <MyButton count={count} onClick={handleClick} />
    </div>
  );
}
```
## Next Steps

By now, you know the basics of how to write React code!

Check out the [Tutorial](https://react.dev/learn/tutorial-tic-tac-toe) to put them into practice and build your first mini-app with React.

# Tuorial: Tic-Tac-Toe

The tutorial is divided into several sections:

* Setup for the tutorial will give you a starting point to follow the tutorial.
* Overview will teach you the fundamentals of React: components, props, and state.
* Completing the game will teach you the most common techniques in React development.
* Adding time travel will give you a deeper insight into the unique strengths of React.

## Overview
### What are you building?

In this tutorial, you’ll build an interactive tic-tac-toe game with React.

## Setup for the tutorial

In the live code editor below, click Fork in the top-right corner to open the editor in a new tab using the website CodeSandbox. CodeSandbox lets you write code in your browser and preview how your users will see the app you’ve created. The new tab should display an empty square and the starter code for this tutorial.

[Code Sandbox](https://codesandbox.io/s/vy6j3f?file=%2FApp.js&utm_medium=sandpack)

## Overview

### Inspecting the starter code

In CodeSandbox you’ll see three main sections:

1. The Files section with a list of files like App.js, index.js, styles.css and a folder called public
1. The code editor where you’ll see the source code of your selected file
1. The browser section where you’ll see how the code you’ve written will be displayed

The App.js file should be selected in the Files section. The contents of that file in the code editor should be:

```
export default function Square() {
  return <button className="square">X</button>;
}
```

styles.css

index.js

```
import React, { StrictMode } from "react";
import { createRoot } from "react-dom/client";
import "./styles.css";

import App from "./App";

const root = createRoot(document.getElementById("root"));
root.render(
  <StrictMode>
    <App />
  </StrictMode>
);
```

Lines 1-5 brings all the necessary pieces together:

* React
* React’s library to talk to web browsers (React DOM)
* the styles for your components
* the component you created in App.js.
* The remainder of the file brings all the pieces together and injects the final product into index.html in the public folder.

### Building the board
Let’s get back to App.js. This is where you’ll spend the rest of the tutorial.

```
export default function Square() {
  return <button className="square">X</button><button className="square">X</button>;
}
```

```
TypeError
Cannot assign to read only property 'message' of object 'SyntaxError: /App.js: Adjacent JSX elements must be wrapped in an enclosing tag. Did you want a JSX fragment <>...</>? (2:46)

  1 | export default function Square() {
> 2 |   return <button className="square">X</button><button className="square">X</button>;
    |                                               ^
  3 | }'
```

React components need to return a single JSX element and not multiple adjacent JSX elements like two buttons. To fix this you can use fragments (<> and </>) to wrap multiple adjacent JSX elements like this:

```
export default function Square() {
  return (
    <>
      <button className="square">X</button>
      <button className="square">X</button>
    </>
  );
}
```
Great! Now you just need to copy-paste a few times to add nine squares and…
```
export default function Square() {
  return (
    <>
      <button className="square">X</button>
      <button className="square">X</button>
      <button className="square">X</button>
      <button className="square">X</button>
      <button className="square">X</button>
      <button className="square">X</button>
      <button className="square">X</button>
      <button className="square">X</button>
      <button className="square">X</button>
    </>
  );
}
```

Oh no! The squares are all in a single line, not in a grid like you need for our board. To fix this you’ll need to group your squares into rows with divs and add some CSS classes. While you’re at it, you’ll give each square a number to make sure you know where each square is displayed.

In the App.js file, update the Square component to look like this:

```
export default function Square() {
  return (
    <>
      <div className="board-row">
        <button className="square">1</button>
        <button className="square">2</button>
        <button className="square">3</button>
      </div>
      <div className="board-row">
        <button className="square">4</button>
        <button className="square">5</button>
        <button className="square">6</button>
      </div>
      <div className="board-row">
        <button className="square">7</button>
        <button className="square">8</button>
        <button className="square">9</button>
      </div>
    </>
  );
}
```
The CSS defined in styles.css styles the divs with the className of board-row. Now that you’ve grouped your components into rows with the styled divs you have your tic-tac-toe board:

![alt text](002_Tutorial/number-filled-board.png "Number filled board")

But you now have a problem. Your component named Square, really isn’t a square anymore. Let’s fix that by changing the name to Board:

```
export default function Board() {
  //...
}
```

### Passing data through props

Next, you’ll want to change the value of a square from empty to “X” when the user clicks on the square. With how you’ve built the board so far you would need to copy-paste the code that updates the square nine times (once for each square you have)! Instead of copy-pasting, React’s component architecture allows you to create a reusable component to avoid messy, duplicated code.

First, you are going to copy the line defining your first square (<button className="square">1</button>) from your Board component into a new Square component:

```
function Square() {
  return <button className="square">1</button>;
}

export default function Board() {
  // ...
}
```

Then you’ll update the Board component to render that Square component using JSX syntax:

```
export default function Board() {
  return (
    <>
      <div className="board-row">
        <Square />
        <Square />
        <Square />
      </div>
      <div className="board-row">
        <Square />
        <Square />
        <Square />
      </div>
      <div className="board-row">
        <Square />
        <Square />
        <Square />
      </div>
    </>
  );
}
```

Note how unlike the browser divs, your own components Board and Square must start with a capital letter.

Let’s take a look:

![alt text](002_Tutorial/board-filled-with-ones.png "Board filled with ones")

Oh no! You lost the numbered squares you had before. Now each square says “1”. To fix this, you will use props to pass the value each square should have from the parent component (Board) to its child (Square).

Update the Square component to read the value prop that you’ll pass from the Board:

```
function Square({ value }) {
  return <button className="square">1</button>;
}
```

function Square({ value }) indicates the Square component can be passed a prop called value.

Now you want to display that value instead of 1 inside every square. Try doing it like this:

```
function Square({ value }) {
  return <button className="square">{ value }</button>;
}

```

For now, you should see an empty board:

![alt text](002_Tutorial/empty-board.png "Empty board")

This is because the Board component hasn’t passed the value prop to each Square component it renders yet. To fix it you’ll add the value prop to each Square component rendered by the Board component:

```
export default function Board() {
  return (
    <>
      <div className="board-row">
        <Square value="1" />
        <Square value="2" />
        <Square value="3" />
      </div>
      <div className="board-row">
        <Square value="4" />
        <Square value="5" />
        <Square value="6" />
      </div>
      <div className="board-row">
        <Square value="7" />
        <Square value="8" />
        <Square value="9" />
      </div>
    </>
  );
}
```

Now you should see a grid of numbers again:

![alt text](002_Tutorial/number-filled-board.png "Number filled board")

Your updated code should look like this:

```
function Square({ value }) {
  return <button className="square">{value}</button>;
}

export default function Board() {
  return (
    <>
      <div className="board-row">
        <Square value="1" />
        <Square value="2" />
        <Square value="3" />
      </div>
      <div className="board-row">
        <Square value="4" />
        <Square value="5" />
        <Square value="6" />
      </div>
      <div className="board-row">
        <Square value="7" />
        <Square value="8" />
        <Square value="9" />
      </div>
    </>
  );
}

```

### Making an interactive component

Let’s fill the Square component with an X when you click it. Declare a function called handleClick inside of the Square. Then, add onClick to the props of the button JSX element returned from the Square:

```
function Square({ value }) {
  function handleClick() {
    console.log('clicked!');
  }

  return (
    <button className="square" onClick={handleClick}>
      {value}
    </button>
  );
}
```

If you click on a square now, you should see a log saying "clicked!" in the Console tab at the bottom of the Browser section in CodeSandbox. Clicking the square more than once will log "clicked!" again. Repeated console logs with the same message will not create more lines in the console. Instead, you will see an incrementing counter next to your first "clicked!" log.

As a next step, you want the Square component to “remember” that it got clicked, and fill it with an “X” mark. To “remember” things, components use state.

React provides a special function called useState that you can call from your component to let it “remember” things. Let’s store the current value of the Square in state, and change it when the Square is clicked.

Import useState at the top of the file. Remove the value prop from the Square component. Instead, add a new line at the start of the Square that calls useState. Have it return a state variable called value:

```
import { useState } from 'react';

function Square() {
  const [value, setValue] = useState(null);

  function handleClick() {
    //...
```

value stores the value and setValue is a function that can be used to change the value. The null passed to useState is used as the initial value for this state variable, so value here starts off equal to null.

Since the Square component no longer accepts props anymore, you’ll remove the value prop from all nine of the Square components created by the Board component:

```
// ...
export default function Board() {
  return (
    <>
      <div className="board-row">
        <Square />
        <Square />
        <Square />
      </div>
      <div className="board-row">
        <Square />
        <Square />
        <Square />
      </div>
      <div className="board-row">
        <Square />
        <Square />
        <Square />
      </div>
    </>
  );
}
```
Now you’ll change Square to display an “X” when clicked. Replace the console.log("clicked!"); event handler with setValue('X');. Now your Square component looks like this:

```
function Square() {
  const [value, setValue] = useState(null);

  function handleClick() {
    setValue('X');
  }

  return (
    <button
      className="square"
      onClick={handleClick}
    >
      {value}
    </button>
  );
}
```

By calling this set function from an onClick handler, you’re telling React to re-render that Square whenever its &lt;button&gt; is clicked. After the update, the Square’s value will be 'X', so you’ll see the “X” on the game board. Click on any Square, and “X” should show up:

Each Square has its own state: the value stored in each Square is completely independent of the others. When you call a set function in a component, React automatically updates the child components inside too.

After you’ve made the above changes, your code will look like this:



### React Developer Tools

React DevTools let you check the props and the state of your React components. You can find the React DevTools tab at the bottom of the browser section in CodeSandbox:

Note

For local development, React DevTools is available as a Chrome, Firefox, and Edge browser extension. Install it, and the Components tab will appear in your browser Developer Tools for sites using React.

```
import { useState } from 'react';

function Square() {
  const [value, setValue] = useState(null);

  function handleClick() {
    setValue('X');
  }

  return (
    <button
      className="square"
      onClick={handleClick}
    >
      {value}
    </button>
  );
}

export default function Board() {
  return (
    <>
      <div className="board-row">
        <Square />
        <Square />
        <Square />
      </div>
      <div className="board-row">
        <Square />
        <Square />
        <Square />
      </div>
      <div className="board-row">
        <Square />
        <Square />
        <Square />
      </div>
    </>
  );
}
```

## Completing the game

By this point, you have all the basic building blocks for your tic-tac-toe game. To have a complete game, you now need to alternate placing “X”s and “O”s on the board, and you need a way to determine a winner.

### LIfting state up
### Why immutability is important
### Taking turns
### Declaring a winner

## Adding time travel

### Storing a history of moves
### Living state up, again
### Showing the past moves
### Picking a key
### Implementing time travel
### Final cleanup
### Wrapping up