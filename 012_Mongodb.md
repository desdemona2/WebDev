# MongoDB

MongoDB is a cross-platform document-oriented database system. It falls under the category of NoSQL databases, which means that it does not use a traditional table-based relational database structure. Instead, MongoDB uses a document data model with dynamic schemas, allowing it to store data in a JSON-like format called BSON (Binary JSON). This database support **replication and sharding**. On linux default path for database is `/var/lib/mongodb`, make sure current user has permission to the database directory and has permissions set to 755. 



It supports many things including **Aggregation, Sharding, Indexing, Replication** 

1. **Aggregation :-** Grouping of data from various documents and returning a single result, on which queries can be run.
2. **GridFS :-** It divides the file into parts which can be stored in chunks, this is useful if a file exceed the size limit.
3. **Sharding and Replication:-** Sharding allow partition of data across multiple using shared key and the process of synchronization all this organised data across many servers to offere redundancy is called **replication**.
4. **Indexing :-** Data in mongodb is indexed so it makes process of searching in database easy and fast.



## Integrating MongoDB with node.js

While connecting mongodb with node, we have 2 options. First one is by using mongodb native driver or to use a **ODM(Object Document Mapper) called mongoose**.

**Using Mongo DB Native Driver** (Node.js)

We need to integrate our server with mongodb using mongodb drivers, but for that we first need to install mongodb driver using npm.

1. Initialize a project using npm => `npm init`
2. Install mongodb driver => `npm install mongodb`
3. import mongodb client => `const {MongoClient} = require("mongodb")`
4. set uri for client => `const uri = "mongodb://localhost:27017"`
5. create client using uri => `const client = new MongoClient(uri)`



### Find Documents

```javascript
const { MongoClient } = require("mongodb");

const uri = "mongodb://localhost:27017"
const client = new MongoClient(uri)

const db = "school";

async function run() {
    try {
    	const database = client.db(db)
    	const students = database.collection("students")
    	
        const query = {class : 10}
        
        // Find single document
        const result = await students.findOne(query)
        
        // Find multiple documents
        const query_mul = {percentage: {lt: 60}}
        const mul_result = await students.(query)
        // Print error if no document found
        if ((await students.countDocuments(query) === 0) {
        	console.log("No Document Found!")    
        }
    } catch (e) {
        console.log("Some error occured: " + e)
    } finally {
        console.log("Closing connection...")
        client.close()
    }
}

run().catch(console.dir)
```



### Inserting documents

```javascript
const { MongoClient } = require("mongodb")

const uri = "mongodb://localhost:27017";
const client = new MongoClient(uri);

const dbName = "fruits"

async function run() {
   try {
       const database = client.db(dbName)
       const collection = datbase.collection("fruits")
       
       const apple = {
           name: "Apple",
           content: "A fruit many people hate because newton loved it"
       }
       // Insert One element in database
       collection.insertOne(apple);
       
       const orange = {
           name: "Orange",
           content: "This fruit is of orange color"
       }
       
       const banana = {
           name: "Banana",
           content: "Fruit that is nutrition rich!"
       }
       
       collection.insertMany([orange, banana]);
   } catch (e) {
       console.log(`Some error Occured: ${e}`)
   } finally {
       console.log("Closing Client!")
   }
}
run().catch(console.dir)
```



### Update/Replace Document

```javascript
const { MongoClient } = require('mongodb')

const uri = "mongodb://localhost:27017";
const client = new MongoClient(uri);

async function run() {
    try {
        const flix = client.db('flix_movies')
        const movies = flix.collection('movies')
        
        // upsert will create the doc if it already doesn't exist
        const options = {upsert: true}
        
        const query = {name: "Jungle Safari"}
        const mul_query = {rating: ${lt: 7}}
    	
    	const update_one = {$set: {genre: "Safari"}}
        // get the result of single insertion
    	const result = await movies.updateOne(query, update_one, options)
        
        const updateMany = {$set: {watchable: false}}
        // get the result of multiple insertions
        const result_mul = await movies.updateMany(mul_query, updateMany)
        
        const rep_query = {name: "Shaktimean"}
        const replacement = {name: "Shaktimaan", rating: 9.8, genre: "Supernatural"}
        
        const rep_result = await movies.replaceOne(rep_query, replacement)
    } catch (e) {
        console.log("Some error occured!")
    } finally {
        console.log("Closing the client...")
        client.close();
    }
}

run().catch(console.dir)
```



### Delete Documents

```javascript
const { MongoClient } = require('mongodb')

const uri = "mongodb://localhost:27017"
const client = new MongoClient(uri)

async function run() {
	try {
    	const query = {name: "Anabella: The stupid doll"}
	
        const flix = client.db('flix_movies')
        const movies = flix.collection('movies')

        const res = await movies.deleteOne(query)

        if (res.deletedCount === 1) {
            console.log("Deleted One Entry!")
        }

        // Delete multiple documents
        const query_mul = {rating: {$regex : "The"}}
        
        const res_mul = movies.deleteMany(query_mul);
    } catch (e) {
        console.log(`Some error occured! ${e}`)
    } finally {
        console.log("Closing Client...")
        client.close();
    }
}

run().catch(console.dir)
```



