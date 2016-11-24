# Basic operations on mongoDB usong the mongo shell

## Prerequisites
Have mongoDB installed and server running.

## The mongo shell
[mongo](https://docs.mongodb.com/v3.2/mongo/) shell is an interactive Javascript interface to MongoDB.
The shell fully supports Javascript, so you can crete and run your own Javascript functions in the shell
itself.

## Documentation reference
Please refer [the mongodb online documentation](https://docs.mongodb.com/v3.2/crud/) for a more
comprehensive treatment.

## The basics
Type `mongo` in the terminal to enter the mongo shell

* `show dbs` - Shows the list of existing databases.
* `db` - Shows the db currently being used.
* `use <db_name>` - Switched to the database.
* `help` - Shows the basic help info
* `db.help()` - Shows the help for db methods
* `db.<collection_name>.help()` - Shows the methods for collections. Note that the collection need
not exist in the database, it should only be a syntactically valid collection name.
* Autocomplete: Doube tap the `tab` key on `db.` or `db.<collection_name>.` to get a list of supported methods.
* `exit` OR double `Ctrl-C` OR `quit()` - exit the shell.