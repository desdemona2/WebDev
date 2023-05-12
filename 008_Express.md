# Express

Express is a library based on node. It makes us task simple like jquery made things for javascript easier.

Express can make setting up a server easy with less and more readable code. For example to listen on a port we only need to run `app.listen(PORT_NUMBER, () => console.log("Started server!"))` instead of using `http.createSerer(callback).listen(PORT, callback1)`.

## Creating and listening on a port

To create a server instead of using `http.createServer(handleReq).listen(PORT, callback)` we use `app.listen(PORT, callback)` for start server and make it to listen on given PORT.

```javascript
const express = require('express');

// Create an instance of app
const app = express();

// Start listening on a PORT
const PORT = process.env.PORT || 5400;
app.listen(PORT, () => {
    console.log("Server Started on PORT: " + PORT);
});
```



## Handle get Requests

This is something which is even simpler than doing an if-else like in native node. Here we can create different handlers for different requests. We use `app.get('/', callbackFunction)`

```javascript
// Handle different requests
app.get('/', (req, res) => {
    res.send("<h1>Home Page</h1>");
})
```

Remeber `res.send` is combination of `res.write` and `res.end`. But by doing send we can't send files like html page. For that we have to use `res.sendFile`

```javascript
// Handle request for root.
app.get('/', (req, res) => {
   res.sendFile(__dirname + "/index.html"); 
});

// Handle request for about page
app.get('/about.html', (req, res) => {
    res.sendFile(__dirname + "/about.html");
});

// Handle request for contact me page
app.get('/contact.html', (req, res) => {
    res.sendFile(__dirname + "/contact.html");
});
```

**Handle 404 error page within Express**

To handle all the pages for resource not found error, we can use delimiter **\*** which represents everything. But we have to define it in the last after all other routes we have defined.

```javascript
const Express = require('express');
const app = Express();

app.get('*', (req, res) => {
    res.sendFile(__dirname + "/error.html");
});
```



## Handling post requests with Express

We can handle post requests with express and it's not too difficult atleast for simple applications. We will only run `app.post("path", callback)` to handle the post request. Below is an example of using handling post request with html form.

```html
<form action="/" method="post">
    <label for="value0">
    <input id="value0" type="text" name="var0" placeholder="value 1">

    <label for="value1">
    <input id="value1" type="text" name="var1" placeholder="value 2">

    <input type="submit" value="Calculate">
</form>
```

```javascript
const Express = require('express');
const parser = require('body-parser');
const app = Express();

app.use(parser.urlencoded({extended: false}));

app.post("/", (req, res) => {
    // variable name will be same as it is 
    // defined as name variable in form input
    let ans = Number(req.body.value0) + Number(req.body.value1);

    res.send("<h1>" + ans + "</h1>");
});

app.get("/", (req, res) => {
    res.sendFile(__dirname + "/index.html");
});


app.listen(5400, () => {
    console.log("Server Started!");
});
```

In above code we are using a parser to parse the data from url body that we get in return from the post request from form. We have to link it with our server using `app.use(parser.urlencoded({extended: true}))`. Than we define `app.post('path', callbackFunction)`, which will handle the post request at given path accordingly. `req.body` will give the body of the file in key value format (json). From `req.body` we can extract the values of variables using `req.body.name`. Here `name` should be replaced with the name defined in input field of form. Than we send a response to the server.

**body-parser** can also parse the request in *text and json format also by using* `parser.text()` and `parser.json()`. It has many other modes also.

The important point for this is that we are processing our data on our server instead of client machine.

**This also has an limitation of failing to load static files like styles.css and index.js**. This is because node can't serve static files unless specified otherwise. To make it serve static files we have to use `app.use(express.static("./path/to/file/parent/dir"))`.



***Handling Multiple post request from a single path***.

We can send different path's as our post request and handle them accordingly. Think we have 2 forms on a single html page.

```html
<form action="/login" method="post">
    <input name="username" type="text" placeholder="Username">
    <input name="password" type="password" placeholder="Password">
    
    <input type="button" value="Submit">
</form>

<form action="/register" method="post">
    <input name="createUsername" type="text" placeholder="Enter new username">
    <input name="createPass" type="password" placeholder="Enter new password">
    
    <input type="button" value="Register">
</form>
```

Now above form is sending 2 types of post requests one with path set as `/login` and `/register`.

```javascript
// to handle login request
app.post('/login', (req, res) => {
    checkCredentials(req.body.username, req.body.password);
});

// to handle register request
app.post('/register', (req, res) => {
    createUser(req.body.createUsername, req.body.createPass);
});
```



