# sails-userCrud

a [Sails](http://sailsjs.org) tutorial application demonstrating adding a user to mongo db

Based on the tutorial from [rawEngineering](http://www.raweng.com/blog/2014/09/24/how-to-get-started-with-sails-js/)

Install sails + sails-mongo:
  
    npm install -g sails sails-mongo

You will need to have mongo installed and mongod running in the background. Get it: [Mongodb.org](https://www.mongodb.org/)

Create new sails project:

    sails new userCrud
    
Create user api model + controller:
    
    sails generate model User name:string email:string password:string
    sails generate controller User index create show edit delete
    
Check the `api` and `controllers` folders. You should see some new boilerplate files.

Now head to `config/connections.js` and setup your mongo config. Here's mine. Default mongoDB uses test db. User and password is blank unless you configure otherwise. This can be done in the mongo CMD. Check this out for setting up mongo users: [Creating Admins](http://docs.mongodb.org/manual/tutorial/add-user-administrator/)

    someMongodbServer: {
    adapter: 'sails-mongo',
    host: 'localhost',
    port: 27017,
    user: '',
    password: '',
    database: 'test'
  },

In `config/models.js` enable your db connector:

    connection: 'someMongodbServer';

In `api/controllers/UserController.js` add this function:

    create: function(req, res){
    var params = req.params.all()

      User.create({name: params.name}).exec(function createCB(err,created){
        return res.json({
          notice: 'Created user with name ' + created.name
        });
      });
    }
    
This handles the C in Crud. Head over to `config/routes.js`:

    'post /User': 'UserController.create'
    
We are saying only allow POST to /USER calling controller UserController, function create.

Set sail!

    sails lift
    curl -X POST --data 'name="github octocat"' http://localhost:1337/User
    
Server should respond with: `Created user with name github octocat`. Sails launches a small webserver on port `1337`.