## Basic Commands

<table>
    <tr>
    	<th>Command</th>
        <th>Used for</th>
    </tr>
    <tr><th colspan=2 style="text-align: center">READ</th></tr>
    <tr>
    	<td>show dbs</td>
        <td>Will list all the databases currently installed</td>
    </tr>
    <tr>
    	<td>use &lt database name &gt</td>
        <td>Create a database with name database</td>
    </tr>
    <tr>
    	<td>db</td>
        <td>return the current database we are working in</td>
    </tr>
    <tr>
    	<td>show collections</td>
        <td>Return all the collection in the current database</td>
    </tr>
    <tr>
        <td>db.&ltcollection name&gt.find()</td>
    	<td>Show all document in a collection</td>
    </tr>
    <tr>
        <td>db.users.find(<strong>{profession: "Cricketer", salary: "$gt1000"}</strong>) ($gt represents greater than)</td>
    	<td>Show all documents matching a particular query</td>
    </tr>
    <tr>
        <td>db.users.find({name: "Rahul"}, {name:1, percentage: 1, _id: 0})</td>
    	<td>Return only a particular value from find</td>
    </tr>
    <tr><th colspan=2 style="text-align: center">CREATE</th></tr>
    <tr>
    	<td>db.users.insertOne({id: 1, name: "Charlie"})</td>
        <td>Will add a document in users collection</td>
    </tr>
    <tr>
    	<td>db.users.insertMany([obj1, obj2, obj3...])</td>
        <td>This will insert values in the database</td>
    </tr>
    <tr><th colspan=2 style="text-align: center">UPDATE</th></tr>
    <tr>
        <td>db.users.updateOne({_id:1}, {$set: {stock: 32}})</td>
    	<td>Update data in a collection</td>
    </tr>
    <tr>
    	<td>db.users.updateMany({age: {$lt: 18}}, {$set: {status: "Reject"}})</td>
        <td>Update all documents with age less than 18 to have status rejected</td>
    </tr>
    <tr><th colspan=2 style="text-align: center">DELETE</th></tr>
    <tr>
    	<td>db.users.deleteOne({id: 2)</td>
        <td>delete element with id 2</td>
    </tr>
    <tr>
    	<td>db.users.deleteMany({age: {$lt: 18}})</td>
        <td>will delete all the elements with age less than 18</td>
    </tr>
    <tr><th colspan=2 style="text-align: center">DELETE Database</th></tr>
    <tr>
    	<td>db.dropDatabase()</td>
        <td>will delete the currently selected database</td>
    </tr>
</table>





## Mongoose (7.0.3)

Mongoose is the alternative driver to access the mongodb from node. It is more simple and elegant than the native driver.

By using mongoose as a driver we can specify a schema for the collection we are creating. It also simplifies pointing to the database. **(This page is based on mongoodb 6.0.5)**

**With mongoose everything is derived from a Schema.** So after importing mongoose and creating a database we need to define a schema.

After declaring a schema we need to compile the schema to a model which will be used to create documents.

We can add functionality to the schema(before compiling it with model) by using `schema.method.play = function() {}`.

[Here](https://mongoosejs.com/docs/api/model.html) we can find the documentation of various mongoose methods.

### Insert using mongoose

```javascript
const mongoose = require('mongoose')

// This will create a connection localhost:port to the database fruit.
// if fruit doesn't exist it will create one.
mongoose.connect('mongodb://127.0.0.1:27017/players')

// Declare schema for players database
const Schema = new mongoose.Schema({
    name: String,
    age: Number,
    jersey_number: Number
});

// Create a collection Crickets using mongoose.model
const cricketer = mongoose.model("Cricket", Schema);

const Rohit = new cricketer({
    name: 'Rohit',
    age: 35,
    jersey_number: 45
});

const Virat = new cricketer({
    name: 'Virat',
    age: 34,
    jersey_numer: 18
});

// save documents in a collection name Cricket inside players database. 
Rohit.save();
Virat.save();

// Insert multiple values
const Rahul = new cricketer({
    name: 'Rahul',
    age: 29,
    jersey_number: 1
})

const Dhawan = new cricketer({
    name: 'Shikhar',
    age: 33,
    jersey_number: 43
})

cricketer.insertMany([Rahul, Dhawan])
```



### Read documents from Mongoose

```javascript
const mongoose = require('mongoose')

function main() {
    // connect with database
	mongoose.connect('mongodb://127.0.0.1:27017/players')
    
    // get schema
    const Schema = new mongoose.Schema({
        name: String,
        role: String,
        age: Number
    })
    
    // compile model from schema
    const players = mongoose.model("Player", Schema)
    
    // run find query on model
    players.find().then(docs => {
        console.log(docs)
    }).catch(err => {
        console.log(err)
    })
    
    players.find({age: ${gt: 30}}, 'name role -_id').then(docs => {
        console.log(docs)
    }).catch(err => {
        console.log("Failed to write data")
    })
}

main().catch(err => console.log(err))
```

We can also add queries in find like **`find({age : {$gt : 30}})`**.



###  Data Validation with mongoose

Validators are used to validate the document being inserted into the database. Mongoose has several built-in validators for different types with different keys. E.g. For **Numbers we have min and max validators**. For **String we have match, minlength, maxlength, enum** type validators. 

**We can also set a value to be unique but for that to work properly we need wait for indexes to get ready before insertion into the database**. There are much more things which can be done using mongoose like validating a value for a field or triming down the input String.

Data validation is done in schema with various properties like type min max size etc.

```javascript
const user_schema = new mongoose.Schema({
    name: {
        type: String,
        default: "Anonymous"
    }
    username: {
        type: String,
        required: [true, "Username is a required field"],
        trim: true,
        lowercase: true,
        minlength: 6,
        maxlength: 20,
        unique: true,
        validate(value) {
            if (!isValid(value))
            console.log(`Username ${value} is Invalid!`)
        }
    },
    password: {
        type: Number,
        min: 6,
        max: 10
    }
})
```

By using unique we can set a value to be unique but for that to work we need to create indexes before inserting data otherwise it won't work as for uniqueness indexes are checked. So before inserting we need to run `collection.on('index', (err) => console.log("Some error"))`

```javascript
users.on('index', (err) => console.log())
```

Now one can create users and add those to database using `save()` or `users.insertMany([A, B, C])`.



### Updating using mongoose

There are few ways with which one can update a document i.e. `replaceOne, replaceMany, updateOne, updateMany`.

```javascript
const user = mongoose.model("User", user_schema);
const query = {name: "David"}

async function update() {
    // update some previous key
    const res = await user.updateOne(query, {age: 24})
    	.then(doc => {
            console.log(doc);
        }).catch(err => {
            console.log(err);
        });
	console.log(`Matched: ${res.matchedCount}`)
    console.log(`Modified: ${res.modifiedCount}`)
    
    // update many documents
    const queryM = {age: {$lt: 40}};
    const resl = await user.updateMany(queryM, {allowed: true})
}

update();
```

**Using replace methods**

```javascript
app.put('/articles/:articleName', async function(req, res) {
    articleModel.replaceOne(
        {title: req.params.articleName},
        // Here req.body is directly passed as content as it 
        // is also a json object with a similar 
        {title: req.body.title, content: req.body.content}
    );
    
    // use replaceMany
    articleModel.replaceMany({
        {id: {$lte: 12}},
		{title: req.body.title, content: req.body.content}
    });
});
```



**Updating only a certain field in a document**

```javascript
app.put("/articles/:articleTitle", async function(req, res) {
    articleModel.updateOne(
        {title: req.params.articleTitle},
        // setting req.body as value for set will ensure that
        // only part which we want to update is updated and 
        // other parts of the documents are intact because we
        // will send only data which needs to be updated.
        {$set: req.body}
    ).then(doc => {
        console.log("Document Updated: " + doc);
    }).catch(err => {
        console.log("Error occured: " + err);
    })
});
```



### Deleting Documents in mongoose

```javascript
const user = mongoose.model("User", user_schema);
async function delete() {
    const filter = {name: "Sonic"}
    const res = user.deleteOne(filter);
	if (res.deletedCount === 1) console.log("Deleted!")

	const query = {age: ${lt: 18}};
	const res_m = user.deleteMany(query);
	console.log("Total deleted elements are: " + res.deletedCount);
}
```

What if we have an array as an item in a schema which have values from another schema. For example consider this case - A schema with a name and an array whose elements are another schema.

```javascript
const team_schema = new mongoose.Schema({
    name: String,
    member_count: Number,
    Captain: String
})
const Team = mongoose.model("Team", team_schema);

const game_schema = new mongoose.Schema({
    name: String,
    total_players: Number,
    teams: [Team]
})
const Game = mongoose.model("Game", game_schema);
```

What if we have to remove a team member from array team? Will we seach for the team in the whole array one by one with **O(n)** time complexity. But for a bigger list it can take from minutes to hours. This is the perfect time to use **`$pull and $pullAll`** operators. [Here](https://www.mongodb.com/docs/manual/reference/operator/update/pull/) we can get more information on `$pull`. This is not the only operator we can use, there are many other operators as well like `$sort, $push, $position etc`. [Here is a list of Array Update Operators](https://www.mongodb.com/docs/manual/reference/operator/update-array/).
