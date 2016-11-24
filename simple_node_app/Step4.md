# Getting data from web form
In this section we will construct a web form and used the input data from the form to
create the object and save into mongoDB.

## Creating a web form
Go to `views/index.jade`  
This is a jade file. [Jade](http://learnjade.com/) is a templating library for writing quick HTML.

Replace the contents of `index.jade` with the following:

```javascript
extends layout

block content
  h1 Pets
  form(action="/newPet")
    input(type="text", placeholder="Name...", name="name")
    input(type="text", placeholder="Type...", name="type")
    input(type="number", placeholder="Age..", name="age")
    input(type="submit", value="add new pet")
```
This creates a simple form with three input fields: for name, type and age, and an action that posts 
to `/newPet` route.

## Using form data to crete object and save to mongoDB

Back in `routes/index.js`, for the `/` route, comment the existing code, and add the line

```javascript
router.get('/', function(req, res, next) {
  // Pet.find({}, function(err, pet) {
  //   if (err) {
  //     console.log(err)
  //     return;
  //   }
  //   res.send(pet);
  // })
  res.render('index');
});
```

Head to the browser and hit [http://localhost:3000](http://localhost:3000) and you should see the basic
form rendered.

For the `/newPet`, replace the handler with the following:

```javascript
router.get('/newPet', function(req, res){

  var newPet = new Pet({
    name: req.query.name,
    type: req.query.type,
    age: req.query.age
  });

  newPet.save( function (err, pet) {
    if (err){
      console.log(err);
      return
    }
    console.log("new pet created: ", newPet)
    res.redirect('/')
  })
});
```

We are extracting the query parameters from the request object (`req`) and using them to create the
new object. Once we save the object we redirect back to the home page.  

Save the file and head back to the browser and reload the home page. Enter some data in the form
and hit `submit`. The page will immediately be refreshed. Go back to the terminal and verify that
an object with the data you entered has been logged. Now go to the mongo shell and verify that the
new data entered is present.

## Showing the list of existing pets in the home page
Along with the form, we would also like to show the list of all pets in our collection. Go to 
`views/index.jade` and add the following snippet to create a simple table showing the pets data:

```jade
table
      tr
        th Name
        th Type
        th Age
      for pet in pets
        tr
          th #{pet.name}
          th #{pet.type}
          th #{pet.age}
```

The syntax `#{pet.name}` means that this will be replaced by the `name` property of the variable
`pet`. We will supply this data when we call the `render` function in `routes/index.js` like so:

```javascript
/* GET home page. */
router.get('/', function(req, res, next) {
  Pet.find({}, function(err, pets) {
    if (err) {
      console.log(err)
      return;
    }
    res.render('index', {pets: pets});
  })
});
```

Save both files and reload the page in the browser. An html page with all the data entered until now should
be displayed. Enter some data in the form and hit submit. The table should get updated immediately. 


## Conclusion
In this guide we concentrated on setting up NodeJS and MongoDB and doing the most basic read and write
operations. The reader can use this uderstanding to develop more sophisticated apps.  

As an exercise, one can extend this app to have filters when showing the data in the home page.
For example, showing only cats or dogs. One can also add update and delete functionalities.

## Postman
[Postman](https://www.getpostman.com/) is a very popular and easy to use http client.
Instead of using the browser, one can simply use Postman to hit the endpoints implemented by
our node app.

## Final state of all the files modified

### `routes/index.js`

```javascript
var express = require('express');
var router = express.Router();
var Pet = require('../models/pets');

/* GET home page. */
router.get('/', function(req, res, next) {
  Pet.find({}, function(err, pets) {
    if (err) {
      console.log(err)
      return;
    }
    res.render('index', {pets: pets});
  })
});

router.get('/newPet', function(req, res){

  var newPet = new Pet({
    name: req.query.name,
    type: req.query.type,
    age: req.query.age
  });

  newPet.save( function (err, pet) {
    if (err){
      console.log(err);
      return
    }
    console.log('new pet entered: ', newPet);
    res.redirect('/')
  })
});

module.exports = router;
```

### `views/index.jade`

```javascript
extends layout

block content
  h1 Pets
  form(action="/newPet")
    input(type="text", placeholder="Name...", name="name")
    input(type="text", placeholder="Type...", name="type")
    input(type="number", placeholder="Age..", name="age")
    input(type="submit", value="add new pet")

    table
      tr
        th Name
        th Type
        th Age
      for pet in pets
        tr
          th #{pet.name}
          th #{pet.type}
          th #{pet.age}
```

### `models/pets.js`

```javascript
var mongoose = require('mongoose');

var petSchema = mongoose.Schema({
    name: String,
    type: String,
    age: Number
});

var Pet = mongoose.model('pet', petSchema);

module.exports = Pet;
```

### `app.js`

```javascript
var express = require('express');
var path = require('path');
var favicon = require('serve-favicon');
var logger = require('morgan');
var cookieParser = require('cookie-parser');
var bodyParser = require('body-parser');
var mongoose = require('mongoose');

var index = require('./routes/index');
var users = require('./routes/users');

var app = express();

// view engine setup
app.set('views', path.join(__dirname, 'views'));
app.set('view engine', 'jade');

mongoose.Promise = global.Promise;
mongoose.connect('mongodb://localhost/node-pets');

// uncomment after placing your favicon in /public
//app.use(favicon(path.join(__dirname, 'public', 'favicon.ico')));
app.use(logger('dev'));
app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: false }));
app.use(cookieParser());
app.use(express.static(path.join(__dirname, 'public')));

app.use('/', index);
app.use('/users', users);

// catch 404 and forward to error handler
app.use(function(req, res, next) {
  var err = new Error('Not Found');
  err.status = 404;
  next(err);
});

// error handler
app.use(function(err, req, res, next) {
  // set locals, only providing error in development
  res.locals.message = err.message;
  res.locals.error = req.app.get('env') === 'development' ? err : {};

  // render the error page
  res.status(err.status || 500);
  res.render('error');
});

module.exports = app;
```

