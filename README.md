# React Router

![React Router Logo](./react-router-logo.png)

## Learning Objectives

* Review importing and using third-party node modules into React using npm
* Use React Router's `BrowserRouter`, `Link`, `Route`, `Redirect`, and `Switch` components to add navigation to a React application
* Review the React component lifecycle and use component methods to time API calls
* Use `axios` to query APIs for data

## Framing (5 min / 0:05)

Up to this point, our React applications have been limited in size, allowing us to use basic control flow in our components' render methods to determine what gets rendered to our users. However, as our React applications grow in size and scope, we need an easier and more robust way of rendering different components. Additionally, we will want the ability to set information in the url parameters to make it easier for users to identify where they are in the application.

React Router, while not the only, is the most commonly-used routing library for React. It is relatively straightforward to configure and integrates with the component architecture nicely (itself being nothing but a collection of components). Once configured, it essentially serves as the root component in a React application and renders other application components within itself depending on the path in the url.

## We Do: [React Translator](https://git.generalassemb.ly/ga-wdi-exercises/react-translator) Setup (5 min / 0:10)

As we learn about React Router, we'll be building out an app that uses the [IBM Watson translation API](https://watson-api-explorer.mybluemix.net/apis/language-translator-v2).

You can find the starter code for this exercise in [this repository](https://git.generalassemb.ly/ga-wdi-exercises/react-translator).

```sh
git clone git@git.generalassemb.ly:ga-wdi-exercises/react-translator.git
cd react-translator
npm install
npm run start
```

## You Do: Axios & Watson API (5 min / 0:15)

We will be using [Axios](https://github.com/axios/axios) to query the IBM Watson API in this exercise. Take 5 minutes to read and test out the Language Translator API at the link below.

[General IBM Watson API Explorer](https://watson-api-explorer.mybluemix.net/)

## You Do: Examine Current Codebase (15 min / 0:30)

Take 10 minutes and read through the code to familiarize yourself with the codebase with a partner or in groups of 3. Prepare to discuss your answers the following questions:

1. What dependencies is the application currently using? Where can I find information on them?
2. What is the purpose of `ReactDOM.render()`? What file is this method being called in?
3. Where are the components of our application located? Why might we want to separate them into their own folders?
4. Where is state located in our application? How is state being passed down to other components?
5. Is data flowing up from child components to parent components anywhere in our application? How is this happening?
6. Where is our application getting data from? How is it accomplishing this?

## I Do: React Router Setup (10 min / 0:40)

> I'm going to work through some initial set up. Just follow along for now, you'll get a chance to work through this on your own in a few minutes using what I do now as a guide.

Currently, we are rendering the response from Watson's Language Translator API service within the existing `Search` component. Let's bring in React Router and set up a separate component to display the results.

### Importing Dependencies

First, we need to install `react-router-dom` and save it as a dependency to `package.json`...

```sh
npm install --save react-router-dom
```

To configure our current application to use React Router, we need to modify the root rendering of our app in `index.js`. We need to bring in the `Router` component and allow it to be the root component of our application. `Router` will, in turn, render `App` through which all the rest of our components will be rendered:

```js
// index.js

import { BrowserRouter as Router } from "react-router-dom";

// ...

ReactDOM.render(
  <Router>
    <App />
  </Router>,
  document.getElementById("root")
);
```

> Note that we are aliasing `BrowserRouter` as `Router` here for simplicity

By making `Router` the root component of our app, all child components, including `App` will have access to a `history` object through which information like the current location and url can be accessed or changed. Additionally, in order to use the other routing components provided by React Router, a `Router` ancestor component is necessary.

Next, in `App.js`, we need to import all of the other components we want to use from React Router...

```js
// src/components/App/App.js

import { Route, Link } from "react-router-dom";
```

### Modifying App's render method

Now that we have access to these components, we need to modify the `App` component's `render()` method to set up navigation. The basic structure we will use is...

```js
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
        <Route path="" render={}/>
      </main>
    </div>
  )
}
```

> **Link** - a component for setting the URL and providing navigation between different components in our app without triggering a page refresh. It takes a `to` property, which sets the URL to whatever path is defined within it. Link can also be used inside of any component that is connected to a `Route`.

> **Route** - a component that connects a certain `path` in the URL with the relevant component to `render` at that location (similar to `body` from handlebars).

Now let's customize `App`'s render method to provide a link to `Search`...

```js
// src/components/App/App.js

render() {
  return(
    <div>
      <nav>
        <h1>React Translator</h1>
        <Link to="/search">Search</Link>
      </nav>
      <main>
        <Route
          path='/search'
          render={() => (
            <Search
              translation={ this.state.translation }
              setTranslation={ (data) => this.setTranslation(data) }
            />
          )}
        />
      </main>
    </div>
  )
}
```

> Notice that we are writing an anonymous callback function in the value for the `Route`'s render property. This callback function **must return a React component**.

> So long as we use an ES6 arrow function, this callback will preserve context (i.e. the value of `this`), allowing us to pass down data and functions into `Search` from `App`. You can use an ES5 anonymous function, but you will then need to use `.bind()` to preserve context.

## You do: Set up React Router and Add a Second Route (15 min / 0:55)

> 10 minute exercise / 5 minute review

* Using the above instructions as a guide, set up React Router in your own application.
* Once you have the setup described above, create a new component named `Results`, import it into `App.js`, and set up a `Link` and `Route` for it in `App`'s render method.
* Have the `Results` component display the `translation` property in `App`'s state by passing it to `Results` via props.
* Finally, remove the `translation` data from `Search` render method (as we are now rendering it in `Results`).

<details>
<summary><strong>Solution</strong></summary>

To set up a new `Results` component...

```js
// src/components/Results/Results.js

import React, { Component } from "react";

class Results extends Component {
  render() {
    return (
      <div>
        <h3>Translation: </h3>
        <p>{this.props.translation}</p>
      </div>
    );
  }
}

export default Results;
```

To import it in `App.js`...

```js
// src/components/App/App.js

import Results from "../Results/Results.js";
```

To setup a `Link` and matching `Route`...

```js
// src/components/App/App.js

render () {
  return(
      <div>
        <nav>
          <h1>React Translator</h1>
          <Link to="/search">Search</Link>
          <Link to="/results">Results</Link>
        </nav>
        <main>
          <Route
            path='/search'
            render={() => (
              <Search
                translation={ this.state.translation }
                setTranslation={ (data) => this.setTranslation(data) }
              />
            )}
          />
          <Route
            path='/results'
            render={() => (
              <Results
                translation={ this.state.translation }
              />
            )}
          />
        </main>
      </div>
    </Router>
  )
}
```

</details>

## I Do: Redirecting (15 min / 1:10)

Currently, we have to manually click on `/results` after submitting a search to render the `Results` component and see the translation. Let's use React Router to force a redirect to `/results` once the search is finished. To do so, we need to use the `history` API that is included as a dependency of React Router.

The `history` object is provided automatically via props from `Router`. To expose and use it, all we need to do is to modify the `render` prop on any `Route` whose rendered component will need access to it. In our case, this will be the `Search` component:

```js
// src/components/App/App.js

<Route
  path="/search"
  render={props => (
    <Search {...props} setTranslation={data => this.setTranslation(data)} />
  )}
/>
```

> The `...` (spread operator) is allowing us to "destructure" the props object being passed by `Router` and apply each of its properties as props on the `Search` component.

Now that we have passed the `history` object down to `Search` via props, we can use it to programmatically set the url path from within `Search` thereby causing `Results` to be rendered:

```js
// src/components/Search/Search.js

handleSearchSubmit(e) {
  e.preventDefault()
  axios.get('https://watson-api-explorer.mybluemix.net/language-translator/api/v2/translate', {
    params: {
      source: this.state.sourceLang,
      target: this.state.targetLang,
      text: this.state.searchPhrase
    }
  })
  .then((response) => {
    this.props.setTranslation(response.data.translations[0].translation)
    this.props.history.push('/results')
  })
  .catch((err) => {
    console.log(err)
  })
}
```

> Note that we are calling `this.prop.history.push()` within `.then()` so that we don't redirect before the response has come back from the Watson API.

## You Do: Expose and Use the History Object to Redirect (20 min / 1:30)

> 15 min exercise / 5 min review

Using the instructions above as a guide, expose and pass the `history` object to `Search` and set-up your own app to redirect to `Results` when a user submits a search.

## Break (10 min / 1:40)

## We Do: Using the Redirect Component (15 min / 1:55)

Another edge case we want to control for is when the user submits a request at a url that we have not set up a `Route` for. To handle this, React Router provides a `Redirect` component that when returned, tells the browser to change the url to that of one of our recognized routes:

### Importing the Redirect Component

To import the `Redirect` component, update your `import` statement in `App.js` as such:

```js
// src/components/App/App.js

import { Route, Link, Redirect } from "react-router-dom";
```

Then set up a catch-all `Route` in `App`'s render method that will render it. Make sure to add it **below** the other `Route` definitions:

```js
// src/components/App/App.js

<Route path="/*" render={() => <Redirect to="/search" />} />
```

Once we refresh the page, we will see an error in the console saying that we are trying to redirect to the same route that we are currently on. This is because, by default, React Router is checking **ALL** of our routes for matches with the url and then rendering **ANY** matching `Route` components independently. To force React Router to treat our routes as unique and **ONLY** render the first matching route, we need to use the `Switch` component:

First, import it from `react-router-dom`:

```js
// src/components/App/App.js

import { Route, Link, Redirect, Switch } from "react-router-dom";
```

Then, wrap **ALL** of our `Route` components within a `Switch` component:

```js
// src/components/App/App.js

<Switch>
  <Route
    path="/search"
    render={props => (
      <Search {...props} setTranslation={data => this.setTranslation(data)} />
    )}
  />
  <Route
    path="/results"
    render={props => (
      <Results {...props} translation={this.state.translation} />
    )}
  />
  <Route path="/*" render={() => <Redirect to="/search" />} />
</Switch>
```

Now, when we submit a random url, React Router successfully redirects us, but not when we submit a recognized url.

## I Do: Sub-Routes (15 minutes / 2:10)

At any one of our current routes, we may want to add sub-routes to access auxiliary views. The way we do this in React is to simply repeat the pattern we used in `App` (using `Link` and `Route`) inside any component that will have child routes. For example, if inside the `Results` component, we wanted to have conditional rendering of a `Share` component based on the url, we could set it up like so:

```js
// src/components/Share/Share.js

import React, { Component } from "react";

class Share extends Component {
  render() {
    return (
      <div>
        <h2>Share on Facebook</h2>
        <button>Share</button>
      </div>
    );
  }
}

export default Share;
```

Then to set it up in `Results`:

```js
import Share from '../Share/Share'
import {
  Link,
  Route
} from 'react-router-dom'

// ...

  render () {
    return (
      <div>
        <h1>Translation:</h1>
        <p>{ this.props.translation }</p>
        <Link to={`${this.props.match.url}/share`}>Share</Link>
        <Route
          path={`${this.props.match.url}/share`}
          component={Share}
        />
      </div>
    )
  }
```

## You Do: Set Up a Sub-Route in Results (10 minutes / 2:20)

Now it's your turn. Set up a sub-route of your own in `Results` using the pattern described above. It can be for a `Share` sub-route or any other view.

## Closing / Questions (10 min)

## Bonus: Redirect to `Search` from `Results` if `translation` is `null`

Currently, if the user does a hard refresh while at `/results`, the component will simply render a blank translation. How could we use the `history` object to tell it to redirect to `/search` (similar to how we did above) if the `translation` prop is `null`?

> Hint: Look into React's component lifecycle hook [componentDidMount](https://facebook.github.io/react/docs/react-component.html#componentdidmount)

<details>
<summary><strong>Solution</strong></summary>

First, we need to pass `history` into `Results` just like we did with `Search`:

```js
// src/components/App/App.js

<Route
  path="/results"
  render={props => <Results {...props} translation={this.state.translation} />}
/>
```

Then, within `Results`, we can use `componentDidMount` to check the translation prop once the component has initialized. If it is `null`, we can use `history` to redirect to `/search`:

```js
// src/components/Results/Results.js

componentDidMount () {
  if (!this.props.translation) {
    this.props.history.push('/search')
  }
}
```

</details>

## Bonus: Add Pronunciation Fetching to the Results Component

Wouldn't it be cool if, in addition to showing the text translation, our app could also provide an audio translation with the words being pronounced? Fortunately, the Watson API also provides a [Text-to-Speech](https://watson-api-explorer.mybluemix.net/apis/text-to-speech-v1) service for this purpose. By combining this with the react lifecycle method we saw earlier, we can have the `Results` component automatically fetch this audio for us and then render it.

In order to use get audio from the `/v1/synthesize` endpoint of the Text-to-Speech service, we must provide two pieces of information in the request url:

* a `voice` param whose value corresponds to a chosen voice from the API's list of voices
* a `text` param whose value corresponds to the translation we want pronounced

To provide a selected voice in the request, we will first need to retrieve the list of possible voices from the API's `/v1/voices` endpoint. First, let's import `axios` in `Results` and then let's send the request from `Results`'s `componentDidMount` method:

```js
// src/components/Results/Result.js

import axios from "axios";
```

```js
// src/components/Results/Results.js

componentDidMount () {
  if (!this.props.translation) {
    this.props.history.push('/search')
  }
  else {
    axios.get('https://watson-api-explorer.mybluemix.net/text-to-speech/api/v1/voices')
      .then((res) => {
        console.log(res)
      })
      .catch((err) => {
        console.log(err)
      })
  }
}
```

> It is typically best practice to send any initial API requests for data using `componentDidMount`

If we inspect the API response in the console, we can see that the `voices` array is nested inside of `data` object and that each voice's `name` is prepended with the two letter abbreviation of the language it represents. In order to be able to select the voice that matches our translation, we will need `Results` to know what language the text was translated to. To achieve this, we will need `Search` to set a second property on `App`'s state reflecting the language that the translation is in. Let's think about how we might modify `App`'s `setTranslation` method to do this...

<details>
<summary><strong>Solution</strong></summary>

One of the easiest ways to do this would be to simply add a second argument to `setTranslation` representing the language of the translation:

```js
// src/components/App/App.js

setTranslation (data, language) {
  this.setState({
    translation: data,
    language: language
  })
}
```

We also will need to add this second argument to the callback we pass to `Search`:

```js
// src/components/App/App.js

<Route
  path="/search"
  render={props => {
    return (
      <Search
        {...props}
        setTranslation={(data, language) => this.setTranslation(data, language)}
      />
    );
  }}
/>
```

Then we can update `Search`'s `handleSearchSubmit` method to provide this second argument to `this.props.setTranslation`:

```js
// Search/Search.js

handleSearchSubmit(e) {
  e.preventDefault()
  axios.get('https://watson-api-explorer.mybluemix.net/language-translator/api/v2/translate', {
    params: {
      source: this.state.sourceLang,
      target: this.state.targetLang,
      text: this.state.searchPhrase
    }
  })
  .then((response) => {
    console.log(response)
    this.props.setTranslation(response.data.translations[0].translation, this.state.targetLang)
    this.props.history.push('/results')
  })
  .catch((err) => {
    console.log(err)
  })
}
```

</details>

<hr />

Now that the we have a `language` property on the `App`'s state, let's pass it via props into `Results` so we can use it to filter the array of voices we are retrieving:

```js
// src/components/App/App.js

<Route
  path="/results"
  render={props => {
    return (
      <Results
        {...props}
        translation={this.state.translation}
        language={this.state.language}
      />
    );
  }}
/>
```

Then let's update `Results`'s `componentDidMount` method to use this information to find the voice that relates to the language we chose:

```js
// src/components/results/Results.js

constructor () {
  super()
  this.state = {
    voiceAudioSource: null
  }
}

componentDidMount () {
  if (!this.props.translation) {
    this.props.history.push('/search')
  }
  else {
    axios.get('https://watson-api-explorer.mybluemix.net/text-to-speech/api/v1/voices')
      .then((res) => {
        let selectedVoice = res.data.voices.find((voice) => voice.name.includes(this.props.language))
        this.setState({
          voiceAudioSource: `https://watson-api-explorer.mybluemix.net/text-to-speech/api/v1/synthesize?text=${this.props.translation}&voice=${selectedVoice.name}`
        })
      })
      .catch((err) => {
        console.log(err)
      })
  }
}
```

> Note that we must add a `constructor` method in order to use `state` on `Results`

Finally, let's update `Results`'s `render` method to create an `audio` element with a `src` equal to the `voiceAudioSource` set on `state`:

```js
// src/components/Results/Results.js

render () {
  let audio =
    this.state.voiceAudioSource?
    <audio controls>
      <source type="audio/ogg" src={this.state.voiceAudioSource}/>
    </audio> :
    null

  return (
    <div>
      <h3>Translation</h3>
      <p>{ this.props.translation }</p>
      { audio }
    </div>
  )
}
```

Now when we submit a search, it automatically loads in the audio pronunciation along with the translation!
