# MEAN

## REFERENCES
a skeleton for a MEAN Stack is presented on 
https://github.com/NusCracker25/stapmean.git

### youtube links
- Academind: https://www.youtube.com/watch?v=1tRLveSyNz8
- https://www.youtube.com/watch?v=wtIvu085uU0
- Udemy: https://www.youtube.com/watch?v=jJAkjtXLyLw (2h35 minutes)
- Microsoft: https://www.youtube.com/watch?v=k0iGvadr_Cc (4h24min)
- Traversy Media: https://www.youtube.com/watch?v=uONz0lEWft0&list=PLillGF-RfqbZMNtaOXJQiDebNXjVapWPZ (10 movies)
- FullStackHour (Indian): https://www.youtube.com/watch?v=SD9cS_6O4zg (3h30)
## WHAT IS IT
MEAN is an accronym used to designates a widely used stack for Webapp.
it stands for MongoDB, Express, Angular, Node.js
it has a cousin with React known as MERN (Mongog, Express, React, Node.js)

## SETUP
Standard way to have it is to have a bunch of REST API for data access operating with MongoDB, Express and Node.js for the back end while the front end is usually operating with Angular served with the association of Express and Node.Js.

### useful tools to use
nodemon is an extension for node.js (to be installed globally). it allows for automatically monitoring what happens on the server side and allows for automatic reload of the server itself.
````
npm install -g nodemon
````

## STEP By STEP NOTES

### BACK END
have an app created
````
npm init
````
then give the appropriate answers
do not forget to put the proper start option in the package.json file by adding within the scripts section
"start":"node app"

#### dependencies needed
express
mongoose
bcryptjs
cors
jsonwebtoken
body-parser
passport
passport-jwt

### SERVER and Routes
create the file where the core code will be hosted
````
touch app.js
````
or with file explorer

this will be plain javascript

import all what is needed

````javascript
const express = require('express');
const cors = require('cors');
const jwt = require ('jsonwebtoken');
const passport = require('passport');
const path = require('path');
const bodyParser = require('body-parser');
const mongoose = require('mongoose');
````
then one can easily create the server to listen on a specific port
````javascript
const app = express();
const port = 3000;
//creation of the server
app.listen(port, ()=>{
    console.log('Server started on port '+port);
});

