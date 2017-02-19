---
title: 'React: Create a simple app'
date: 2017-02-12 17:04:33
tags: React 
desc: Simple React App
---

Learn the basics of react by creating a simple app with just using react and not worry about anything else.

**[Demo](http://madsid.com/pages/chucknorris/)**  || **[Code](https://github.com/madsid/myReact/tree/chucknorris)** 

## You Will Learn
- React Components ( Container and presentation)
- Props and State
- React Component lifecycle
- React inline styles

## [React Components](https://facebook.github.io/react/docs/components-and-props.html): 

React lets you split the UI into components which is isolated and reusable. For Example if you are creating a movie site and need a rating bar. You can create a component for that and use it as many times as needed.  

Basically components are divided into two types:  
- Container component 
- Presentation component  

### __Container components:__ 
These are the high level components which coordinates with the  application to provide data to other container or presentation components. You can think of them as data sources.  

### __Presentation components:__ 
These are low level components which interact with the container component to get the data passed via props (more about it later) and all they worry about is about presenting the data. They do not worry about data origin or anything to do with it.  

### **Props:** 
Props is a javascript object which is passed on to the child components. This is mostly used by container components to pass the information to the presentation components.  

### **State:** 
State is also a javascript object but stores the application information and can be altered as the application changes. For Example, storing name of the user or storing data received via API calls. Remember to not change the state object directly.   

### **[Component Lifecycle:](https://facebook.github.io/react/docs/react-component.html)**
React provides super awesome functions for each components to use for several purposes and are called for each event that occurs in the life cycle of a component. Some of them are  

#### **ComponentWillMount():** 
This function is called when the component is going to mount to the page.  

#### **ComponentDidMount():**
This function is called when the component is mounted to the page. This is when we fetch the data (Jokes in this case) and render them.  

### **Inline Styles:** 
This is another feature I love in react. Each component can have it's own Inline styles. This is a controversial topic since it will overwrite the global styles by default, but it's a cool feature.  

Create-react-app is an awesome boilerplate for starters to learn just react. This tutorial is built using on top of this.   

Open the console/terminal and type the below commands. (you need to have node js installed on your machine)  

```bash
npm install -g create-react-app 
create-react-app my-app 
cd my-app/ 
npm start 
```

This should start a server with simple welcome page.  

Alright, Now that we are done with introduction. Let's get to building stuff. Fire up your favorite code editor and follow along the tutorial code.   

Note: If you do not want to mess around with inline styles, you can copy them from the source code.  

If you would like to run the tutorial code locally. 

```bash 
git clone  -b chucknorris --single-branch https://github.com/madsid/myReact.git <name> 
cd <name>/ 
npm install  
npm start  
```

Assuming you looked at the demo page. Our first task is to identify the components. I imagine two components.  

1. **Joke block** is a container component contains all the jokes and passes to other component to render it  

2. **joke** is a presentation component, gets the joke via props and will render it. 

Nanoajax is a tiny library which allows us to make ajax calls without getting too much into the details.  

```jsx
import React, { Component } from 'react';
import Joke from './Joke';
import nanoajax from 'nanoajax';

class JokeBlock extends Component {
  constructor(props) {
  super(props);
    this.state = {
      data: []
    };

    this.getData = this.getData.bind(this);
    this.handleClick = this.handleClick.bind(this);
  }

  getData(){
    nanoajax.ajax({url:"http://api.icndb.com/jokes/random/5"}, function(code, resp){
      if(code === 200){
        resp = JSON.parse(resp);
        this.setState({
            data : resp.value
        });
      }
    }.bind(this));
  }

  handleClick(){
    this.getData();
  }
  componentDidMount(){
    this.getData();
  }

  render() {
    var jokes = this.state.data.map(function(joke){
      return (
        <div key={joke.id}>
          <Joke data={joke.joke}/>
        </div>
      )
    });
    return (
      <div style={styles.main}>
        <p style={styles.reload} onClick={this.handleClick} > More &#x21bb;</p>
        <hr style={styles.hr} />
        <div> {jokes} </div> 
      </div>
    );
  };
}
```


Let's set the state in constructor to an empty array which is loaded later. As soon as the component mounts we make the call to get the jokes data.  

The most important thing to notice in the code it setState() to set the state when data is received. This let’s react know something is changed and this component has to be re-rendered.  

There is an onClick handler for the more link which calls the getData() function to fetch new joke dataset and updates the state.  

```jsx
import React, { Component } from 'react';

class Joke extends Component {
  render() {
    return (
      <div style={styles.main}>
        <div style={styles.blockquote}> 
          <p style={styles.joke}> {this.props.data} </p>
        </div>
        <hr style={styles.hr}/>
      </div>
    );
  }
}
```
It displays whatever data it receives from the props object. Which you can notice from this.props.data in the code.  

# What Next  

I hope you enjoyed this article as much as I did writing it. This is my first article so please excuse my mistakes.  

I recommend to checkout redux or flux for a full react application with efficient state management once you get familiar with react. Also ES6 is heavily used in react community so, check it out as well. 