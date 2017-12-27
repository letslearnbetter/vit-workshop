# Mongoose schemas and models

## Install mongoose
[Mongoose](http://mongoosejs.com/) is a nodeJS pacakge used to interface with MongoDB.
`npm install mongoose --save`
Note that we did not use the `-g` flag because we want this package to be specific to the current project. The `--save` flag
is used to create the entry for the package in `package.json`  

## About mongoose
[Mongoose](http://mongoosejs.com/docs/index.html) is a node module that allows node apps to connect operate on mongodb.  

[Schemas](http://mongoosejs.com/docs/guide.html) are used to define the structure of the documents in mongoDB.  
Schemas are not followed stricitly by default but `strict` option can be used to enforce it.

[Models](http://mongoosejs.com/docs/models.html) are constructors for documents that are based on schemas that we
define.

## Create a new Schema and a Model
Create a new folder in you project root folder called `models`. This is where our mongoose schemas and models will reside.
Inside `models` create a file called `pets.js`

In `pets.js` require mongoose using the comamnd `var mongoose = require('mongoose')`  

Next we will create a new mongoose schema for our pets collection. Each pet should have a name, a type and an age.
Add the following lines:

```javascript
var petSchema = mongoose.Schema({
    name: String,
    type: String,
    age: Number
});
```

Next, we will use this schema to create a model. To create a model, two parameters are required:  
1. The name of the collection (singular), mongoose will look for the plural version in mongodb.
2. The schema

Add the following line to create the model:
`var Pet = mongoose.model('pet', petSchema);`

Lastly we will export this model so that other files can use it create objects using this model. Add the following  
`module.exports = Pet;`

## Connect to mongoose
1. Go to `app.js` and import mongoose again: `var mongoose = require('mongoose')`  
2. Connect to mongodb by adding this line `mongoose.connect('mongodb://localhost/node-pets');`
Here `localhost` is the hostname where mongodb server is rinning and `node-pets` is the database name.
If the database does not already exisit, it will be created when we start adding documents.  
3. Go back to the terminal and verify that the node app got restarted without any errrors and that
you are still able to access the [localhost:3000](localhost:3000) page. This means that the connection
to mongodb server was successful.

## Creating new objects using the model
Go to `routes/index.js`

This is where we define the behavior of the app when a client tries to connect to it.
For example, the following section means that when a client tries to acces the home url `/` using
 `GET` method, then our app will send back an html page with a title of 'Express`  

```javascript
/* GET home page. */
router.get('/', function(req, res, next) {
  res.render('index', { title: 'Express' });
});
```

We'll add a new route `/newPet`. Add the following lines: 

```javascript
router.get('/newPet', function(req, res){
  var newPet = new Pet({
    name: 'poppy',
    type: 'dog',
    age: 12
  });

  console.log(newPet);
  res.send(newPet);
});
```
This means that when a client does a `GET` request on the url `/newPet`, we will create a new pet
object using the model we created previously, log it on the console and also send the newly created
object back to the client.  

Save the file and hit [http://localhost:3000/newPet](http://localhost:3000/newPet) from the browser to
verify if the behavior is as expected.

## [Next](Step3.md)