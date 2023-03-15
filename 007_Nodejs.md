# Node.js

node is a javascript library which can be used in backend for a website. Node is actually a javascript runtime (something like jre for java). It enables us to run javascript code on server and it's written in c++. [Here](https://nodejs.org/docs/latest/api/) is the list of native modules and their documentation on how to use those. These modules are very powerful and can be used for different tasks that can makes things easy to work with.



## Using Core Modules

To work with module we have to use `require` to enable it i.e. `let fs = require('fs')`. For all modules we have to require them to use.

```javascript
// load file system module
const fs = require('fs');

// copy file1.txt and paste it as file2.txt
fs.copyFileSync("file1.txt", "file2.txt");
```

1. **Path**

   ```javascript
   const path = require("path");
   
   // Base File Name
   console.log(path.basename(__filename));
   // this will simply extract the filename from
   // whole path returned by __filename.
   
   // will return path to file without file name
   console.log(path.dirname(__filename));
   
   // file extension
   console.log(path.extname(__filename));
   
   console.log(path.parse(__filename));
   // will return a object which will have different
   // properties like base, ext, name in one object.
   
   // Concatenate paths
   console.log(path.join('/','home','user','website','index.html'));
   // will return a new path by joining all values 
   // and will automatically add delimiter to it.
   ```

2. **File System Module**

   ```javascript
   const fs = require('fs');
   const path = require('path');
   
   // create a folder (by default most will be
   // asynchronous but there is a sync. version too)
   // and won't take any callback functions fs.mkdirSync('file path', options);
   fs.mkdir('./myFolder', {}, function (err){
       if (err) throw err;
       alert("Folder Created!");
   });
   // 1st parameter will be full file path, 2nd will
   // be options, 3rd will be callback when dir. is made.
   
   // Create and write to a file
   fs.writeFile(path.join(__dirname, 'hello.txt'), "Hello World!", err => {
       if (err) throw err;
       alert("Content written into file!");
   })	// writeFileSync(file, options)
   
   // Append to a file
   fs.appendFile(path.join(__dirname, 'hello.txt'), "Hello World!", err => {
       if (err) throw err;
       alert("Content written into file!");
   })	// appendFileSync(file, options)
   
   // Read a file
   fs.readFile(
       path.join(__dirname, 'hello.txt'),
       'utf8',
       (err,data) => {
           if (err) throw err;
           console.log(data);
   }); // fs.readFileSync()
   ```
   
3. **OS**

   ```javascript
   const os = require('os');
   
   // get platform (win98, linux, darwin)
   os.platform();
   
   // get architect
   os.arch();
   
   // get cpu core info
   os.cpus();
   
   // Get Free Memory
   os.freemem();
   
   // Get total memory
   os.totalmem();
   
   // Get Home Directory
   os.homedir();
   
   // System UPtime
   os.uptime();	// in sec.
   ```

4. **url**

   ```javascript
   const url = require('url');
   const myUrl = new URL("www.mywebsite.com");
   ```

5. **Events**

   Events module can be used to create and listen to node events. To create an event we use `emitterObj.addListener(callback)` or `emitterObj.on('event', callback)`. To raise and event we use `emitterObj.emit('event').`

   ```javascript
   const EventEmitter = require('events');
   
   // not really required as MyEmitter can be 
   // initialized directly. This is useful when we want
   // some customizations in EventEmitter.
   class MyEmitter extends EventEmitter {}
   
   const myEmitter = new MyEmitter();
   
   // creating a listener for an event
   myEmitter.on('myCustomEvent', () => console.log("Custom Event Fired!"));
   
   // Raising/Firing an event
   myEmitter.emit('myCustomEvent');
   ```

   To create a custom logger for node we can use events:

   ```javascript
   // Custom Logger Class
   const EventEmitter = require('events');
   const uuid = require('uuid');
   class Logger extends EventEmitter {
       log(msg) {
           this.emit('log_msg', `${uuid.v4()}: ${msg}`);
   	}
       // whenever loggerObj.log will be called it will fire a 'log_msg'
       // event. For which one can define a listener in file
   }
   
   module.exports = Logger;
   
   // Using this Logger in another file
   const Logger = require('./logger');
   
   const logger = new Logger();
   
   logger.on('log_msg', (data) => {
       console.log(data);
   })
   
   logger.log("Task Done!");
   ```

   

