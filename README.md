# Intro to React State

## Learning Objectives
 - Learn about state
 - Learn how to declare state in a React component
 - Learn how to iterate over some data and render it

<!--SEI1 5:23 -->

## State

State is the data for a particular view of a React component.

Let's imagine we have a component that has a shopping cart.

At first, our cart is empty, so our `state` would likely have an empty array.

Then we add an item into our cart. We'd push an object like this one into our cart:

```js
{
itemName: 'Jar of Speculoos',
description: 'Imagine butter cookies dissolved in butter, made into cookie butter and stored in a jar. Stop imagining and now buy this!',
price: 6.99
}
```

Now our view of our shopping cart will change, based on the data or the `state` of the shopping cart.

### Set Up
Let's build a tiny online store and render the items available to us.

- `mkdir react_state_store`
- `cd react_state_store `
- `touch app.js index.html`
- `cp ./../data.js  .`
- set up your html boilerplate and add all the React script tags
- link the `data.js` file above `app.js` (both below the React script tags)
- add a `div` with a class of `container` where we'll render our react
- test that you can see your data with `console.table(products)` on the first line in `app.js`

<!--
<!DOCTYPE html>
<html lang="en" dir="ltr">
  <head>
    <meta charset="utf-8">
    <title>React State Store</title>
  </head>
  <body>
    <div class="container">
    </div>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/react/16.3.2/umd/react.production.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/react-dom/16.3.2/umd/react-dom.production.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/babel-standalone/6.26.0/babel.min.js"></script>
    <script type="text/babel" src="data.js"></script>
    <script type="text/babel" src="app.js"></script>
  </body>
</html>
-->

<!--SEI1 5:38 after questions -->

## Render a list

![try console.table instead of console.log](https://i.imgur.com/wo7ayxR.png)

We want to be able to see an unordered list of our product names in the browser.

This is exactly what React is designed to do: render views based on data.

Let's set up our React app first.

**app.js**

```js
class App extends React.Component {
  render () {
    return (
      <div>
        <h1> Big Time Shopping </h1>
      </div>
    )
  }
}

ReactDOM.render(
  <App />,
  document.querySelector('.container')
)
```

<!--SEI1 5:41 -->

Currently, our app has no `state` (no data for our view). That's ok! Not all components have to have `state`. A simple navigation bar that is just 'hard coded' can be a react component - some components are just for presentation.

However, in the case of our online store, we'll want a list based on our data, so we will add `state`.

`state` is a special key word in react. In order to use it, we used to have to set up a `constructor` function, like we did in Unit 1.

```js
class App extends React.Component {
  constructor (props) {
    super(props)
    this.state = {
      products: products
    }
  }
  render () {
    return (
      <div>
        <h1> Big Time Shopping </h1>
      </div>
    )
  }
}
```

>**Note:** You do not need to add the above code to your app, but keep it in mind because you may see this syntax in Stack Overflow.

Thanks to improvements in recent versions of JS and React, though, we can just do the following now:

```js
class App extends React.Component {
  state = {
    products
  }
  render () {
    return (
      <div>
        <h1> Big Time Shopping </h1>
      </div>
    )
  }
}
```

We can look at our React dev tools and now see that our products are being stored in the `App` component's `state`.

![app state](https://i.imgur.com/XAxOGgh.png)

Inside the `return` of `render()` function is special. The syntax that is recognized is JSX. JSX is a mishmash of HTML and JavaScript. There are a few gotchas inside the return, for instance we can't console log. We'll see others during this unit.

Much like we did in unit 2, we can embed our data in the html. Let's put the first product in there for rendering. We have to wrap any JS we want to render in curlies `{}`.

```js
render () {
  return (
    <div>
      <h1> Big Time Shopping </h1>
      {this.state.products[0].name}
    </div>
  )
}
```
We should see the allen wrench show up.

Unlike EJS (or handlebars), we can't write for loops here. But what we can use is the `.map` function. `.map` will iterate over every item, manipulate it in some way, and return the new array.

In our case, we want to make an unordered list:


```js
render () {
  return (
    <div>
      <h1> Big Time Shopping </h1>
      <ul>
        {
          this.state.products.map(product => {
            return <li>{product.name}</li>
          })
        }
      </ul>
    </div>
  )
}
```

<!--SEI1 5:54 after questions -->

With ES6, if our function is one line of code (and no more), we can skip the curly braces after the arrow and we can skip the return statement - it will implicitly return the one line.

```js
render () {
  return (
    <div>
      <h1> Big Time Shopping </h1>
      <ul>
        {
          this.state.products.map(product =>
            <li>{product.name}</li>
          )
        }
      </ul>
    </div>
  )
}
```

In our case, we may also want to show the price, so let's update our code:

```js
<li>{product.name} - {product.price}</li>
```

<!--SEI1 6:07  after questions -->
