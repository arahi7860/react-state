[![General Assembly Logo](https://camo.githubusercontent.com/1a91b05b8f4d44b5bbfb83abac2b0996d8e26c92/687474703a2f2f692e696d6775722e636f6d2f6b6538555354712e706e67)](https://generalassemb.ly/education/web-development-immersive)

# React Components Part 2: React State

We've already seen how to define components using functions. Most of the
components you define will be function components like this because they'll be
easy to write and use. But, some of your components will need to be a little
more dynamic. For that, we'll need to implement state.

## Prerequisites

- HTML, CSS, JavaScript
- React function components and props

## Objectives

By the end of this, developers should be able to:

- Review passing data to a React component via `props`
- Define and use nested components
- Identify `state` in a React app
- Modify the `state` of a React component through events
- Distinguish container and presentational components

## Introduction

In this lesson we will be looking at how data is managed within a React
application. In particular, we will compare and contrast a component's `props`
and `state`. They are similar, but have a couple key distinctions:

- `props` are passed into a component, but `state` is local or native to the
  component
- While we cannot change `props` (immutable) from within a component, we can
  change a component's `state` (mutable).

First, let's revisit the fundamental unit of any React app: **components**.

## Components

We want most of the components in our application to be small and reusable.
That's how we really leverage the power of React.

While components should be independent, we still need them to talk to each other
by passing data. However, to keep components small and focused, we pass only the
data that is _specific to that component's purpose_.

Data that is passed into one component by a parent component (or the application
root), we refer to as `props`.

Lets take a look at two components.
`index.js`
```jsx
import React from "react";
import ReactDOM from "react-dom";
import App from "./App.js";

ReactDOM.render(
  <App/>,
  document.getElementById("root")
);
```
`App.js`

```jsx
import React from "react";
import Hello from "./Hello.js";

import React from 'react';
import Hello from './Hello'

function App() {
  return (
    <div>
      <Hello name={'Nick'} age={24}/>
    </div>
  );
}

export default App;
```


```js
// Hello.js
function Hello(props) {
  return (
    <div>
      {/* name and age are immutable */}
      <h1>Hello {props.name}</h1>
      <p>You are {props.age} years old</p>
    </div>
  );
}
```

Here we are passing values, `"Nick"` and `24` into our first `Hello` component
in `App.js`, where we are **composing** `Hello` with the JSX expression,
`<Hello name={"Nick"} age={24} />`.

Any values that we pass into a component are called `props`. Inside of the
component we refer to them as `props`

Props are one of the things that make React so powerful and help us make
**independent** and **reusable** components. We can pass different data to our
`Hello` component and easily get the same html and css but with different data:

```jsx
function App() {
  return (
  <>
    <Hello name={"Nick"} age={24} />
    <Hello name={"Billy"} age={30} />
    <Hello name={"Sheryl"} age={24} />
    <Hello name={"Spencer"} age={15} />
   </>
  )
}
```

We cannot change the values of received `props` inside a component - they are
**immutable**.

So what do we do with data we want to control from within a component?

## State

The limitation of props is that we can't change the data from within the
component. The data that we can change within a component is called
**[state](https://facebook.github.io/react/docs/state-and-lifecycle.html)**.
While we haven't talked about state yet, you have worked with it before! Your
project 4s all tracked some state, regardless of which game you built:

- **Trivia**: what is the current score, what card is currently displayed to the
  user, is the user's input correct or incorrect?
- **Simon**: what order of buttons did the user push, what is the order of
  buttons they were supposed to push, what round or level are they on?
- **Tower of Hanoi**: how many discs are in each tower, how many moves has the
  user made?

We can figure out the `state` of a turn-based game because there is a clear idea
of a beginning and end and states that reflect progress from one turn to the
next turn: what flash card is the user on, what buttons do they need to push,
how are the discs distributed among the three towers.

<details>
  <summary>Q: So we know an application can have different states. But how do we transition in between them?</summary>

> A: Events! (or user actions/input)

</details>

### State and Rendering

Before moving on to build our application, it's worth mentioning another aspect
of component `state`: when it changes, our components re-render:

![](./images/react-component-state-update.png)

Our UI gets updated when state changes. The user takes some action, like
submitting information via a form, and the component holding that form has a
`state` that is updated with the value of the user's input.

## Check for Understanding

- What is the difference between `state` and `props`?
- What do we use `props` for?
- What do we use `state` for?

<details>
<summary>Solution</summary>
- State is mutable and is designed to be passed to and from components and allows you to share information</br></br>
- To store information that is specific to a component and that you don't intend to change </br>
</details>

We've done a fair amount of framing so far, so let's dive in to building our
application!

## A Simple Example

We'll walk through these different pieces by building out a simple counter
component.

```js
function Counter() {
  const [count, setCount] = useState(0)

  const increment = () => setCount(count + 1)
  const decrement = () => setCount(count - 1)

  return (
    <div>
      <span>Current Count: ${count}</span>
      <button onClick={increment}>+</button>
      <button onClick={decrement}>-</button>
    </div>
  )
}
```
## Array Destructuring

```js
const [count, setCount] = useState(0)
```

The first thing we notice here is the `const [count, setCount] = ...`. If this syntax is unfamiliar, we are simply using [ES6 Array Destructuring](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment). `useState` returns an array with two items: the first is the value of the state, and the second is a function you can use to update that state, triggering a re-render. The above could be rewritten as:

```js
const countState = useState(0)
const count = countState[0]
const setCount = countState[1]
```

## Initial State

Back to this line:

```js
const [count, setCount] = useState(0)
```

We see that we call `useState` with a value of `0`. This argument is the initial state - in this example, `count` will be assigned to `0`.


## Calling the update function

Moving on to the next two lines:

```js
  const increment = () => setCount(count + 1)
  const decrement = () => setCount(count - 1)
```

Here, we are creating two new functions that will call `setCount`. These functions are then assigned to the buttons as handlers:

```js
  return (
    <div>
      <span>Current Count: ${count}</span>
      <button onClick={increment}>+</button>
      <button onClick={decrement}>-</button>
    </div>
  )
```

When we click the `+` button, the `increment` function is called, which executes `setCount(count + 1)`. When `setCount` has been called, the component will re-render, and our `count` will have been increased by one:

```js
const [count, setCount] = useState(0) /* same as before, except useState will return an updated count */
```

## Shopping Cart

1. Copy the data in the `data.js` file of this repository.
2. Create a component that renders all of the items in the data.
3. Create a form to add a new item to the cart.

## Closing

If asked, could you explain the differences between props and state? We've now
covered the differences between the two as well as how you can use state to
control data inside a component and how to update state to display new data to a
user.

Defining components and working with props and state (data) constitutes the
majority of the work of building a React application.