## Handle delete Requests

```javascript
app.delete('/blogs/:blogName', (req, res) => {
    if (deleteBlog(req.param.blogName)) {
    	res.send("Blog deleted")
    }
});
```

We can delete items with delete requests.



## Put and Patch Request

Think we have a document like below

```json
{
    title: "Mars",
    content: "Moon is a natural satellite of Earth! It rotates around Earth and completes a single rotation in 30 days"
}
```

In above document we have object title as Mars but it's content is of moon. Now we need to update the document to make it right. 

There are 2 ways we can request for an update, one by using put method and another by patch method. 

**Problem with *put* request** is that it will update the whole document even if we wanted to update the title only, It is meant to replace the whole document while **patch** will update only part which we intend to change.

**Using put request**

```javascript
app.put('/posts/:title', async function(req, res) {
    posts.updateOne(
        {title: req.params.title},
        {title: req.body.updatedTitle}
    ).then(doc => {
        res.send("Updated post " + doc);
    }).catch(err => {
        res.send("Some Error occured: " + err);
    });
});
```



**Using patch request**

```javascript
app.patch('/posts/:title', async function(req, res){
    posts.updateOne(
    	{title: req.params.title},
        {title: req.body.updatedTitle}
    ).then(doc => {
        res.send("Updated post " + doc);
    }).catch(err => {
        res.send("Some Error occured: " + err);
    });
});
```



## Middleware

Middleware is something that happens between server receiving a request and sending back a response. Our get, post, delete etc. requests are middleware. **Middleware takes atmost 3 values** `req, res, next`. req is request server received, res is response server is returning back, next will be a function which can be used to call the next middleware. 

If post or get are middlewares then why doesn't it contains next patameter? That's because in most cases we don't need to call another middleware for get or post requests.

We can define our middleware globally by using `use` method.

```javascript
const express = require('express');

const app = express();

app.use(customMiddleware);
app.get('/', (req, res) => {
    console.log("Home")
});
app.get('/about', (req, res)=> {
    console.log("About")
});

function customMiddleware(req, res, next) {
    console.log("I am middleware");
}
```

Middlewares execute in order. If in above code we had our `customMiddleware`  defined after get requests it would have never executed because get request will send the response without reaching to the next middleware. But there is a way to do that -> by calling next() in current middleware.

```javascript
const express = require('express');
const app = express();

app.get('/', (req, res, next) => {
    res.send("Home");
    next();
});

app.use(middleware);
function middleware(req, res, next) {
    console.log("middleware called");
}
```

In above code after sending the response for `/` path we are calling our next middleware.

We can use our middlewares to authenticate or do some other things before going to home page. To do that we need to create a middleware for authentication and pass it to the request handler before processing next middleware.

```javascript
app.get('/', authenticate, (req, res) => {
    console.log("userpage");
    res.send("Home Page");
});

function authenticate(req, res, next) {
    console.log("Authenticating user")
    boolean auth = someVerificationFunction();
    
    if (auth) {
        next();
    } else {
        res.send("You are not allowed access this page");
    }
} 
```

In above code we are sending 2 middlewares in our get request. 1st one being authenticate and 2nd being function handling **req and res** for path `/`. 



## Express parameters(Routing)

Rounting can be defined as something to handle requests. [This](https://expressjs.com/en/guide/routing.html) post may be helpful to understand routing. Here we will talk about Route parameters.

Route parameters are named URL segments that are used to capture the values specified at their position in the URL. The captured values are populated in the `req.params` object, with the name of the route parameter specified in the path as their respective keys.

```javascript
app.get('/users/:userId/books/:bookId', (req, res) => {
	res.send(req.params.<userId>)
})
```

This will handle all the requests with url as /users/**anyId**/books/**bookId**. This means we won't have to handle all different requests by creating a whole new get request handler. This will help getting rid of repetetive code.

Anything can be filled at userId and bookId which can be handled accordingly in the node application.



## Chainable Route Handlers

Chainable route handlers are the ones targeting a single path but with different request types. Most of the times we have same request path but with different request types like `/articles` with `post, get, delete` requests. We create different route handlers for all these which makes things look ugly and hard to debug, that's where **Chainable Route Handlers** comes into existence.

```javascript
// Think we have a route path for /files with delete get and post request
app.route("/files").get(
		async function(req, res) {
            res.send(getAllItems());
        }
	).post(
        async function(req, res){
            deleteAllItems();
        }
	).delete(
		async function(req, res) {
            deleteAllItems();
        }	
);
```

