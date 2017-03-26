<div align="center">
  <img src="react-router-logo.png" />
  <h1 align="center">React Router</h1>
</div>

## Learning Objectives
- Import and use third-party node modules into React using npm (Node Package Manager)
- Use `BrowserRouter`, `Link`, and `Route` to allow for navigation and URL manipulation
- Use `axios` to query APIs for data


## Framing

## We Do: Setup
```
instructions
```

## You Do: Examine Current Codebase
Read through the code to familiarize yourself with the codebase. Then, with a partner, try to answer these questions:
1. ...
2. ...


## An Aside: Axios
Axios is a node module commonly used with React to send HTTP requests to an API. It functions much like jQuery's Ajax method. Some benefits to using Axios:
- It is a promise-based library which allows for a simpler and cleaner syntax
- It is lightweight and focused solely on handling HTTP requests (as opposed to jQuery which brings in an extensive set of new functions and methods)
- It is flexible and has a number of very useful methods for doing more complex requests from one or multiple API endpoints

[Axios Documentation](https://github.com/mzabriskie/axios)

To load in the Axios module:
```js
// If you are using Babel to compile your code
import axios from 'axios'

// In standard vanilla Javascript
let axios = require('axios')
```

To use Axios to query an API at a given url endpoint:
```js
  axios.get('url')
    .then((response) => {
      console.log(response)
    })
    .catch((error) => {
      console.log(error)
    })
```

You can also append values to the parameters by passing in a second input to `.get()`:
```js
  axios.get('url', {
    params: {
      key1: value1,
      key2: value2
    }
  })
  .then((response) => {
    console.log(response)
  })
  .catch((error) => {
    console.log(error)
  })
```
Which would result in a GET request to: `url?key1=value1&key2=value2`
We will be using Axios to query the [IMB Watson API](https://watson-api-explorer.mybluemix.net/) in this exercise.


## I Do: React Router Setup
Currently, we are rendering the response from Watson's Translator API service within the existing `SearchContainer` component. Let's bring in React Router and set up a separate component to display the results.

### Importing Dependencies

First, we need to install `react-router-dom` and save it as a dependency to `package.json`:
```
npm install --save react-router-dom
```

Then, in `App.js`, we need to import all of the components we want to use from React Router:
```js
// App.js

import {
  BrowserRouter as Router,
  Route,
  Link,
} from 'react-router-dom'
```
> Note that we are aliasing `BrowserRouter` as `Router` here for simplicity

### Modifying App's render method
Now that we have access to these components, we need to modify the `App` component's `render()` method to set up navigation. The basic structure we will use is:
```js
// App.js

render() {
  return (
    <Router>
      <div>
        <nav>
          <Link to=""></Link>
          <Link to=""></Link>          
        </nav>
        <main>
          <Route path="" render={}/>
          <Route path="" render={}/>
        </main>
      </div>
    </Router>
  )
}
```
> - **Router** - the root component inside of which all `Link`'s and `Route`'s must be nested. It can only have **one** direct child element (thus the need for the enclosing `div` tag around `nav` and `main`)


> - **Link** - a component for setting the URL and providing navigation between different components in our app without triggering a page refresh (similar to Angular ui-router's `ui-sref`). It takes one property, `to`, which sets the URL to whatever path is defined within it.


> - **Route** - a component that connects a certain `path` in the URL with the relevant component to `render` at that location (similar to Angular ui-router's `ui-view` or erb's `yield`)

Now let's customize this structure to provide a link to our current component `SearchContainer`:
```js
render() {
  return (
    <Router>
      <div>
        <nav>
          <Link to="/search"></Link>   
        </nav>
        <main>
          <Route
            path="/search"
            render={() => {
              return(
                <SearchContainer
                  onSearchInput={(e) => this.handleSearchInput(e)}
                  langOptions={this.state.langOptions}
                  setSourceLang={(e) => this.setSourceLang(e)}
                  setTargetLang={(e) => this.setTargetLang(e)}
                  onSearchSubmit={(e) => this.handleSearchSubmit(e)}
                  translation={this.state.translation}
                />
              )
            }}
          />
        </main>
      </div>
    </Router>
  )
}
```
> Notice that we are writing an anonymous callback function in the value for the `Route`'s render prop. This callback function must `return` a react component.

> Another benefit of using a callback in the render prop is that it preserves context, allowing us to pass down data and functions into `SearchContainer` in the same way as we have done previously.

## You do: Set up React Router and Add a Second Route
Using the above instructions as a guide, set up React Router in your own application. Once you have the setup described above, create a new component named `Results`, import it into `App.js`, and set up a `Link` and `Route` for it in `App's` render method. Have the `Results` component simply display the `translation` property in `App`'s state.

<details>
<summary><strong>Solution</strong></summary>
<br>
To set up a new `Results` component:
```js
import React, { Component } from 'react'

class Results extends Component {
  render() {
    return(
      <div>
        <h3>Translation: </h3>
        <p>{this.props.translation}</p>
      </div>
    )
  }
}

export default Results
```
<br />

To import and use it in `App.js`:
```js
import Results from '../Results/Results.js'
```
<br />
```js
render() {
  return(
    <Router>
      <div>
        <nav>
          <Link to="/search">Search</Link>
          <Link to="/results">Results</Link>
        </nav>
        <main>
          <Route
            path="/search"
            render={() => {
              return(
                <SearchContainer
                  onSearchInput={(e) => this.handleSearchInput(e)}
                  langOptions={this.state.langOptions}
                  setSourceLang={(e) => this.setSourceLang(e)}
                  setTargetLang={(e) => this.setTargetLang(e)}
                  onSearchSubmit={(e) => this.handleSearchSubmit(e)}
                  translation={this.state.translation}
                />
              )
            }}
          />
          <Route
            path="/results"
            render={() => {
              return(
                <Results translation={this.state.translation}/>
              )
            }}
          />
        </main>
      </div>
    </Router>
  )
}

```

</details>
