# React Jeopardy Lab
In this lab, you will a simple Jeopardy game by consuming the [jService](http://jservice.io/) API. You are going to add this game as a new component to your application from the lecture in class today.

To get started you need to get a few things set up so we are able to communicate to the jService API.

## Setup

* Create a `jeopardyService.js` file in the **src** directory. This will be the service we use to make HTTP requests to the API.
```javascript

//import the axios HTTP client to communicate with the API
import axios from 'axios';

class JeopardyService {

    constructor(url = 'http://jservice.io/api/random', client = axios.create()){
        this.url = url;
        this.client = client;
    }

    getQuestion(){
        return this.client.get(this.url);
    }

}

export default JeopardyService;

```

* Since our service uses Axios we have to add it to our project as a dependency.
    * from the root directory of your project run `npm i axios --save` to install axios.

* Create a **Jeopardy** component in **src/components/jeopardy/** and import our JeopardyService.

**src/components/jeopardy/jeopardy.js**
```javascript
import React, { Component } from 'react';

//import our service
import JeopardyService from "../../jeopardyService";

class Jeopardy extends Component {

  //set our initial state and set up our service as this.client on this component
  constructor(props){
    super(props);
    this.client = new JeopardyService();
    this.state = {
      data: {},
      score: 0
    }
  }

  //get a new random question from the API and add it to the data object in state
  getNewQuestion() {
    return this.client.getQuestion().then(result => {
      this.setState({
        data: result.data[0]
      })
    })
  }

  //when the component mounts, get a the first question
  componentDidMount() {
    this.getNewQuestion();
  }

  //display the results on the screen
  render() {
    return (
      <div>
        {JSON.stringify(this.state.data)}
      </div>
    );
  }
}

export default Jeopardy;

```

## Normal Mode
Let's start building our game

* Update the navigation and router of your application to display the new component
* Display the question, category, and point value returned from the API
* Create a way to keep track of the user's score
* Provide a way for the user to submit an answer to the question
* If the answer is correct then add the point value of the question to the to the user's score. 
* If the answer is wrong, subtract the point value from the user's score.
* After the user answers a question display another random question from the API.

## Medium Mode 

* Create a stateless display component for the Jeopardy game that handles the display of the game
* Update the Jeopardy component to only render the new display component
* Pass the needed props from the Jeopardy component to the display component so it has all the data it needs to display the game data and submit the user's answer

* The other should all the display of the data through passed in props. 

## Hard Mode
* Instead of displaying a single random question, display 3 categories. 
* Once a user selects a category, display the question for the category selected
* The rest of the application should work the same