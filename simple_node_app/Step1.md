# Starting up the basic express app

## npm
(npm) is a package manager for nodeJS and javascript that makes it easy to install and manage dependencies. It is installed by default
when we install node.

## Install express-generator
[Express generator](https://expressjs.com/en/starter/generator.html) is used to scaffold the basic directory structure for a NodeJS application.
`npm install express-generator -g`
Here the `-g` flag means that this package will be installed globally.

## scaffold a new project
1. Create a new directory `mkdir node-pets`
2. Switch to the newly created directory `cd node-pets`
3. Run the express command to scaffold a basic app `express`.
4. Inspect `package.json` which should have the dependent packages and their versions listed. Thse packages are not yet installed.
5. `npm install` to install the required dependencies.
6. Notice that a `node-modules` folder will be created which will have all the required dependencies.

# Run!
Now we are ready to run our basic node app. go to the project root and use the command `nodemon bin/www` to start our node server.
Something like the following should be be printed in the console.
```
[nodemon] 1.11.0
[nodemon] to restart at any time, enter `rs`
[nodemon] watching: *.*
[nodemon] starting `node bin/www`
```
Visit [localhost:3000](localhost:3000) in a browser and a welcome page should be rendered.

# [Next](Step2.md)
