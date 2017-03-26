<div align="center">
  ![React Router](react-router-logo.png)
  <h1 align="center">React Router</h1>
</div>
## Learning Objectives
- Import and use third-party node modules into React using npm (Node Package Manager)
- Use `BrowserRouter`, `Link`, and `Route` to allow for navigation and URL manipulation
- Use axios to query APIs for data


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


## We Do: React Router Setup
Currently, we are rendering the response from Watson's Translator API service within the existing `Search` component. Let's bring in React Router and set up a separate view to display the response.

First, we need to install `react-router-dom` and save it as a dependency to `package.json`:
```
npm install --save react-router-dom
```
Then, let's create a new file `Router.js` to define a new root component for our application (the component under which all other components are nested). In `Router.js`, to bring in React Router and its related components:
```js
import React, { Component } from 'react'
import {
  BrowserRouter as Router,
  Route,
  Link
} from 'react-router-dom'
```
