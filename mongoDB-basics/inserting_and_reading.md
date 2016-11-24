# Inserting and Finding documents with mongo shell

## Insert operations
1. Create a new database called `learn_mongo` with `use learn_mongo`. Note that this command
switches you to the new database even though the database does not actually exist on the disk yet.
2. Run `show dbs` and verify that `learn_mongo` does not exist yet. The entry will start appearing
when we actually start adding some documents into this database.
3. Insert a new document into the `pets` collection: `db.pets.insert({name: 'poppy', type: 'dog', age: 12})`
4. Run `db.pets.find()` to retrieve the document just creted. The result would look something like the
following:  
`{ "_id" : ObjectId("5836dc8d10c4218797088eb4"), "name" : "poppy", "type" : "dog", "age" : 12 }`  
Notice that we never inserted an `_id` field. `_id` is a mandatory field and it must be present in
all documents. It also must be unique across the documents in a collection. In the event that
`_id` field is not supplied, mongoDB automatically generates an `_id` using the objectID.
5. We can also insert multiple documents in one go. Lets try that now:  
`db.pets.insert([{_id: 2, name: 'husky', type: 'dog', age: 7}, {_id: 3, name: 'precious', type: 'cat', age: 9}])`
6. Lets see all the documents again, Run the command `db.pets.find({})`
Note that this command is equivalent to `db.pets.find()`. The argument to `find()` is a query object.
This means that we are looking for documents wothout any filter, that is we will match all documents
int the collection.
7. MongoDB has a flexible schema. This means that the documents in a collection can have different structures
Lets insert some documents with different structures:  
`db.pets.insert([{}, {randomKey: "randomValue"}, {name: "bell"}])`  
We inserted an empty document, a document which has key which is not found in any other document and
a document which has only a subset of the keys that we have been using. Run `db.pets.find()` again to see
the results.

## Basic querrying
1. To filter by a particular field name: `db.pets.find({name: 'poppy'})`
```
{ "_id" : ObjectId("5836dc8d10c4218797088eb4"), "name" : "poppy", "type" : "dog", "age" : 12 }
```
2. We can also filter by a combination of fields `db.pets.find({type: 'dog', age: 7})` This will fetch
all documents which have the `type` field set to dog AND the age field set to 7.
```
{ "_id" : 2, "name" : "husky", "type" : "dog", "age" : 7 }
```
3. Or operation can be done using the `$or` keyword like so: `db.pets.find({$or: [{type: 'dog'}, {age: 9}]})`
```
{ "_id" : ObjectId("5836dc8d10c4218797088eb4"), "name" : "poppy", "type" : "dog", "age" : 12 }
{ "_id" : 2, "name" : "husky", "type" : "dog", "age" : 7 }
{ "_id" : 3, "name" : "precious", "type" : "cat", "age" : 9 }
```
4. Keywords like `$lt`, `$gt`, `$lte`, and `$gte` can be used to perform logical comparisons like
less than, greter than, less than or equal to, greater than equal to. Ex: to get all documents with
the age field less than 10: `db.pets.find({'age': {$lt: 10}})`
```
{ "_id" : 2, "name" : "husky", "type" : "dog", "age" : 7 }
{ "_id" : 3, "name" : "precious", "type" : "cat", "age" : 9 }
```


