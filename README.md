<div align="center">
  <img src="react-router-logo.png" />
  <h1 align="center">React Router</h1>
</div>

## Learning Objectives
- Import and use third-party node modules into React using npm (Node Package Manager)
- Use `BrowserRouter`, `Link`, and `Route` to allow for navigation and URL manipulation
- Use `axios` to query APIs for data


## Framing (10 min)

## We Do: Setup (5 min)
```
instructions
```

## You Do: Examine Current Codebase (10 min)
Take 10 minutes and read through the code to familiarize yourself with the codebase. Then, with a partner, take 5 minutes and discuss your answers these questions:
1. ...
2. ...


## An Aside: Axios (15 min)
Axios is a node module commonly used with React to send HTTP requests to an API. It functions much like jQuery's Ajax method. Some benefits to using Axios:
- It is a promise-based library which allows for a simpler and cleaner syntax
- It is lightweight and focused solely on handling HTTP requests (as opposed to jQuery which brings in an extensive set of new functions and methods)
- It is flexible and has a number of very useful methods for doing more complex requests from one or multiple API endpoints

[Axios Documentation](https://github.com/mzabriskie/axios)

> Note: Axios is just one of many Javascript libraries that we could use for handling requests. One of the big selling points of Node is the ability to mix and match technologies according to preference. Other commonly-used libraries for handling requests are Fetch and jQuery.

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
We will be using Axios to query the IBM Watson API in this exercise. Take 5 minutes to read and test out the Language Translator API at the link below.

[IBM Watson API Explorer]((https://watson-api-explorer.mybluemix.net/))


## I Do: React Router Setup (20 min)
Currently, we are rendering the response from Watson's Language Translator API service within the existing `SearchContainer` component. Let's bring in React Router and set up a separate component to display the results.

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


> - **Link** - a component for setting the URL and providing navigation between different components in our app without triggering a page refresh (similar to Angular ui-router's `ui-sref`). It takes a `to` property, which sets the URL to whatever path is defined within it. Link can also be used inside of any component that is connected to a `Route`.


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
> Notice that we are writing an anonymous callback function in the value for the `Route`'s render property. This callback function must `return` a react component.

> Another benefit of using a callback in the render property is that it preserves context, allowing us to pass down data and functions into `SearchContainer` in the same way as we have done previously.

## You do: Set up React Router and Add a Second Route (15 min)
- Using the above instructions as a guide, set up React Router in your own application.
- Once you have the setup described above, create a new component named `Results`, import it into `App.js`, and set up a `Link` and `Route` for it in `App's` render method.
- Have the `Results` component simply display the `translation` property in `App`'s state.
- Finally, remove the `translation` data from the `SearchContainer` component (as we are now rendering it in `Results`).

<details>
<summary><strong>Solution</strong></summary>


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

To import it in `App.js`:

```js
import Results from '../Results/Results.js'
```

To setup a `Link` and `Route` to it:

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

## Break (10 min)

## I do: Redirecting (20 min)

Currently, we have to manually click on `/results` after submitting a search to render the `Results` component and see the translation. Let's use React Router to force a redirect to `/results` once the search is finished. To do so, we need to import React Router's `Redirect` component.

### Importing the Redirect Component

```js
// In App.js

import {
  BrowserRouter as Router,
  Route,
  Link,
  Redirect
} from 'react-router-dom'
```

We only want to redirect **after** the user has searched and the data has come back from the API. Fortunately, we already have a method that fires when this happens (`handleSearchSubmit()`). Let's update the promise on `handleSearchSubmit()` to set a new property on `state` that we can use to see if a user has searched.

```js
// In App.js

handleSearchSubmit(e) {
  e.preventDefault()
  axios.get('https://watson-api-explorer.mybluemix.net/language-translator/api/v2/translate', {
    params: {
      source: this.state.sourceLang,
      target: this.state.targetLang,
      text: this.state.searchPhrase
    }
  }).then((response) => {
    this.setState({
      translation: response.data,
      hasSearched: true
    })
  })
}
```

Now whenever we get a response back from the API, `handleSearchSubmit` will both set the response to `translation` and set a `hasSearched` property to `true` on `state`.

Since React automatically calls a re-rendering (of its virtual DOM) of any child components of a component whose state has been updated, we know that the `Route` for SearchContainer will be re-rendered. With that in mind, we can change the `render` property on the `Route` for the `Search` component to check if a user `hasSearched`, and if so, force it to `Redirect` to `/results`.

```js
// In App.js (the App component's render method)

            <Route
              path="/search"
              render={() => {
                if(this.state.hasSearched) {
                  return <Redirect to="/results" />
                }
                return(
                  <SearchContainer
                    onSearchInput={(e) => this.handleSearchInput(e)}
                    langOptions={this.state.langOptions}
                    setSourceLang={(e) => this.setSourceLang(e)}
                    setTargetLang={(e) => this.setTargetLang(e)}
                    onSearchSubmit={(e) => this.handleSearchSubmit(e)}
                  />
                )
              }}
            />
```

What we are saying here is when this route is rendered, check `App`'s state for `hasSearched`. If it is true, render the `<Redirect />` component instead of the `<SearchContainer />` component. The `<Redirect />` component will then render whichever `Route` whose `path` matches its `to` property.

However, now we have another problem; once the `Redirect` takes us to the `Results` component, we are unable to go back to `/search` to submit a new search (since `hasSearched` is still `true` on `App`'s state). We need a way to switch that back to `false` once `Results` has been rendered.

### React Component Lifecycle Methods
React components come preloaded with [many very useful methods](https://facebook.github.io/react/docs/react-component.html) to allow us to execute code before, during, or after a given component's rendering. In this case, we would like to update App's state **after** the `Results` component has rendered. To do so, we can use the `componentDidMount()` method inside of the `Results` component to call a method to do just this:

> For a full list of React Component methods, read the [Documentation](https://facebook.github.io/react/docs/react-component.html)

```js
class Results extends Component {
  componentDidMount() {
    // Code to be executed once the Results component has finished rendering
  }
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

We need to update the parent ( `App` ) component's state when `Results` renders. To accomplish this, let's create a method in `App` that sets `hasSearched` to false and then pass it down to `Results` via props:

```js
// In App.js (inside of the App component)

clearSearch() {
  this.setState({
    hasSearched: false
  })
}


// And then pass it via props to the Results component in the render method

            <Route
              path="/results"
              render={() => {
                return(
                  <Results
                    translation={this.state.translation}
                    clearSearch={() => this.clearSearch()}
                  />
                )
              }}
            />
```

Lastly, call the `clearSearch()` method inside of `componentDidMount()`:

```js
class Results extends Component {
  componentDidMount() {
    this.props.clearSearch()
  }
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

Now we can navigate back and forth between `/results` and `/search` seamlessly.

## You do: Import and Configure the Redirect Component (20 min)
Using the instructions above as a guide, import `Redirect` from `react-router-dom` and set-up your own app to redirect to `Results` when a user submits a search.

## You do: Add Pronunciation to the Results Component (20 min)
Translating text into other languages is cool, but not that cool. Let's add some functionality to the Results component. Read the link below on IBM Watson's Text to Speech API and, using Axios to handle requests, update the Results component to:

[IBM Watson Text to Speech API](https://watson-api-explorer.mybluemix.net/apis/text-to-speech-v1#/)

- Show a drop-down of possible voices (nationalities) to select from using the API
- Display the phonetic pronunciation of the translated phrase based on the selected voice
- Create an HTML5 audio element that plays the selected voice speaking the translation aloud

> Hint: Give the component `state` and send off requests for any initially needed data when the `componentDidMount()`

<details>
<summary><strong>Solution</strong></summary>

```js
// In Results.js

import React, { Component } from 'react'
import axios from 'axios'

class Results extends Component {
  constructor(props) {
    super(props)
    this.state = {
      translation: this.props.translation,
      voiceOptions: [],
      selectedVoice: null,
      textPronunciation: null,
      audioPronunciationSource: null
    }
  }
  componentDidMount() {
    this.props.clearSearch()
    this.getVoiceOptions()
  }
  getVoiceOptions() {
    axios.get('https://watson-api-explorer.mybluemix.net/text-to-speech/api/v1/voices')
      .then((response) => {
        this.setState({
          voiceOptions: response.data.voices
        })
      })
  }
  setVoice(e) {
    this.setState({
      selectedVoice: e.target.value
    })
  }

  getTextPronunciation() {
    axios.get('https://watson-api-explorer.mybluemix.net/text-to-speech/api/v1/pronunciation', {
      params: {
        text: this.state.translation,
        voice: this.state.selectedVoice
      }
    })
    .then((response) => {
      this.setState({
        textPronunciation: response.data.pronunciation
      })
    })
    .catch((error) => {
      console.log(error)
    })
  }

  getAudioPronunciation() {
    this.setState({
      audioPronunciationSource: `https://watson-api-explorer.mybluemix.net/text-to-speech/api/v1/synthesize?text=${this.state.translation}&voice=${this.state.selectedVoice}`
    })
  }

  getPronunciations(e) {
    e.preventDefault()
    this.getTextPronunciation()
    this.getAudioPronunciation()
  }
  render() {
    let voices = this.state.voiceOptions.map((voice, index) => {
      return <option value={voice.name} key={index}>{voice.name}</option>
    })
    let audio =
      this.state.audioPronunciationSource?
      <audio controls>
        <source type="audio/ogg" src={this.state.audioPronunciationSource}/>
      </audio> :
      null

    return(
      <div>
        <h3>Translation: </h3>
        <p>{this.state.translation}</p>

        <h3>Get Pronunciation</h3>
        <form onSubmit={(e) => this.getPronunciations(e)} >
          <p>Choose Voice</p>
          <select onChange={(e) => this.setVoice(e)}>
            {voices}
          </select>
          <input type="submit" value="Select Voice"/>
        </form>
        <p>{this.state.textPronunciation}</p>
        <p>{audio}</p>
      </div>
    )
  }
}

export default Results

```

</details>


## Closing (5 min)
