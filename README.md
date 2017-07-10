# Introduction to React

## Overview
In this workshop, we will be learning the basics of the React JavaScript framework and building a sample project using the technologies learned.
* React encourages reusable, maintainable, and interactive front-end code.

## Learning Objectives
* Understand what makes React different from other JavaScript frameworks and JavaScript without a framwork.
* Install and setup React.
* Understand best practices for React setup.
* Introduce concept of state in React.
* Introduce concept of interactivity in React.
* Work on building a sample status widget.

## Introduction to React
React is a web framework developed by Facebook in order to make elements of user interfaces more modular and easier to maintain. According to React's website it is used to "Build encapsulated components that manage their own state, then compose them to make complex UIs."

#### Example: Facebook Reactions
* Imagine you worked at Facebook when they wanted to shift from using likes to reactions.
* Look at your Facebook and see how many different places reactions are used.
* Using traditional JavaScript, HTML, and CSS would it be easy and to implement these reactions in multiple places? Would there be repeated code between the different reaction components?
* React makes it easy to utilize UI components in multiple locations because instead of separating your HTML and JavaScript code, it uses JavaScript in order to write your HTML code.

#### Exercise: TODO MVC
* With the other people at your table, look at the code on the [TodoMVC](http://todomvc.com/) website. This site shows the same to do list application written in a bunch of different front end frameworks. 
* For this exercise, look at the [VanillaJS](http://todomvc.com/examples/vanillajs/) and [React](http://todomvc.com/examples/react/#/) examples on their site.
* Vanilla is what developers use to refer to JavaScript that doesn't make use of a framework.
* If you know another front-end framework like Angular, VueJS, or JQuery, it may be helpful to look at their implementations after this workshop.
* On the side of the page, there are links to Github source code for each version of the code. Go ahead and look through the files and see what differences there are between the implementations. 
* At the end, each table will point out what differences they found between the React and VanillaJS versions of TODO MVC.

#### Alternate Exercise: IMDBExplorer
* Same as above but with my IMDBExplorer app from my GA Code Sample.
* Pro: Much simpler code, similar to how I was envisioning the code for this workshop to look.
* Cons: Would rather not use my code, maybe not perfect implementation.

## Installing React

#### Our Set Up vs. Production Set Up
For this workshop, due to time constraints, we will be using CDN versions of React, Babel (a compiler that allows us to use next generation JavaScript features), Bootstrap's CSS, and FontAwesome Icons. CDNs allow us to use resources hosted on other sites within our code. In a production environment, however, it is inefficient to ping another website to retrieve an asset. A production-ready React application would most likely use a bundler, most commonly Webpack, to combine the React.js code and any other JavaScript libraries we use with our own code. Using Webpack (and a development server) would also allow us to split up our React code into multiple files. I usually have each React class that I build in a separate file. A good resource that goes further in depth on Webpack is [here](http://blog.andrewray.me/webpack-when-to-use-and-why/).

#### Project Set Up
* For this workshop, the only technical tool you need installed is a text editor and a web browser. If you do not have one installed, please go ahead and download [Sublime Text](https://www.sublimetext.com/) or [Atom](https://atom.io/).
* If you have git installed on your computer, clone this repository. If not, you can also click the download button on the GitHub site to download the starter code files.
* Open the `START.html` file on your computer in both your text editor and in the browser. 
* Also open the `REACT.html` file in another tab in your text editor.
* This file has HTML and CSS code that we will be translating to React code. If you have extra time, feel free to play around with the styling and layout.

# Project: Recreating the Facebook Status Widget in React
## React Components
If you open up the REACT.html file included in the repository you downloaded from GitHub, you will see a Hello World React component written in the file to start out.
```javascript
<div id="root"></div>
<script type="text/jsx">
    // Uses a JavaScript class that extends the React Component class
    class HelloWorld extends React.Component {
        render () {
            // Tells React what HTML code to render
            return (
                <div>
                    <h1>Hello World</h1>
                </div>
            )
        }
    }

    // Tells React to attach the HelloWorld component to the 'root' HTML div
    ReactDOM.render(
        <HelloWorld />,
        document.getElementById('root')
    )
</script>
```
Let's walk step by step through this code.

* The JavaScript code starts out with declaring a class called `HelloWorld` which is a subclass of the React.Component class, which we get from the React CDN we have included at the top of the HTML file. To read more about JavaScript classes, [this](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes) is a great resource.

* Within the `HelloWorld` class, we declare a render method which returns the HTML code that we want our component to display. In this case, that is a simple "Hello World" message, but you will see later on that what we render can be much more complex and interactive.

* Finally, we tell React which component we want to render as well as which HTML element we want to attach that component to. 

#### Exercise: Renaming and Replacing Components
* Rename the component to `Status` and make the component render the HTML code from the `START.html` file.

* CATCH: in React, we call the class attribute className since class is a keyword reserved for declaring a JavaScript class. Replace teh class attribute with className throughout your file.

TODO: I think it will be easier to have a new HTML file here instead.

### Breaking Components into Subcomponents
At the beginning of this workshop, we discussed how many different places the reaction functionality is used on Facebook. Similarly, many of the items on our Facebook status widget are reused in other places outside of statuses. This is where React shines. We can build reusable components that are used within multiple parent components -- so we could have a Photo component and a Status component that both have a share subcomponent. If, in the future, we wanted to change something about the share component, we would only have to change the code in one place.

#### Exercise: Identifying Subcomponents
* With the people at your table, discuss where we should break our app down into further subcomponents. Write your group's answers on the whiteboard table.

After this excercise, copy and paste the code from SUBCOMPONENTS.jsx into your script tags. There are multiple correct ways to break the component into subcomponents, it will just be easier for us to all be on the same page going forwards.

The syntax to include one React component within another is very similar to the way we call regular HTML tags -- for a HelloWorld component we would write <HelloWorld /> where we want it within the render method of the parent component. We can even use multiple instances of a subcomponent within a parent component.

## State and Props
Even though our HTML code is now more modular and reusable, it still just renders the exact same view that our original HTML file did. In this section of the workshop we will start the process of making our components interactive.

### Props
Imagine that we wanted our comment box to allow for a different number of letters in different places. On a status, for example, we want a user to be allowed to write a 200 letter long response. On a picture, however, we want them to only be able to write a 100 character response. React allows us to pass arguments from the Picture component and the Status component to specify how many letters we want to allow in our response, rather than having two different comment components.

#### Constructors
In order to utilize the property functionality in React, we must declare a constructor on our classes. A constructor is a special method that is called automatically when we instantiate our component and declares properties that can be used within it. The constructor will take the props argument and then call `super(props)` which will then call the constructor of the React.Component class with the props passed to the instance of our component. If you don't get this, don't worry about it for now -- [this](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes/constructor) is a good resource for learning more about constructors and super in JavaScript.

```javascript
class Comment extends React.Component {
    constructor (props) {
        super(props)
    }
    ...
}
```

#### Props Syntax
Passing props to components follows the following syntax:
```javascript
<Comment maxLetters={20} />
<Comment text='hello world' />
<Comment show={false} />

var test = 'hello world'
<Comment text={test} />
```
Text is passed within quotation marks. Variables, numbers, and booleans are passed within single brackets.

In order to show the value of the property within our component, we can use a similar expression.
```javascript
<small>{ this.props.maxLetters } Remaining</small>
```
Now lets add this to our status code.

#### Exercise: Extending LikeIcon Code
Add a constructor to the LikeIcon. For now, pass liked as true from Like to the LikeIcon component.

### State
Looking at the finished code, the like button and the the comment box are the two elements that we want to be interactive.

The state of the component is any data that will change within it. For the like button, the data that changes is whether or not the status has been liked, so we want to store whether or not the status has been liked within the state of the component.

In order to store that data, we will add a constructor to our JavaScript class and add the state declaration within it. It is just a normal JavaScript object. 
```javascript
class Like extends React.Component {
    constructor (props) {
        super(props)
        this.state = {
            liked: false
        }
    }
```
We can refer to the state of our component similarly to the props.
```javascript
{ this.state.liked }
```

#### Excercise: identifying and implementing state in the Comment component
What do you think changes within the comment component? Add the state and property to its constructor and then pass the liked state to the LikeIcon component.

## Event Handlers
Now that we have identified which elements of our components are interactive, and which parts of the state will change, lets start working on actually making them react to user interaction. In regular HTML you may have used  `onclick` listeners in your code that execute a JavaScript function when the user clicks on a button. This is very similar to how we handle interaction in React.

In React, the event handlers share the same names with the HTML ones, they are just camelCase instead of all lower case. So, onclick is onClick and onchange is onChange.

Let's first start out with handling the user input on the comment box. We can write a function that writes a message when the textarea changes.
```javascript
handleChange (event) {
    console.log('hello world')
}
...
<textarea onChange={ this.handleChange }>
```
React will automatically pass the data from the event to the method. If you are interested in what data is passed, you can console.log(event) instead of 'hello world'.

Usually when we have an event listener, we will want to update the state of the component based on that event. In this case we want to update the text of the component.

First, we need to make sure that the keyword `this` is accessible within our method. We can add `this.handleChange = this.handleChange.bind(this)` to the constructor in order to do this automatically.

In order to change the state of the component, we can use the setState method, which looks like this: 
```javascript
this.setState({
    text: event.target.value
})
```

We can then make the letters remaining count dynamic.
```javascript
 <small>{ this.props.maxLetters - this.state.text.length } Remaining</small>
```

#### Exercise: Adding an Event Listener to the Like Button
Add an event listener to the Like Button to update the state of the component. 

## Conditionals
### Rendering
In the Like component, we want to render the LikeIcon only if the status is liked. This will look very similar to any other JavaScript if statement. In the render method, we will return one thing if the status is liked, and nothing if it is not.
```javascript
render () {
    if (this.props.liked) {
        return (
            <div>
                <span className="fa-stack fa-sm">
                    <i className="fa fa-circle fa-stack-2x blue-icon"></i>
                    <i className="fa fa-thumbs-up fa-stack-1x fa-inverse"></i>
                </span>
            </div>
        )
    } else {
      return null
    }
}
```
Catch: You must return something from the else statement (null does work), omitting the else will cause an error.

### ClassNames
We can also have different classNames on HTML elements depending on the props or state of the component. Here I used a terniary statement, but you could abstract this out to a method or use a state variable as the entire class name.
```javascript                                
<button type="button" 
        className={ "btn no-outline " + (this.state.liked ? "btn-outline-primary" : "btn-secondary") }
        onClick={ this.toggleLike }
>
```

### Styles
Similarly, we can conditionally change the style of an element. React does follow a different syntax than normal CSS, and some properties have slightly different names. You can read more [here](https://facebook.github.io/react/docs/dom-elements.html).
```javascript
<small style={{ color: this.state.text.length > this.props.maxLetters ? '#d9534f' : '#5cb85c' }}>{ this.props.maxLetters - this.state.text.length } Remaining</small>
```

## Next Steps
### Extending our Exercise:
#### Exercise: Changing Likes to Reactions
Try to implement many different styles of likes using React components.

#### Exercise: Create a Photo Component
Try to create a Photo Post Component in React. Re-use subcomponents from earlier in this workshop.

### Tutorials
* [React Documentation](https://facebook.github.io/react/tutorial/tutorial.html)
* [DevCoffee](https://www.youtube.com/watch?v=ZnRFerIP8aA)
* [Wes Bos Redux](https://www.youtube.com/watch?v=hmwBow1PUuo&list=PLu8EoSxDXHP5uyzEWxdlr9WQTJJIzr6jy)
