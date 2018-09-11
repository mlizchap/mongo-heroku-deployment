# mongo-heroku-deployment

## Setup 
### App Setup
- create a new project with git init 
- initialize a git repository for the project 
  ```javascript
  $ git init
  ```
- create a basic express app and server
### Heroku Setup
- create the heroku app
  ```javascript
  $ heroku create
  ```
  - generates a new git remote repository associated with your app
  - generates a random name to be associated with the app
### Database Setup
- in *mLab* - Create a new single-node Sandbox MongoDB database in US EAST
  - click on the newly created db 
  - create a new user and password 

- store the connection string (string given to you on mLab) in a config variable called `MONGODB_URI`
  ```javascript
  heroku config:set MONGODB_URI=mongodb://your-user:your-pass@host:port/db-name
  ```
  - can now access string with variable `process.env.MONGODB_URI`
  
  ## Database Connection 
  - create db variable in the global scope for reuse, this way the connection pool so that the connection can be shared amongst route handlers
    ```javascript
    var db;
    ```
  - connect to the DB, assign the database object to the `db` variable and then start the server after
    ```javascript
    mongodb.MongoClient.connect(process.env.MONGODB_URI, function (err, database) {
      if (err) {
        console.log(err);
        process.exit(1);
      }

      db = database;
      
      var server = app.listen(process.env.PORT || 8080, function () {
        var port = server.address().port;
        console.log("App now running on port", port);
      });
      
  })
  ```
## Deployment and Testing of the Deployment
- Deploy the code
  ```
  $ git add .
  $ git commit -m 'first commit'
  $ git push heroku master
  ```
- ensure at least one instance is running
  ```javascript
  $ heroku ps:scale web=1
  ```
- use cURL to issue a post request

