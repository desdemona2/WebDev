# React

React is a frontend framework which can render small components of website rather than refreshing the whole website. Large number of websites are built on react framework including websites like twitter, facebook etc. 

It will offer reusable components and live changes.



## JSX

JSX is an important component of React. It will help pass html from javascript file to the html file without any need to modify html file. JSX is like html but javascript variables can be used within it using `{}`.

```javascript
const React = require('react');
const ReactDOM = require('react-dom');

function callback() {
    document.querySelector('#root').style.color = "blue";
}

// arg1: What to print, arg2: What to print, callback when render is completed
ReactDom.render(<h1>Hello World!<h1>, document.querySelector('#root'), callback);
```

render function in ReactDom render's whatever is passed to it on a given selector. After page is rendered it will make a callback as defined in above code.

Behind the scenes this code is compiled into plain javascript by a compiler called [babel](https://babeljs.io/). One can check [here](https://babeljs.io/repl) how a given code is compiled into javascript code from html elements. Babel has the ability to convert latest version code to a browser compatible code like from es6 to previous gen code. So we can use import statements instead of require like

```javascript
import React from "react";
import ReactDOM from "react-dom";
```



The same code if were written in vanilla javascript it would be like this:

```javascript
const h1 = document.h1;
h1.innerHTML = "Hello World!"

document.querySelector("#root").appendChild(h1);
document.querySelector("#root").style.color = 'blue';
```

React took a single line to do what native javascript took 4 lines to do.

**Remember render method only takes a single html element, passing more than one will crash the website**. One solution to that is wrap the different html elements in a div or simply empty angle brackets like this:

```jsx
ReactDOM.render(
  <>
    <h1>Hello world!</h1>
    <p>This is paragraph</p>
  </>,
  document.querySelector("#root"),
  callback
);
```



**We can use javascript within html tags defined using jsx**. If a variable in html is wrapped inside curly braces it will be treated as javascript variable `{}`. Like below:

```jsx
const string = "This is my Heading";

ReactDOM.render(
  <>
    <h1>{string}</h1>
    <p>This is paragraph</p>
  </>,
  document.querySelector("#root"),
  callback
);
```

Here `{string}` will be automatically interpreted as This is my Heading. One can also have a code like `{String.uppercase(string)}` or `{31+Math.random()}`. 

**STATEMENTS ARE NOT ALLOWED INSIDE JSX {} SO IF-ELSE, FOR OR WHILE LOOP WON'T WORK**. 



### JSX Attribute and Styling

When putting html in javascript file there can be problems when both have same reserved keywords. In this case class is a keyword in both html and javascript. So when we are using html inside javascript using class attribute wihin html may cause problems. So **instead of using class jsx uses `className`** to set a class for a particular element. Same goes for keyword **'for'**. for in html forum is used with label which represent the id of the element which the label is representing but **'for'** is again a reserved keyword in javascript. So **instead of using for in html jsx uses forHtml instead of for**.

While importing jsx files in html instead of using `<script src="./script/index.js" type="text/javascript"` we should use `<script src="./script/index.js" type="text/JSX"`. Now browser will know that file javascript file contains JSX code.

```jsx
ReactDOM.render(
	<>
    <h1 className="heading">This is a heading</h1>
    </>,
    document.querySelector("#root")
)
```

Here we are using className instead of class. There are many other properties in html which are changed in JSX and most of those are just camelcased like [global attribute](https://www.w3schools.com/tags/ref_standardattributes.asp) `contenteditable` in html is `contentEditable` in JSX.

In `styles.css` which will be imported as css file in index.html, we will have a normal css properties like below

```css
.heading {
    color: blue;
    font-size: 2rem;
}
```

In this way we can add styling to our elements.

**Styling can be done with inline csss too**. But inline css in JSX needs objects to be passed as properties not a string.

```html
<h1 style="color: red;">
    Hello World!
</h1>
```

But in JSX we have to pass javascript objects instead of strings

```jsx
const headingStyle = {
    fontSize: "1.2rem",
    textDecoration: "line-through solid red",
    color: "blue"
}

<h1 style={headingStyle}>
	Hello World!
</h1>
```

Note properties like `font-size` or `text-decoration` is now camel cased.



## React Components

We can create different components which can be used later many times instead of hard coding and using it once.

Components are returned by functions where those components are declared. Function name should start with a capital letter otherwise when using it in the html code for the render function otherwise compiler will not be able to differentiate between html vs javascript element.

```jsx
function Heading() {
    return <h1 style={myHeading}>Hello this is heading</h1>
}

ReactDom.render(
	<Heading />,
    document.querySelector("#root")
)
```

In render function compiler will automatically recognize that <Heading> is a javascript function with some html element.

It is not considered to be a good practise to keep multiple functions inside one application and moreover jsx functions should have a own file (as it is considered a good practise). 

So we should save Heading function in it's seprate file with jsx extension instead of js `Heading.jsx` and we will import it later for using it inside a render function.

This is a simple page with a Heading and simple list

File: **Heading.jsx**

```jsx
import React from "react";

function Heading() {
    return <h1>Fav Movies</h1>
}

export default Heading;
```

File: **List.jsx**

```jsx
import React from "react";

function List() {
  return (
    <ul>
      <li>Prestige</li>
      <li>Shutter Island</li>
      <li>Inception</li>
    </ul>
  );
}

export default List;
```

File: **App.js**

```jsx
import React from "react";

import Heading from "./Heading";
import List from "./List";

function App() {
  return (
    <>
      <Heading />
      <List />
    </>
  );
}

export default App;
```

File: **index.js**

```jsx
import { createRoot } from "react-dom/client";

import App from "./App";

const root = createRoot(document.getElementById("root"));

root.render(<App />);
```

This is the most common structure one will see in most of the react apps.

**One of the best things of react is that it will only update the portion of the app which is updated or changed**. Below is an app which show current time and update it every second.

```jsx
import { createRoot } from 'react-dom/client';

const root = createRoot(document.querySelector("#root"));

function getTime() {
    const date = new Date();
    const time = `${date.getHours()}:${date.getMinutes()}:${date.getSeconds()}`
    
	root.render(<p>{time}</p>)
}

// it will keep callig getTime after a gap of 1sec.
setInterval(getTime, 1000);
```

### Import/Export statements in es6

default exports will be exported directly without any need to specify the item. `export default hash` will set the hash to be the default exporting value. So anykind of import statement will import hash directly. `import anyName from "./security"` will set anyName equal to hash method.

There are ways we can export other variables too.

**File Name: security**

```react
function secureKey () {
    return "This is my security key";
}

function calculateHash(toBeHashed) {
    return convertToSha(toBeHashed)
}

function salt(toBeSalted) {
    return saltValue(toBeSalted);
}

// will set secureKey as default export value
export default secureKey;

export { calculateHash, salt };
```

while importing default values it can be named anything but other values need to have the same name when importing

```react
import Key from "./security";
import { calculateHash, salt } from "./security";
```

### Creating React project in local env.

Run `npx create-react-app app-name`. Running this will install **react, react-dom, react-scripts**. `react-scripts` are only to run app in local environment. 

This will create a directory named app-name in project folder where we have to navigate and run `npm start`.

npm start will start our local server and will automatically open it in a browser window.



## React Props

Reat props is a shortform for **React Properties**. By creating components in react using functions doesn't make sense if we have to create every element like we did in HTML. We need a way to pass properties to those cards. Without those there is nothing like reusable code.

Think of a Note application, we have a note component containing a Heading and paragraph. But for every note we have to create different function if we don't pass some values or make it dynamic. That's where Props comes in.

A property is defined as props.<item_name> in component, while in jsx code these will be defined like <item_name> = value;

```jsx
function Note(props) {
    return <div>
    	<h1>{props.heading}</h1>
        <p>{props.content}</p>
    </div>
}

root.render(<Note heading="Work Notes" content="This is the content of my Note"/>);
```



## Mapping Components

By following [previous module](#React-Props), we can now create a single Note component can reuse it anywhere we want. So if we have 3 Notes we can do it like this

```react
root.render(
	<div>
    	<Note heading="Work" content="Need to update app" />
        <Note heading="Home" content="Clean House" />
        <Note heading="Social" content="Play football game with friends" />
    </div>
);
```

This is really nice that we don't have to write all the code for Note element and we can dynamically set Note's properties using **React props.**

But it still makes no sense to write manually these 3 Notes. It is where **map** function comes in. We have seen map function in python, java etc. And what it does in those 2 languages is take an array as an input than process each item from a given function and return a new list.

In javascript it does works similarly. 

```javascript
const names = ["Aaron", "Rutherford", "Michael"]

function prefixK(val) {
    return val + "K"
}

names.map(prefix);
```

We can't run statements in jsx code but we can use functions like **map, filter, reduce**.

**File: Notes.js**

```javascript
const myNotes = {
    {
    	id: 1,
    	heading: "Home",
    	content: "Clean Home"
	}
	{
        id: 2,
        heading: "Work",
        content: "Commit new changes"
    }
	{
        id: 3,
        heading: "Physical",
        content: "Play a football Game"
    }
}

export default myNotes;
```



```react
import Notes from "./Notes";

function CreateNote(note) {
    return <Note heading={note.heading} content={note.content} />
}

root.render(
	<div>
    	{Notes.map(CreateNote)}
    </div>
)
```

Map will take each element one by one and process it in CreateNote function which will return a component. **But remember that react automatically live render things which are updated or changed without affecting other components**, for that react needs to be able to differentiate components created using functions like map. To make these components differentiable we have to set a key and give it a unique id for React to identify it. In our notes a key named id is already present which is unique too. So we can pass it to the create function

```react
import Notes from "./Notes";

function CreateNote(note) {
    return <Note key={note.id} heading={note.heading} content={note.content} />
}

root.render(
	<div>
    	{Notes.map(CreateNote)}
    </div>
)
```



## Map, Filter, Reduce, Find, FindIndex

Map function takes a list and process each element through a function and return a new list with processed values.

```javascript
// Map function
const array = [1,2,3,4,5,6];

const mapped = array.map(val => val + 5);
// mapped = [6,7,8,9,10,11]

const filtered = array.filter(val => val%2 == 0);
// filtered = [6,8,10]

function processReduce(accumulator, currentVal) {
    return accumulator + currentVal;
}
const reduceValue = array.reduce(processReduce);
// reducedValue = 21;
```

Find loop through the array and as soon as it finds a value which matches the condition passed it returns it. While findIndex does the same thing but instead of returning the value like find, it instead returns Index of the item.

```javascript
const array = [32,53,6,76,43];

const value = array.find((x) => (x > 32) && (x % 2 == 0)); // 76
// it will return any value greater than 32 which is also even

const index = array.findIndex(x => (x > 32) && (x % 2 == 0)); // 3
// it will return the index of any value greater than 32 and also even.
```



## States in React

There will be times when we want to update our single component on pressing a button or updating it after a particular time instead of rendering the whole page and we also know that react is famour for it's this feature.

But the question is how will react know that we want to update a component. That's where **hooks** comes in. We will hook a state with a componenet and whenever the value of state changes react will know that this component needs to be changed.

**useState** is the function we use to bind a particular element to a component. It is a **hook** so it can only be used inside a function not in a class.

In one of the previous sections we created an app, where we were using `setInterval` method to re-render our app after every second. But now with this new app instead of re-rendering the whole page we only re-render the time component to make our website better on user's system.

**File: App.jsx**

```jsx
import React from "react";
import { useState } from "react";

function getTime() {
    const date = new Date();
    return `${date.getHours()}:${date.getMinutes()}:${date.getSeconds()}`
}

function App() {
    const [ time, setTime ] = useState(getTime());
    
    function updateTime() {
        time = getTime();
        useState(time);
    }
    
    setInterval(1000, updateTime);
    
    return (
    	<h1>{time}</h1>;
    );
}
```



## Forms in React

In React we can monitor what user is typing using **input**'s **`onChange`** method.

By using `onChange` we can monitor every click of the user in a function defined for **onChange**. We can use it with hooks to do a specific task. Like updating the UI while user is adding the name.

```jsx
function App() {
    const [ name, setName ] = useState("");
    function onInputChange(event) {
        const updatedName = event.target.value;
        setName(name);
    }
    
    return (
    	<div>
        	<h1>Hello! {name}</h1>
            <input type="text" onChange={onInputChange} />
        </div>
    );
}
```



we handle input using **hooks** in react. But how to handle **form** using React. A form when submitted sends a post request to server and refreshes itself but with react we can handle it's input before sending it to server and moreover we can stop it from sending a request to the server.

```jsx
function App() {
    const [ name, setName ] = useState("");
    const [ username, setusername ] = useState("");
    
    function handleChange(event) {
        setName(event.target.value);
    }
    
    function preSubmit(event) {
        registerUser(name);
        
        // This will stop the form from sending a post request.
        event.preventDefault();
    }
    
    return (
    	<form onSubmit={preSubmit}>
            <h1>Hello</h1>
        	<input placeholder="Enter name" onChange={handleChange}/>
            <button type="submit">Register</button>
        </form>
    );
}
```



## Managing a React Component Tree

We cap split our app in different components but there will be times when someone wants to do something with the component. Think of a todo app in which we want to change color of the text of the item which is done to red.

That is possible if our Todo Item had a state associated with it, which can be done with the below code

```jsx
import React, { useState } from "react";

function Item(props) {
    const [ isDone, setIsDone ] = useState(false);
    
    function onItemPressed() {
        setIsDone(!isDone);
    }
    return (
    	<li 
            style= {{
                color: isDone?"red":"black"
            }}
            onClick="onItemPressed">
            {props.value}
            </li>
    );
}
export default Item;
```



**But what if we want to change our state on the basis of some other value from some Outer component**. Well in that case we can pass a function or a variable in our Inner component as a prop value which we can use in our Inner Component later.