````
then in command line, execute the usual
````
npm start
````
which will look for the script labeled "start" in the package.json file
Once done, navigate to [localhost:3000](http://localhost:3000/) in any webbrowse you like.
A message indicating you that 
````
Cannot GET!
````
which is normal as nothing is currently done by the server itself! there is no route to the home page.

---
**NOTE**
for defining the port to listen to, it is also useful to refer to an environment variable (as it ease the later on configuration with docker for instance). another alternative is to use some json formated file.
---

---
**NOTE**
instead of running ````npm start```` one may use ``nodemon`` instead.
---

##### 1st route

before 'launching' the server, once can create as many routes as we want.
the creation of routes follow the standard HTTP keyword for transaction (GET, SET, ...)
````javascript
app.get('/', (req,res)=>{
    res.send('Invalid end point');
})
````
a get end-point is added to the "home page" (actually the root for the server). The route itself is the first parameter of get function, in this case `` '\' ``
Then the callback function has two parameters (``req``: request and ``res``:result), ``req`` is to be read (below one explains how) and ``res`` is being filled for further usage by the recepient.

#### middleware for CORS and body parser
Using the ``CORS`` package allows for our middleware to be called from outside its domain.
It is a middleware that either filters out or in request for external domain, in javascript world the default setting is to prevent external domain to use your API (as you may not control what is being done with it)
````javascript
app.use(cors());
````
**MORE DETAILS**: can be found on cors [website](https://github.com/expressjs/cors) to finely configure it

Other midleware to be used is ``body-parser``; this one helps in reading out the content of request, either expressed in json format of else

````javascript
app.use(bodyParser.json());
````
#### creating the users routes

Last but not least, the routes for managing users are defined in another javascript file (so as to avoid overfloading of the main one and stick to simple files)

in a first step this users are to be managed in a file (defined as a javascript one).
A good express practice is to create a folder named ``routes`` into which the files describing the different routes will be created.

here we augment the ``require`` section with the proper reference to the users.js file
````javascript
const users = require('./routes/users');
````
the created variable is then referred to (after the middleware definition)
````javascript
app.use('/users', users);
````
this line of code explicitized that for any routes starting with ``/users`` we must first use the ``users`` variable. In users.js, a Router is defined which covers the various routes below the path ``/users``

#### Router in users.js for users routes

the ``users.js`` file embeds the definition of expected behavior for all the related routes.
this is a plain javascript file which makes usage of ``express``
````javascript
const express = require('express');
const router = express.Router();
````
Express is used to get access to the actual Router object which is used for registering additionnal routes

````javascript
router.get('/register',(req, res, next)=>{
res.send('REGISTER');
});
````
---
**NOTE**
here pay attention to the way routes are being built recursively. the associated route to the above end point is the concatenation of ``/users`` and ``/register``
---

one shall read the express markdown tutorial to get some memories of the usage of routes especially with the callback making use of ``(req, res, next)`` which implies that after some handling in this function a potential callback function can also be used (referenced with ``next``).

Eventually do not forget to export the required object within the module
````javascript
module.exports = router;
````

as the purpose is not only to "register' new users, we are also creating the following end poitns with, so far, very limited capability: only monitoring of proper connection.
````javascript
/**
 * authenticate the user to connect him to the back end
 * 
 */
router.post('/authenticate', (req, res, next) =>{
    res.send('AUTHENTICATE');
});

/**
 * provides the detail on the users profile
 */
router.get('/profile', (req, res, next)=>{
    res.send('PROFILE');
});

/**
 * next is to validate the jwt when receiving it
 */
router.get('/validate', (req, res, next)=>{
    res.send('VALIDATE');
});
````

#### Server and static files
the server is likely to serve some static files (might be the result of an angular app creation)

1. create the static folder into which the server will search for files
2. refer to it in the static object for server
````javascript
app.use(express.static(path.join(__dirname, 'public')));
````
add the above line before the reference to the body parser (though after the cors usage)

#### connection to database
the server will hook to a given database which connection settings are btw stored into a config file

a config file is a text file into which configuration properties are stored. Note this config file can be saved as ``json`` format

the config file is a plain javascript one. it only export an object with a properties created in plain text
````javascript
module.exports={
    database:'mongodb://localhost:27017/meanauth',
    secret:'asecret'
}
````
pay attention to not publish the secret on github ;)

this object is then refered to in the app.js file.
````javascript
const config = require('./config/database');
````
connection to the database it then straight forward as the structure of the database object which has been created is aligned with the expected one from the mongoose connector.
We also check the connection to database is effective in code
````javascript
mongoose.connect(config.database);
//detecton of connection
mongoose.connection.on('connected', ()=>{
    console.log('Connected to database is done'+config.database);
});
````
### USER MODEL & REGISTRATION
#### TODO ADD DETAILS ON MODELS (short introduction)

#### Creation of Model for users
Model describig what the user is is described in a unique user.js file. associated to that one will also implement some properties of it.
It is a good practice to structure your project files such that routes, models, configurations... are clearly appearing so that you can easily maintain and evolve your porject on the long run.
Another good practice is to systematically develop unit test and integration test.

let's then create a folder model
````
mkdir model
touch user.js
code user.js
````
``touch <name>`` creates a file named by the parameter given to the command.
Assuming VS Code is installed on your computer, then ``code <file>`` edits the file in VS Code.

User model will perform all operation related to user management in database; therefore we will use mongoose for connecting to the mongodb one.
also, since password and other pieces of information are to be encrypted, we will make use of bcryptjs.
````javascript
const mongoose = require('mongoose');
const bcrypt = require('bcryptjs');
const config = require('../config/database');
````
The definition of the User model is realized within a mongoose Schema.
Note the structure of schema name ``UserSchema`` which reminds us of complex type **definition**.
````javascript
/* user schema created */
/* user schema created */
const UserSchema = mongoose.Schema({
    name: {
        type: String
    },
    email: {
        type: String,
        required: true
    },
    username: {
        type: String,
        required: true
    },
    password: {
        type: String,
        required: true
    }
});
````
Each propertie is defined by its name, its type, its requireness and additional elements.
Since this definition is to be used accross the board, let's export it as a mongoose.Model
````javascript
/** let's export the User schema as a model definition known as User */
const User = module.exports = mongoose.model('User', UserSchema);
````
Then, the capabilities around user management are built one by one and exported from the module.
````javascript
module.exports.getUserById = function(id, callback){
    //does something smart such as
    User.findById(id, callback);
}
````
other function can be added
````javascript
/* this exported function allows to get user by its preferred username
*/
module.exports.getUserByUserName = function(username, callback){
    // creates a query which will look for an object which property username has the value username
    const query = {username: username};
    //note an error procedure should be managed here in case of disconnection, when the request doesn't find anything...
    User.findOne(query, callback);
}
````
once the ``User`` model is defined, one can easily use it into the Router for users. Declare the proper ``const`` in the users.js file
````javascript
/** import the definition of User */
const User = require('../models/user');
````
#### extension of the users.js (create actual behavior)

Let's first register our new user, observe that the register route is actually reached through a ``POST`` not through a ``GET`` which is meant to retrieve information from the server
````javascript
/**
 * register a new user within the database
 */
router.post('/register', (req, res, next)=>{
    let newUser = new User({
        name: req.body.name,
        email: req.body.email,
        username: req.body.username,
        password: req.body.password
    });
    User.addUser(newUser, (err, user)=>{
        if(err){
            res.json({success: false, msg: 'Failed to register user'});
        }
        else{
            res.json({success: true, msg: 'User registered'});
        }
    });
});
````
a new User is created from the information expelled from the body of the request (thanks to ``body-parser`` middleware). An object is created with the definition User
then one uses the function addUser which will perform the registration.
Note the error here is assumed to be a boolean where it could actually be something more elaborated.

##### addUser Function in the model
````javascript
/* this function adds a new user
@param user - the new user to add
@param callback function which has an error status and the reference to the user
*/
module.exports.addUser = function(newUser , callback){
    // let's hash the password: see documentation for bcryptjs
    bcrypt.genSalt(10, (err, salt)=>{
        bcrypt.hash(newUser.password, salt, (err, hash) =>{
            if(err) throw err; //brute force passing over of the error
            // replace password in newUser with the hashed version of it
            newUser.password = hash;
            newUser.save(callback);
        });
````
here we are mostly hashing the password with a salt then replacing the "cleared" password in the object with an hashed version of it.

#### USAGE OF WEBTOKEN
every password, by definition, has to be protected.
transaction between client side and server side or even between services are authenticated and therefore authorized or not with webtoken.
Passport and JSON Web Token are of this kind.
Both are middleware which we need to add on the server.
````javascript
/* body-parser for spelling out content of request */
app.use(bodyParser.json());

/* add Passport middleware */
app.use(passport.initialize());
app.use(passport.session());
````
Passport requires to select a strategy for token usage. 
using Passport-jwt, this is done within a config file which defines the way we are handling the token.

let's create a ``passport.js`` file within the config folder and apply our strategy in it.

````javascript
/**
 * the files contains the JWT strategy of our choice.
 * 
 */

const JwtStrategy = require('passport-jwt').Strategy;
const ExtractJwt = require('passport-jwt').ExtractJwt;
const User = require('../models/user');
const config = require('../config/database'); 
````
``JwtStrategy`` will be used to define the token strategy JsonWebToken.
``ExtractJwt`` will support the extraction rules for the token
we don't want to mess up directly with database in this element but, since the token will be used to identify the user, we need to know what a User is made of.
Eventually we bring in the ``config`` object to get to kwow the ``secret``

Then the strategy is to be exported outside this module.... ``module.exports..``

````javascript
/** passport.js */
module.exports = function(passport){
    //options for the strategy
    let opts = {};
    //where the jwt is to be taken from in the request (here we take it from the AuthHeader, could be Bearer..)
    opts.jwtFromRequest = ExtractJwt.fromAuthHeader();
    //secret for encryption of information
    opts.secretOrKey = config.secret;
    //new strategy is then created and used by passport
    passport.use(new JwtStrategy(opts , (jwt_payload, done)=>{
        // get the user from the database
        User.getUserById(jwt_payload_id, (err, user)=>{
            //if we get an error.... 
            if(err){
                return done(err, false);
            }
            //if we actually get an user
            if(user){
                return done(null, user);
            }else{
                //no error during the request... but user was not found
                return done (null, false);
            }
        })
    }))
};
````
here we assume the strategy to extract the token is from the header, this means therefore that once we have the token, it will have to be put within the authheade.
Jwt is created if ``user`` is found in the database, this is delegated to the database facade (actually the User model itself).

The definition of the strategy to be used is then realized within the server code after the installation of the middleware for passport and the creation for the session

````javascript
/** app.js */
/* add Passport middleware */
app.use(passport.initialize());
app.use(passport.session());

/** definition of the strategy to be used */
require('./config/passport')(passport);

````



#### AUTHENTICATE USERS
back to ``users.js`` where the authenticate method will now be implemented.
````javascript
router.post('/authenticate', (req, res, next) =>{
    //get username
    const username = req.body.username;
    //get (candidate) passport
    const password = req.body.password;
    // then let's try to get the user by its username
    User.getUserByUserName(username, (err , user )=>{
        // if there is an error, this one is passed over to the client
        //remark this is not ideal and shall be treated with a proper error management
        //TODO: treat errors properly
        if(err) throw err;
        if(!user){
            //it seems the requested user does not exist
            return res.json({success: false, msg: 'User does not exist'});
        }else{
            //if user exists in db, then the password is checked wrt what is stored
            User.comparePassword(password , user.password, (err , ismatch)=>{
                //TODO: treat errors properly
                if(err) throw error;
                //then depending on the result of the comparison jwtoken is generated... or not
                if(ismatch){
                    //Creation du token
                    // last parameter is bunch of option to be passed
                    let userT = user.toObject();
                    //remove the password from user data
                    delete userT.password;
                    //then generate the token from the userT object
                    const token = jwt.sign(userT, config.secret, { expiresIn: '1h' });

                    res.json({
                        sucess:true,
                        token: 'JWT '+token,
                        // user is recreated from the information available in db, but we don't send the object from the db as it has the password in it
                        user: {
                            id: user.id,
                            name: user.name,
                            username: user.username,
                            email: user.email
                        }
                    });
                }
                else{
                    //password is not valid
                    res.json({
                        sucess: false,
                        msg: 'Wrong password'
                    });
                }
            });
        }
    }
});
````
1st: recuperate the information from the request itself (username and password)
2nd: from username, search in db if the user does exist or not. In case he doesn't, then client is sent back a negative result with appropriate information
3rd: if user does exist, then we compare password and upon result of the comparison, it is decided to either proceed or not.
4th: proceeding is about generating the JWT for which plain object is required in payload (no string), to do so one has to be carefull not to add confidential data in the object (this shall have been done from the getuser... but if password was needed for comparison), in such case copy the object and remove the confidential information (password and anything we don't want to see handed over by whatever mean)

---
**__NOTES__**
1. Errors are caught and simply passed over (``throw err``) this is not acceptable. our code should be the one doing the error control and management.
2. Usage of ``TODO`` in code is interesting, but shall always be associated to a proper ticketing system referenced in the code.
3. messages for error are nice, but it is more efficient to have an error management system with clear code where all messages are centralized. (for internationalization, debug....)
4. having access to the secret all over the place is not safe. one shall centralize this.
---

##### ComparePassword (in user ...)
compare password is pretty simple as it has just an encapsulated ``bcrypt.compare(..)`` call but it is better to keep it within user as it limits access to db and centralized operation for user
````javascript
/**this function provides a password check mechanism
 * it receives a user and a candidate password
 * @param hash
 * @param candidatePwd
 * @callback function
 */
module.exports.comparePassword(canditatePwd, hash, callback){
    //though this can be put directly in the route, it is safer (more structured also) to keep it encapsulated
    bcrypt.compare(candidatePwd,hash, (err, isMatch)=>{
        if(err) throw err;
        callback(null, isMatch);
    });
}
````
---
**__NOTE__**
Documentation for method is to be added to support large development... tools are existing to support us with management of it also with generation of header, skeleton. they can be added to VSCode

## MORE DETAILS

### Usage of POSTMAN
Postman is a chrome extension (can also be a standalone version) which allows for the developper of API to test those and their answers.
the extension version has been deprecated years ago and now the standalone version can be downloaded from [here](https://www.postman.com/)

it can be used to test REST API
an alternative is RestEasy or even the command line Curl