6. **http**.

   Creating a web server is easy with express but we can also create it using http only.

   Here is a simple webserver which can be visited with url: ***localhost:5000***.

   ```javascript
   const http = require('http');
   
   // Create a simple webserver.
   http.createServer((req, res) => {
       res.write("Hello World!");
       res.end();
   }).listen(5000, () => console.log('Server Running!'));
   ```

   Another web-server but not very simple.

   ```javascript
   const http = require('http');
   const path = require('path');
   const fs = require('fs');
   
   const server = http.createServer((req, res) => {
       console.log(req.url);
       if (req.url === '/') {
           // adding headers
           // writeHead(response code, headers)
           res.writeHead(200, {'Content-Type': 'text/html'})
           fs.readFile(__dirname + "/index.html", (err, content) => {
               if (err) throw err;
               res.write(content);
               res.end();
           });
       } else if (req.url === '/about') {
           // we can simply end the request with sending html string
           res.end('<h1>About Page</h1>');
       } else if (req.url === '/api/users') {
           const users = [{
               "name": "Rohit",
               "age": 35,
               "Jersey": 45
           }, {
               "name": "Virat",
               "age": 34,
               "Jersey": 18
           }];
           res.writeHead(200, {'Content-Type': 'application/json'});
           res.write(JSON.stringify(users));
           res.end();
       }
   });
   
   // It will run on PORT specified in environment
   // if there is not PORT var in env than it
   // will run on port 5400
   const PORT = process.env.PORT || 5400;
   
   server.listen(PORT, () => {
       console.log(`Server Running on port: ${PORT}`);
   });
   ```

   **Everytime we change this file we have to restart the server. But this can be avoided by using nodmon, but we need to install it globally for that**. If it is not global we need to edit `package.json` and set scripts like below

   ```json
   "scripts": {
       "start": "node index",
       "dev": "nodemon index"
   },
   ```

   After setting these we can simply run `node index` by running `npm run start` and `nodemon index` via `npm run dev`.

## Using External Modules

To install external modules we need to use a package manager for node ***(npm (node package manager))***. These packages are reusable code, which can be used to reduce time to create something and have a look at different types of ideas others are using.

To add any external module we need to initialize our website directory as npm package using `npm init`. After that we will install the module we want to add. Best place to find modules for node is [npmjs website](https://www.npmjs.com/). From there we can find the module and also we have to install it in **our project** directory. `npm install express` can be used to install express package locally for one project. `npm install -g express` will install the project globally.

A dependency can also be install as dev dependency. `npm install --save-dev nodemon` or `npm install -D nodemon`. **Nodemon** is a dependecy which can prevent server restart on every change in our project. After installing a dependency one folder will be created which will have dependencies in it `node_modules`. It will also create 2 files `package.json` and `package.json.lock`. `package.json` will be created on initializing a directory as npm package and it will contain information about project and also have dependencies listed in it. `package.json.lock` will have information about dependencie like version, link etc. 

If we somehow deleted `npm_modules` directory we don't have to install every package again. We can simply run `npm install` and it will sync the directory with `package.json` file and download all the dependencies listed in `package.json`.



## Creating Modules(require and export)

One can also create his own modules (simply some files which have an export).

```javascript
// filename: value.js
const value = {
    name: "something",
    version: "1.0.0",
    link: "www.somesite.com"
};

// export will be used to export a value
module.exports = value;
```

```javascript
// filename: index.js
const value = require('./value');

console.log(value.name);
```

A file running with node is auto-wrapped inside a function with definetion `function(exports, require, module, __filename, __dirname)`. That's why we can use functions like exports and require.



Node comes with a **repl**(read evaluate print loop) which can allow to run javascript directly in console. Simply running `node` in terminal will start the interactive node session(repl session).

