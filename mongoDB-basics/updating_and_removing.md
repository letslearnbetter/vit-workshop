# Updating and removing documents

## Updating
Updates in mongodb are atomic. If two updates are sent at the same time, then the order is not
guaranteed but both the updates are guranteed to happen one after the other.

Let's start by updating the document with `name` "Bell" to have `type` "cow".  
`db.pets.update({name: "bell"},{type: 'cow'})`
Look at the result with `db.pets.find()`  
But wait! the result isn't what we expected!
```
{ "_id" : ObjectId("5836e04710c4218797088eb7"), "type" : "cow" }
```
Looks like the name field was removed.  

The update function takes two parameters, the first being the quey and the second being the replacement
document. So, our update operation actually replaced the existing document!  

To add a new field in an existing document, we need to use the `$set` operator like so:
`db.pets.update({type: "cow"},{$set: {name: 'Bell'}})`  
And this time we have the right behavior:
```
{ "_id" : ObjectId("5836e04710c4218797088eb7"), "type" : "cow", "name" : "Bell" }
```
### $inc
The `$inc` keyword can be used to increment a numerical field. If the field does not exist,
it will be created with an inital value 0 and then incremented: `db.pets.update({type: "cow"},{$inc: {age: 2}})`  
```
{ "_id" : ObjectId("5836e04710c4218797088eb7"), "type" : "cow", "name" : "Bell", "age" : 2 }
```

### The multi option
By default, the update operation will update only one out of all the matches that it finds. This 
behavior can be changed to update all matching documents using the multi flag.
Ex: `db.pets.update({type: "dog"},{$inc: {age: 1}}, {multi: true})`

### Upsert
If an update is performed with the upsesrt option set to true, and there are no matching documents
found for the query used, the update operation will result in the creation of a new document.  
`db.pets.update({type: "mouse"},{$set: {name: "Stuart"}}, {upsert: true})`
```
{ "_id" : ObjectId("58373324c2f12e8d441ee3ee"), "type" : "mouse", "name" : "Stuart" }
```

## Removing
To remove documents that have the name field as null: `db.pets.remove({name: null})`

To remove the entire collection: `db.<collection_name>.drop()` 



