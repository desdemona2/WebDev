# Authentication and Security

There are different ways we can authenticate users. Some are secure some aren't enough

1. Store username/email and password in mongoose database as plain text.
2. Store username/email and password in mongoose with password being in encrypted state



## Storing Password(Encrypted) in MongoDB

We need to use some encryption technique to encrypt data in our database. There are different kind of encryption we can use, one of them is `mongoose-encryption` (needs to be installed using npm).

With this type of encryption we can do 2 different things.

1. We can use a **encryptionKey and signingKey**, which we can plug later to our schema.

   ```javascript
   const encKey = "SomeRandomHardToGuessText";
   const sigKey = "SomeOtherRandomHardToGuessText";
   
   // plug
   userSchema.plugin(encrypt, {encryptionKey: encKey, signingKey: sigKey});
   ```

2. Using **Secret String** instead of 2 different keys

   ```javascript
   const encrypt = require("mongoose-encryption");
   
   const userSchema = new mongoose.Schema({
       email: String,
       password: String
   });
   
   const secret = "MySecretStringThatNoOneKnows";
   // If encryptedFields is not passed, it will encrypt everything in database
   userSchema.plugin(encrypt, {secret: secret, encryptedFields: ['password']});
   
   const User = mongoose.model("User", userSchema);
   app.post('/post', (req, res) => {
       new User({
           email: req.body.username,
           password: req.body.password
       }).save();
   });
   ```

   Best thing  about this module is that it will encrypt the data when we call save on it and will automatically decrypt when we call find on it.

**This is a bad idea to put your secret key in server file instead of environment variables**. 

1. To save a file in env variables we need to install npm package `dotenv`.
2. Now add it on top of server file `require('dotevn').config()`. That's it we don't have to do anything else in the server file.
3. Create `.env` file on root directory of the project with environment varaibles set in a format like `VARAIABLE_NAME=VALUE` i.e. `SECRET_KEY=MySuperSecretString`.
4. Variables defined in .env file can be accessed using `process.env.VARIABLE_NAME` like `process.env.SECRET_KEY`.



## Hashing Password

There are many hashing algorithm sha, md5 etc. Here we will use **md5**. There is nothing fancy about it we just need to wrap the field we want to convert to a hash in md5 object like below

```javascript
const md5 = require('md5');

const myString = "This is string that is going to be encrypted"
console.log(md5(myString)); // this will create a irreversible hash for myString.
```

As you may know **Hashing is irreversible**, So we can't generate password from a hash(it is nearly impossible as it can take upto million years). If we want to compare a value with its hashed form than we have to hash the value and compare the hashes to check if those are same or not.

Same goes for passwords. If password is saved as hash than while comparing password for login we need to hash password entered by user and compare it with hash stored on server.

```javascript
app.get('/login', (req, res) => {
    User.findOne({email: req.body.username})
    	.then(doc => {
        	if (doc != null) {
                // compare hash stored on server with 
                // hashed value of user entered password
                if (doc.password === md5(req.body.password)) {
                    res.render('secret_page');
                } else {
                    res.send("Username/password wrong")
                }
            }
    	})
});
```



**The fact that same string generates same password** can be used to get the password if hacker somehow got access to the hashed password. 

1. Think of a situation where hacker got access to a database and finds some similar hashes for different accounts as hashed passwords.
2. If hashes are same one thing is clear that passwords of all those different users is same as a single hash can generate single value.
3. If these users are using similar password than they must be using a common password
4. In this scenario hacker will get the hashes of all the common passwords and compare hashes with it, and probably will get a hash similar to one in database and now he know password of that user.

The fact that a good graphics card can generate around `20 billion` hashes in one second.



## Hashing and Salting

We saw the problem with hashing in previous section but we can cope with that problem using salting. With salting we add some random text to the password sent by the user or any other field we want to protect. Like if string we want to protect is `Hello` than a salted version of it will be `Hello749asjfla`. 

