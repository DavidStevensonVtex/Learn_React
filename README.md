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




## Rendering lists
## Responding to events
## Updating the screen
## Using Hooks
## Sharing data between components
## Next Steps
