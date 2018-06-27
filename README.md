# React Router

![React Router Logo](./react-router-logo.png)

## Learning Objectives

* Use React Router's `BrowserRouter`, `Link`, `Route`, `Redirect`, and `Switch` components to add navigation to a React application
* Review the React component lifecycle and use component methods to integrate with API calls
* Talk about SPAs

## Framing (5 min / 0:05)

Up to this point, our React applications have been limited in size, allowing us to use basic control flow in our components' render methods to determine what gets rendered to our users. However, as our React applications grow in size and scope, we need an easier and more robust way of rendering different components. Additionally, we will want the ability to set information in the url parameters to make it easier for users to identify where they are in the application.

React Router, while not the only, is the most commonly-used routing library for React. It is relatively straightforward to configure and integrates with the component architecture nicely (since it's just a collection of components). 

We will configure it as the root component in a React application. Then we'll tell it to render other components within itself depending on the path in the url. This way we don't have to reload the entire page to swap out some data.

Don't confuse it with the express router! They do different things, though they both operate based on paths.

## We Do: [React Bitcoin Prices](https://git.generalassemb.ly/dc-wdi-react-redux/react-bitcoin-prices) Setup (5 min / 0:10)

Let's get set up with the react bitcoin price checker!

## You Do: Axios & Coindesk API (5 min / 0:15)

We will be using [Axios](https://github.com/axios/axios) to query the Coindesk API in this exercise. Take 5 minutes to read and test out (using postman or the browser) the API docs below

[Coindesk API](https://www.coindesk.com/api/)

Also, install the [React developer tools](https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi?hl=en) chrome extension if you haven't already. It'll come in very handy for inspecting components.

## You Do: Examine Current Codebase (15 min / 0:30)

Take 10 minutes and read through the code to familiarize yourself with the codebase with a partner or in groups of 3. Prepare to discuss your answers the following questions:

1. What dependencies is the application currently using? Where can I find information on them?
2. What is the purpose of `ReactDOM.render()`? What file is this method being called in?
3. Where are the components of our application located? Why might we want to separate them into their own folders?
4. Where is state located in our application? Is state being passed down to other components?
5. Where is our application getting data from? How is it accomplishing this?

---

think about what we want to accomplish based on what we have so far
we have 4 components. in one sentence, what does each do?

* app.js
* home.js
* price.js
* currencies.js

let's establish some goals:

* be able to display each of the components
* use that list of currencies to:
  * pass a value into the price component
  * have the price component do its ajax call for us

/install react-router react-router-dom
/import browserRouter in index.js
/configure reactDom.render to use router

look at app.js now
import link, route
we do: define a route to home, add link to /
you do: define a route to point to /currencies component, add link in navbar
you do: add switch component around routes

talk about order of route matching

we do: modify currencies component to use Link instead of anchor
you do: define a route for price component 

talk about params
talk about difference between `render` and `component` in route

fix route component with params, render, passing props in
talk about spread operator

---


## We Do: React Router Setup (10 min / 0:40)

Currently, we are rendering just the App component, which renders the Home component. Let's bring in React Router and set it up to allow us to display multiple components

### Importing Dependencies

First, we need to install `react-router` and `react-router-dom` as dependencies in `package.json`. Running `npm install` should automatically do this for us.

```sh
npm install react-router react-router-dom
```

To configure our current application to use React Router, we need to modify the root rendering of our app in `index.js`. We need to import  the `Router` component and place it as the root component of our application. `Router` will, in turn, render `App` through which all the rest of our components will be rendered:

```jsx
// index.js
import { BrowserRouter as Router } from "react-router-dom";

//...

ReactDOM.render(
  <Router>
    <App />
  </Router>,
  document.getElementById("root")
);
```

> Note that we are aliasing `BrowserRouter` as `Router` here for simplicity

By making `Router` the root component of our app, it will pass down several router-specific objects to its child components. Things like current location and url can be accessed or changed. Additionally, in order to use the other routing components provided by React Router, a `Router` parent component is necessary.

Next, in `App.js`, we need to import all of the other components we want to use from React Router.

The three main ones we're going to use today are:

```jsx
<Route />
<Link />
<Switch />
```

Let's go ahead and import just route and link for now, we'll cover switch later.

```js
// src/components/App/App.js

import { Route, Link } from "react-router-dom";
```

Now that we have access to these components, we need to modify the `App` component's `render()` method to set up navigation. The basic structure we will use is...

```jsx
// src/components/App/App.js

render() {
  return (
    <div>
      <nav>
        <Link to=""></Link>
        <Link to=""></Link>
      </nav>
      <main>
        <Route path="" render={}/>
        // render and component are both options we can use.
        // we will cover them both
        <Route path="" component={}/>
      </main>
    </div>
  )
}
```

> **Link** - a component for setting the URL and providing navigation between different components in our app without triggering a page refresh. It takes a `to` property, which sets the URL to whatever path is defined within it. Link can also be used inside of any component that is connected to a `Route`.

> **Route** - a component that renders a specified component (using either `render` or `component`) based on the current url (`path`) we're at. `path` should probably match a `<Link to="">` defined somewhere.

Now let's modify the render method in `App.js` to include our Link and Route components.

```jsx
// src/components/App/App.js

render() {
  return(
    <div>
      <nav>
        <Link to="/">
          <img src="https://en.bitcoin.it/w/images/en/2/29/BC_Logo_.png" alt=""/>
          <h1>Bitcoin prices</h1>
        </Link>
      </nav>
      <main>
        <Route path="/" 
          component={Home}
        />
      </main>
    </div>
  )
}
```

Great! But this doesn't do anything because we're already on the homepage.

Also, note that we used `component` in this case to display our home component. We're doing that because we just want to display it without any changes - we're not passing anything in, we're not modifying anything.

## You do: Add a Second Route and Link (10 min / 0:50)

> 5 minute exercise / 5 minute review

* Using the above instructions as a guide, add a new Link to `/currencies` and a route to match it. What component do you think you want to render?


<details>
  <summary>Solution</summary>

```jsx
// src/Components/App/App.js
//...
import Currencies from '../Currencies/Currencies'

// ...

render() {
  return(
    <div>
      <nav>
        <Link to="/">
          <img src="https://en.bitcoin.it/w/images/en/2/29/BC_Logo_.png" alt=""/>
          <h1>Bitcoin prices</h1>
        </Link>
        <Link to="/currencies">Currency List</Link>
      </nav>
      <main>
        <Route path="/" 
          component={Home}
        />
        <Route path="/currencies" 
          component={() => <Currencies />}
        />
      </main>
    </div>
  )
}
```
</details>

<!--
<details>
<summary>wat</summary>
</details> -->

Now we've got two components and two routes. Perfect. Let's take a look at our currencies component and see what we need to do to make it work.

## We do: Currencies component (5 min / 0:55)

If we look at this component we see a long list of links. Note that the links are using regular `<a>` tags.

What happens if we click on a link? It works, but the whole page reloads! Gross. Let's fix that.

Go ahead and replace the a tag with a `<Link>` component. Make the `to` prop value equal to the `href` value.

```jsx
// src/Components/Currencies/Currencies.js
import { Link } from 'react-router-dom'

//...

  render() {
    let list = listOfCurrencies.map(item => {
      return (
        <div className="currency" key={item.currency}>
          <p><Link to={"/price/"+ item.currency}>{item.currency}</Link>: {item.country}</p>
        </div>
      )
    })
  }

  // ...
```

Great! Now go back to the page and click the link again, what happens?

**MAGIC!**

![shia](https://media.giphy.com/media/ujUdrdpX7Ok5W/giphy.gif)

It changes the route for us (notice the URL changing) but we don't have any routes set up to match that. Let's do that now.


## Break (10 min / 1:05)

## You do: Prices Component (10 min / 1:15)

> 5 min exercise, 5 min review

Back in `App.js`, we need to add another `<Route>` component. This time though, we want to include a parameter.

Look at the URL that we're on after clicking on a currency. Then look at the Price component. How might you write the `path` prop to make it work?

> Hint: This part is just like defining a route in express.

## We do: Fix prices component (25 min / 1:40)

We've added a route but not everything will work yet. HOW COME!?

There are a couple things we need to fix.

Firstly, we have to make sure we're using `render` instead of `component` in our route. That's because we're going to be passing some props into our component.

```jsx
//...
<Route path="/price/:currency" 
  render={() => <Price />}
/>
//...
```

Now we need to add a couple things to `<Price />`

We've got a function in this (`App.js`) component called setPrice. let's pass that in.

```jsx
<Route 
  path="/price/:currency" 
  render={() => <Price setPrice={this.setPrice} />}
/>
```

We also have to pass our URL parameter into `<Price />`. This is where the arrow function comes in to play.  

```jsx
<Route 
  path="/price/:currency" 
  render={(routerProps) => <Price setPrice={this.setPrice} {...routerProps} />}
/>
```

> The `...` (spread operator) is allowing us to "destructure" the props object being passed by `Router` and apply each of its key/value pairs as props on the `Price` component.

Finally, we need to pass in the current component's state. We can also use the spread operator for that.


```jsx
<Route 
  path="/price/:currency" 
  render={(routerProps) => <Price setPrice={this.setPrice} {...routerProps} {...this.state} />}
/>
```

### Spread explained

The `...` syntax looks funny, so let's step aside and show you something that might be more familiar.

We know what our state object looks like, so we can use that as an example.

```js
this.state = {
  price: null
}
```

This turns into:

```js
<Price
  price={this.state.price}
/>
```

If we use the react dev tools, we can what props have been passed down from the `routerProps` object.

```js
let routerProps = {
  history: { /* stuff in here */ },
  location: { /* stuff in here */ },
  match: { /* stuff in here */ }
}
```

So if we spread the routerProps object, we'll get something like this:

```js
<Price
  history={ /* stuff in here */ }
  location={ /* stuff in here */ }
  match={ /* stuff in here */ }
>
```

Putting it all together, we turn this:

```jsx
<Price 
  setPrice={this.setPrice}
  {...routerProps}
  {...this.state} 
/>
```

Into this:

```jsx
<Price 
  setPrice={this.setPrice}
  history={}
  location={}
  match={}
  price={this.state.price}
/>
```

Super cool right?

![shia](https://media.giphy.com/media/ujUdrdpX7Ok5W/giphy.gif)

We still have some weird display quirks, and for that, we'll use `<Switch>` to fix them.

## Using Switch (10 min / 1:50)

Switch works just like the switch/case statements in javascript. We're comparing string values (in this case, routes) and executing conditions (rendering components) based on what matches turn out true. 

Since we're not using switch right now, we'll see something like this:

![preswitch](./images/pre-switch.png)

There are two components stacked on top of each other! That's silly.

> Ask class: Why does this happen?

There are two ways to handle this: using the Switch component, or specifying `exact` on routes.

Let's look at our routes in `App.js` again:

```jsx
<Route path="/" 
  component={Home}
/>
<Route path="/currencies" 
  component={Currencies}
/>
<Route 
  path="/price/:currency" 
  render={(routerProps) => <Price setPrice={this.setPrice} {...routerProps} {...this.state} /> }
/>
```

Try putting `exact` on the `/` path route component.

```js
<Route path="/" 
  exact
  component={Home}
/>
```

> Note: this is equivalent to putting `exact={true}`

Beautiful! this is a great solution, unless we have  many different routes.

If we had a list of routes like:

* `/currencies`
* `/currencies/new`
* `/currencies/:id`
 etc

we would have to put `exact` on `/currencies` or else, any time we went to `/currencies/something` it would match both the root (`/currencies`) AND the `/currencies/something` routes and both would be rendered.

We can avoid all this by just using `<Switch />`.

Back in `App.js`, let's import the `<Switch />` component and then wrap all of our routes in it.

```jsx
import { Route, Link, Switch } from 'react-router-dom' 

render() {
  return(
    <div>
      <nav>
        <Link to="/">
          <img src="https://en.bitcoin.it/w/images/en/2/29/BC_Logo_.png" alt=""/>
          <h1>Bitcoin prices</h1>
        </Link>
        <Link to="/currencies">Currency List</Link>
      </nav>
      <main>
        <Switch>
          <Route path="/" 
            component={Home}
          />
          <Route path="/currencies" 
            component={Currencies}
          />
          <Route 
            path="/price/:currency" 
            render={(routerProps) => <Price setPrice={this.setPrice} {...routerProps} {...this.state} /> }
          />
        </Switch>
      </main>
    </div>
  )
}
```

So easy!

![shia](https://media.giphy.com/media/ujUdrdpX7Ok5W/giphy.gif)

## Wrapping up (remainder of class)

Build something.