Salted version is than used to create the hash and salt is saved on server for comparison in future while loging in. [Here](https://www.npmjs.com/package/bcrypt) is the documentation for bcrypt.

There are many hashing algorithms that support salting and have npm modules like `bcrypt`. 

1. Need to be installed with `npm install bcrypt`.
2. Add it in the script using `require('bcrypt')`.
3. Unlike md5 module we need to define level of salting with this hashing type.

A single salt means simply taking plain text password adding some salt with it and than hash it. Double salting is: Do single salting than add some random stuff at the end of the hash generated using single salt and hash it again. This process continues for 3 level of salting, 4, 5... n level of salting. Remeber more than 10 level of salting maybe a lengthy process depending on processor of server. **So it is better to keep salting level upto 10**.

```javascript
const bcrypt = require('bcrypt');
const saltLevel = 10;

// adding to database
app.post('/addItem', async function(req, res) {
    bcrypt.hash(req.body.password, saltLevel)
        .then(function(err, result) {
            if (!err) {
                new User({
                    email: req.body.username,
                    password: result
                }).save();
            } else {
                res.send("Some error occured!")
            }
    	}
	);
})

// comparing with user passed password
app.post('/login', async function(req, res) {
    User.findOne({email: req.body.username})
    	.then(async function(doc) {
        	if (doc != null) {
                bcrypt.compare(req.body.password, doc.password)
                    .then(async function(result) {
                    	if (result === true) {
                        	res.render('top secret page');
                        } else {
                            res.send("Your password is wrong");
                        }
                	});
            } else {
                res.send("This username is not registered!")
            }
    	})
})
```



## Cookies and Sessions

Cookies are a great way to store data. There are different types of cookies and they store data for different operations. Some cookies save data to show relevant ads which is annoying. But there are other cookies like session cookies which tells the server if the person trying to login is already logged in or not. These type of cookies are really helpful to save a session and make user logged in which makes it a good experience because no one wants to login again and again to the same website.

There are few npm packages which will help users to deploy and read cookies from the user like

> passport
>
> passport-local
>
> passport-local-mongoose
>
> express-session
>
> express-flash (optional)

These 4 packages can be combined to make cookie handling easy.

Both passport package and express-session have their own work to do. **express-session** handles the cookies while **passport** handles the authentication.

**Passports uses the concept of strategies to authenticate requests.** Strategies can vary from simple username passpord verification to Login using Facebook, Google, LinkedIn etc. **(using OAuth authentications)**.

One more package which can be used to show something when password is wrong or username is wrong we can use **`express-flash`**.



**express-session**

We have to create session middleware using **express-session** with some options which are set as per our requirement

- cookie.expires: will be set to a Date object, by default no expiration is set so client will treat it as non-persistent cookie and will be delete when session ends.
- cookie.maxAge: will do the same work as **cookie.expires** but value given to it will be in milliseconds. **When expires and maxAge** both are defined, one defined later will be considered and former will be ignored.
- cookie.path: will set the path of the cookie to be saved. By default, this is set to `'/'`, which is the root path of the domain.
- cookie.secure: When set to true will secure the cookie. **But this needs connection to be https**.
- secret: Secret is the secret key which will be used to sign the session ID cookie. It's value will be a string that needs to be saved securely.
- resave: Forces the session to be saved back to the session store, even if the session was never modified during the request. It's value will be true or false.
- rolling: Force the session identifier cookie to be set on every response. The expiration is reset to the original `maxAge`, resetting the expiration countdown. It's default value is false.
- saveUninitialized: Forces a session that is "uninitialized" to be saved to the store. A session is uninitialized when it is new but not modified. Choosing `false` is useful for implementing login sessions, reducing server storage usage, or complying with laws that require permission before setting a cookie.

 

**passport-local-mongoose** (plm)

First one need to create plm object and then plug it in with schema. It won't alter the schema but will add a username, hash and salt value to store the username, hased password and salt value respectively.

- To configure plm we need to define the **strategy and serialsize & deserialize functions**

  - ```javascript
    passport.use(new LocalStrategy(User.authenticate()));
    // after version 0.2.1 above line can be replaced with
    // passport.use(User.createStrategy());
    
    passport.serializeUser(User.serializeUser());
    passport.deserializeUser(User.deserializeUser());
    ```

  - serialize will convert state of an object into byte-stream and deserialize will do the opposite.

- While pluggin in additional options can be used as

  - saltlen: specifies the salt length in bytes. Default: 32
  - `keylen`: specifies the length in byte of the generated key. Default: 512
  - `interval`: specifies the interval in milliseconds between login attempts, which increases exponentially based on the number of failed attempts, up to maxInterval. Default: 100
  - `maxInterval`: specifies the maximum amount of time an account can be locked. Default 300000 (5 minutes)
  - `usernameCaseInsensitive`: specifies the usernames to be case insensitive. Defaults to 'false'.
  - `usernameLowerCase`: convert username field value to lower case when saving an querying. Defaults to 'false'.
  - `limitAttempts`: specifies whether login attempts should be limited and login failures should be penalized. Default: false.
  - `maxAttempts`: specifies the maximum number of failed attempts allowed before preventing login. Default: Infinity.
  - `unlockInterval`: specifies the interval in milliseconds, which is for unlock user automatically after the interval is reached. Defaults to 'undefined' which means deactivated.
  - `findByUsername`: Specifies a query function that is executed with query parameters to restrict the query with extra query parameters. For example query only users with field "active" set to `true`. Default: `function(model, queryParameters) { return model.findOne(queryParameters); }`. See the examples section for a use case.

- In addition to all this custom error messages can be set by overriding default error messages.

  - `MissingPasswordError`: 'No password was given'
  - `AttemptTooSoonError`: 'Account is currently locked. Try again later'
  - `TooManyAttemptsError`: 'Account locked due to too many failed login attempts'
  - `NoSaltValueStoredError`: 'Authentication not possible. No salt value stored'
  - `IncorrectPasswordError`: 'Password or username are incorrect'
  - `IncorrectUsernameError`: 'Password or username are incorrect'
  - `MissingUsernameError`: 'No username was given'
  - `UserExistsError`: 'A user with the given username is already registered'



### Working with passport with mongoose

Working with passport-local-mongoose is way easier than working with native passport-local as passport-local-mongoose handles many things automatically which one has to do on his own in passport-local.

We have to follow below steps to make it work

1. import these modules using require `express-session passport passport-local-mongoose`. We don't need to import `passport-local` as it is a dependency of `passport-local-mongoose` and it will handle it automatically.

2. We need to create a session middleware using below code

   ```javascript
   app.use(session({
       secret: "A super secret string which should be stored in .env file",
       resave: false,
       saveUninitialized: false
   }))
   ```

   - **resave** will ensure that if file is not changed than it is not resaved again
   - **saveUninitialized** will ensure that file is not saved if it is uninitialized(empty) as it is of no use when it is empty.

3. Initialize the passport module using passport.initialize and add it to server as middleware using **`app.use(passport.initialize())`**.

5. Add passport session as middleware to the node application **`app.use(passport.session())`**

5. Create a model from the schema. Here we are using **User ** as a model. 

6. Set `passport-local-mongoose` as a plugin for the user schema.

6. Create a strategy using `passport.use(User.createStrategy())` make sure User model is already defined using mongoose.

7. Set serializeUser and deserializeUser using these 2 lines

   ```javascript
   passport.serializeUser(User.serializeUser());
   passport.deserializeUser(User.deserializeUser());
   ```



#### Registring and authenticating user session with mongoose and passport

To register a user using passport we need to use register function predefined with passport.

```javascript
User_model.register({username: req.body.username}, req.body.password, function(err, user) {
    if (err) {
        console.log("Failed to register the user")
    } else {
        passport.authenticate(
            "local",
        	{
            	failureRedirect: '/login',
            	failureMessage: true
            }
        ) (req, res, function() {
            res.redirect('/super_secret_page');
        })
    }
})
```

with register method we are passing the username field to create an user in mongoose database with username value from request body. Any value we don't want to hash should be included in object with username like age: 22, height: 5.8 etc. Next value is password which will be salted and hashed and will be saved in a field password in mongoose database.



To login using mongoose passport we need to create an user object with the credentials passed by the user and than use `req.login` function defined in passport to login.

```javascript
// User is the model object
const user = new User({
    username: req.body.username,
    password: req.body.password
})

// try login using mongoose passport
req.login(user, function(err) {
    if (err) {
        console.log("Error: " + err);
        res.redirect('login');
    } else {
        // on successful login create a session cookie using authenticate.
        passport.authenticate("local")(req, res, function() {
            res.redirect('/super_secret_page');
        });
    }
})
```



When gonig to routes like `/register, /login` etc. one can confirm weither the user is already logged in by checking the request and calling `req.isAuthenticated()` which will return a boolean value. so we can redesign our login or register routes like this

```javascript
app.get('/register', (req, res) => {
    if (req.isAuthenticated()) {
        res.redirect('/super_secret_page');
    } else {
        res.render('register').
    }
})
```



## Third -party OAuth

OAuth is industry standard protocol for authentication. It can be used to login to a service or app using google, facebook, twitter etc. To use it with node one need to install `passport-google-oauth20` package and get the strategy from it.

To authenticate users, one must set OAuth concent in google developer console and after setting it we will get 2 values one for **CLIENT_ID** and another one for **CLIENT_SECRET**. These 2 values are very important and are required while authenticating so we need to keep them secure and available at the same time in our server code (keep those values in .env file). 

After setting up the consent for google oauth we need to get **google oauth strategy** and make passport use it by using `passport.use(strategy)`

```javascript
const GoogleStrategy = require('passport-google-oauth20').Strategy;

passport.use(new GoogleStrategy({
        clientID: "CLIENT_ID we got from google console",
        clientSecret: "CLIENT_SECRET we got from google console",
        callback: "http://<server_address:port>/auth/google/<callback name>"
	},
	function(accessToken, refreshToken, profile, cb) {
    	User.findOrCreate({googleId: profile.id}, function(err, user) {
            return cb(err, user);
        })
	}                         
))
```

Callback function in **GoogleStartegy** above is going to be execute to verify all the entries and it will return items like accessToke, profile etc.

In above strategy values in first object like clientID, clientSecret, callback will be passed to google authenticator. **Callback should be same as defined in google credentials**. Callback function will be used to return the user that authenticated with google.

***keep in mind that User in above code is a Model and findOrCreate is not  a mongoose method It is a pseudo method which is telling us to findOrCreate a user***. But there is a npm module named as `findOrCreate` which does the intended work and is better to use than implementing find or create manually. It can be installed using `npm install mongoose-findorcreate`.

```javascript
const findOrCreate = require('mongoose-findorcreate');

// Now add it as a plugin in schema
User.plugin(findOrCreate);
```



Local serialization will not work with google authentication so we need to redefine serialization and deserialization functions for passport like below (these will work for both local and google auth serialization).

```javascript
passport.serializeUser(function(user, done) {
  done(null, user.id);
});

passport.deserializeUser(function(id, done) {
  User.findById(id).then(function(user) {
      if (user == null) {
        console.log("failed to find any user with this id")
        done(null, null);
      } else {
        done(null, user);
      }
  }).catch(err => {
      console.log("Some error occured: " + err);
      done(err, null);
  });
});
```



We need to create a button for google authentication in our html files which will have a get route which will be used to start the authentication. Here take that route as `/auth/google` then we can start authentication using `passport.authenticate('google', {scope: ['profile', 'email']})`. Scope is a way to tell google that we are going to need these values (here profile and email).

Now we have to define the callback route that we defined in **google credentials** and **google strategy** which woule be in this case `/auth/google/secrets`

```javascript
app.get('/auth/google',
    passport.authenticate('google', {scope: ['profile']})
);

app.get('/auth/google/secrets',
    passport.authenticate('google', {failureRedirect: '/login'}),
        function(req, res) {
            console.log("Redirecting...")
            res.redirect("/secrets");
        }
    )
```

Here user information will be stored in request so one can access items like id using `req.body.id` which will return id of the current user.

### Setup google auth with local auth stepwise

1. Install these npm packages `express-session, passport, passport-local-mongoose, passport-google-oauth20, mongoose-findorcreate`

2. Create object from all those packages and get Strategy from google auth.

   - ```javascript
     const mongoose = require('mongoose');
     const session = require('express-session');
     const passport = require('passport');
     const passportLocalMongoose = require('passport-local-mongoose');
     const GoogleStrategy = require('passport-google-oauth20').Strategy;
     const findOrCreate = require('mongoose-findorcreate');
     ```

3. Set app to use express session using `app.use`

   - ```javascript
     app.use(session({
         secret: process.env.SECRET_KEY,
         resave: false,
         saveUninitialized: false
     }));
     ```

4. Initialize passport and enable session with passport using below commands

   - ```javascript
     app.use(passport.initialize());
     app.use(passport.session());
     ```

5. Connect with database and create userSchema and plug `passportLocalMongoose` and `findOrCreate` 

   - ```javascript
     mongoose.connect("mongodb://127.0.0.1:27017/userDB", {useNewUrlParser: true});
     
     const userSchema = new mongoose.Schema({
     	email: String,
         password: String,
         googleId: String
     });
     
     userSchema.plugin(passportLocalMongoose);
     userSchema.plugin(findOrCreate);
     ```

   - **findOrCreate** is not a mongoose function it is a different package that implements easier way for us to find or create the user if it doesn't exist.

6. Create a model object from the schema passed

   - ```javascript
     const User = new mongoose.model("User", userSchema);
     ```

7. Create strategy for local authentication and google authentication as we are implementing both

   - ```javascript
     passport.use(User.createStrategy());
     
     passport.use(new GoogleStrategy({
             clientID:     process.env.CLIENT_ID,
             clientSecret: process.env.CLIENT_SECRET,
             callbackURL: "http://localhost:5400/auth/google/secrets",
             // userProfileURL: "https://www.googleapis.com/oauth2/v3/userinfo",
             // passReqToCallback   : true
     },
     function(accessToken, refreshToken, profile, cb) {
             User.findOrCreate({ googleId: profile.id }, function (err, user) {
             return cb(err, user);
         });
     }));
     ```

8. Create serialize and deseiralize methods for packing and unpacking of session cookie.

   - ```javascript
     passport.serializeUser(function(user, done) {
       done(null, user.id);
     });
     
     passport.deserializeUser(function(id, done) {
       User.findById(id).then(function(user) {
           if (user == null) {
             console.log("failed to find any user with this id: " + id);
             done(null, null);
           } else {
             done(null, user);
           }
       }).catch(err => {
           console.log("Some error occured: " + err);
           done(null, user);
       });
     });
     ```

9. Define routes to start goole authentication

   - ```javascript
     app.get('/auth/google',
         passport.authenticate('google', {scope: ['profile']})
     );
     
     // google will redirect to this url if defined so in strategy and google developer console.
     app.get('/auth/google/secrets',
         passport.authenticate('google', {failureRedirect: '/login'}),
             function(req, res) {
                 console.log("Redirecting...")
                 res.redirect("/secrets");
             }
         )
     ```

10. (Optional: This step is for authenticating with passport local mongoose, This is already define in last type of authentication) One can create register, login, logout routes for local authentication or user creation like below.

    ```javascript
    app.route('/login')
        .get(async function(req, res) {
            if (req.isAuthenticated()) {
                res.redirect('/secrets');
            } else {
                res.render('login');
            }
        })
        .post(async function(req, res) {
            const user = new User({
                username: req.body.username,
                password: req.body.password
            })
    
            req.login(user, function(err) {
                if (err) {
                    console.log("Some Error occured: " + err);
                    res.send('Error: ' + err.name);
                } else {
                    if (user != null) { 
                        console.log("132: authenticate");
                        passport.authenticate("local")(req, res, function() {
                            res.redirect('/secrets');
                        });
                    } else {
                        res.redirect('/login');
                    }
                }
            })
        })
    
    
    app.route('/register')
        .get(async function(req, res) {
            if (req.isAuthenticated()) {
                res.redirect('/secrets');
            } else {
                res.render('register');
            }
        })
        .post(async function(req, res) {
            // register method is defined in passport-local-mongoose
            User.register({username: req.body.username}, req.body.password, function(err, user) {
                console.log(req.body.password);
                if (err) {
                    console.log("Error occured: " + err);
                    res.send(err);
                } else {
                    passport.authenticate("local", { failureRedirect: '/login', failureMessage: true })(req, res, function() {
                        res.redirect('/secrets');
                    })
                }
            })
        })
    
    app.get('/logout', function(req, res) {
        req.logout(function(err) {
            if (err) {
                console.log("Some Error occured: " + err);
                res.redirect('/');
            } else {
                res.redirect('/');
            }
        });
    });
    ```

    
