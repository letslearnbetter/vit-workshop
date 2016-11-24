# Persisting and fetching data from mongoDB

## Saving data
Until now, we only created a new pet object and sent it back to the client. We will go a step further
and save the new object into mongoDB.  

In `index.js`, after creating the new Pet object, save it to mongodb by adding the following lines in
the `/newPet` route handler just after the line where we create the new object.

```javascript
newPet.save( function (err) {
    console.log(err);
  })
```

The `save` function takes a callback as an argument. The argument `err` contains the error info, if any
encountered while executing this operation. In the case of no errors `err` will be `null`

Hit [http://localhost:3000/newPet](http://localhost:3000/newPet) from the browser, there would not be any
apparent difference but the the object will also be saved into mongoDB this time.  

1. Open another terminal and invoke the mongo shell with the command `mongo`
2. Switch to the `node-pets` database with the command `use node-pets`
3. Find the documents in the collection `pets` with the command `db.pets.find()`. You should find the
object that we creted in the find result.

## Fetching data
Now we want to list all the documents we have stored in mongoDB and show it on the home page. To do
this we modify the `/` route handler by removing the call to `res.render` and adding a call to `find`
method as below:  

```javascript
/* GET home page. */
router.get('/', function(req, res, next) {
  Pet.find({}, function(err, pet) {
    if (err) {
      console.log(err)
      return;
    }
    res.send(pet);
  })
  //res.render('index', { title: 'Express' });
});
```

The first arguments to `find` here is the filter, which is empty in this case meaning that all 
documents will be matched.  

The second argument is the callback which takes two arguments. The parameter `err` is the errors object
and will have the error information if any where encountered and will be nl otherwise. `pet` is the result
object that will contain the result of the find operation.

Hit [http://localhost:3000/](http://localhost:3000/) and you should find the the objects that are saved
in the `pets` collection in mongoDB will be printed out.  

The `index.js` file should look something like this at the end of this step:  

```javascript
var express = require('express');
var router = express.Router();
var Pet = require('../models/pets');

/* GET home page. */
router.get('/', function(req, res, next) {
  Pet.find({}, function(err, pet) {
    if (err) {
      console.log(err)
      return;
    }
    res.send(pet);
  })
  //res.render('index', { title: 'Express' });
});

router.get('/newPet', function(req, res){
  var newPet = new Pet({
    name: 'poppy',
    type: 'dog',
    age: 12
  });

  newPet.save( function (err) {
    console.log(err);
  })

  console.log(newPet);
  res.send(newPet);
});

module.exports = router;
```


