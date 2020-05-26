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
**MORE DETAILS**: can be found on cors [website](https://github.com/expressjs/cors) to fine configure it

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
mongoose.connect(config.database, {useNewUrlParser: true});
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
    opts.jwtFromRequest = ExtractJwt.fromAuthHeaderWithScheme('jwt');
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
##### More on JWT
[here](https://medium.com/@siddharthac6/json-web-token-jwt-the-right-way-of-implementing-with-node-js-65b8915d550e)
[snippets](https://www.codota.com/code/javascript/functions/jsonwebtoken/sign)
[or here](https://www.npmjs.com/package/jsonwebtoken)

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

#### Route protection (user the jwt)
with a mechanism now capable of identifying who is truly requesting something from server, one can easily protect access to data from the server itself.
This is done for each route.

````javascript
/**
 * provides the detail on the users profile
 */
router.get('/profile', passport.authenticate('jwt' , {session:false}), (req, res, next)=>{
    res.send('PROFILE');
});
````
to protect a route, the second parameter shall be used.
````javascript
... passport.authenticate('jwt' , {session:false}) ...
````
#### RESOURCES FOR BACK END
- [ ] error handler
https://www.freecodecamp.org/news/learn-how-to-handle-authentication-with-node-using-passport-js-4a56ed18e81e/


### FRONT END - ANGULAR
#### Introduction
the front end is 'what the user sees from the application'. It can be viewed in any webbrowser and as such is mostly HTML/CSS/JS built.
Angular is framework developped by Google, Inc. and made available to the community under the MIT license.
It has very large community of users and a release cycle of 6 months. By the time these lines are written, the activ version is Angular 9.
More details are given in our Angular.MD.
Angular is mostly geared towards SPA (Single Page Application), but this shall not be seen as a limitation.
Angular is Typescript based.

Our front will be build with the Material design, with support of themes and shall ultimately support internationalization.

#### First Step: Creation of the app

Angular has a Command Line Interface tool which supports most of the operation necessary for the creation of the app:
````
npm install -g @angular/cli
````
once installed one can check the version of it
````
ng version
````

##### Creation app
thanks to Angular CLI the initial steps for creating an app are simple
````
ng new <myapp> --routing --style=sass --prefix=<my_prefix>
````
Standard basic usage would be ``ng new <myapp>``: it creates a default app and its workspace

since we are likely to make it a bit more complex let's add a Routing module automatically with the option ``--routing``
the ``--style=sass`` option will serve down the road for the theming of our application.
Finally, let's define the prefix we want to use in our component per the app name for instance, this will help identifying easily what is a custom component.

Once the workspace and the application are created, using the ``ng serve`` or ``ng s`` command will compile (test if required) the code for the application, then create the appropriate webserver for testing purpose. Do open a webbrowser and navigate to   http://localhost:4200/.

##### Add Angular Material
the material design style is an interesting piece of work realized by the Google team as one of the most exhaustive guideline for User Experience and User Interface.
It has an angular compliant implementation (also created by Google's team): http://material.angular.io
It can be added to your app with the command 
````
ng add @angular/material
````
this command will need some more information before doing the proper setup. Answer them with your own preferences.

##### Add PWA support
although not mandatory, if you plan to make your MEAN app a progressive webapp, it is better to do it from the very begining
````
ng add @angular/pwa
````

#### settings for angular projet
compilation 'output directory' should be defined within the ``angular.json`` file where the settings for the project are to be found.

#### Structure for the Angular app
there exist many approaches for structuring the files and folder, the modules in Angular Application.
We follow the core/shared/feature approach.
Services are part of the code module.

Create the appropriate modules in the app
````
ng g m core
````
or 
````
ng generate module core
````
note the camel case notation

This creates a module as a submodule of the module where the command line is situated. It also registers the newly created modul

The ``core`` module embeds the core of the application, datamodel, where the business logic (client side) is expressed.
The ``shared`` module embeds the common pieces for all features (this is where reusable components will be created.)
The ``feature`` module has the expression of the workflow and user interaction capabilities.

The core and shared modules are for supporting cross concerns components or modules. As such, there is no need for adding a ``Routing`` module to them. On the other hand, the ``feature`` module needs a Router.

#### SHARED Module
The shared module will bring all the common modules such as the Material one (btw, this is good practice to have the material components to be used in a module as it saves the burden of importing them into each component you will create).

Other useful components are the Http one and http client.

#### TODO: add section on routing and preparing of the core of application
usage of navigation schematic and positionning of routers
pay attention to the address of the links to be used

#### TSLINT.json
the ``tslint.json`` file in your app folder contains some interesing settings for the compiler.
amongst them, it is quite useful to enrich the section ``compilerOPtions`` with a subsection ``paths``. This one shall contain the list of submodule in your application and will allow you to then use an easy reading description of your imports statement by referring to a module with the ``@`` symbol: e.g

````javascript
import { User} from '@core/users'
````
````json
{   
    "compileOnSave": false,
    "compilerOptions": {
        "baseUrl": "./",
        "outDir": "./dist/out-tsc",
        "sourceMap": true,
        "declaration": false,
        "downlevelIteration": true,
        "experimentalDecorators": true,
        "module": "esnext",
        "moduleResolution": "node",
        "importHelpers": true,
        "target": "es2015",
        "lib": [
          "es2018",
            "dom"
        ],
        "paths": {
            "@app/*": ["src/app/*"],
            "@feature/*" : ["src/app/feature"],
            "@core/*" : ["src/app/core"],
            "@shared/*" : ["src/app/shared"],
            "@environments/*": ["src/environments/*"]
        }
        ...
    }
}
````


#### TODO document the creation of the auth service
certainly have a subsection on observable at this place
more on security and JWT: [here](https://blog.angular-university.io/angular-jwt-authentication/) or [here](https://jasonwatmore.com/post/2020/04/19/angular-9-jwt-authentication-example-tutorial)


#### Usage of environements.js
within the your ``myapp\src`` folder, one can find a folder ``environments`` whithin which an ``environments.js`` file allows to add you specific environment variables
It contains a ``json`` structured object for you to add your app settings in:

````javascript
export const environment = {
  production: false,
  apiURL: 'http://localhost:3000'
};
````
Then for environment file can be imported when needed.

#### USAGE OF INTERCEPTORS FOR ADDING TOKEN
we'll create ann HTTP interceptor that will be used to attach the JWT access token to the authorization header of the ongoing requests. This interceptor is part of the core of our applicaiton. As such, have it created in the ``core`` module.

Create the src/app/auth.interceptor.ts file and add the following code:
````javascript

import { Injectable } from "@angular/core";
import { HttpInterceptor, HttpRequest, HttpHandler } from "@angular/common/http";
import { AuthService } from "./auth.service";

@Injectable()
export class AuthInterceptor implements HttpInterceptor {
    constructor(private authService: AuthService) { }

    intercept(req: HttpRequest<any>, next: HttpHandler) {
        const accessToken = this.authService.getAccessToken();
        req = req.clone({
            setHeaders: {
                Authorization: `JWT $[accessToken}` 
            }
        });
        return next.handle(req);
    }
}
````

The interceptor is an @Injectable object (which must be made available in the root of the application). It must have access the auth service (to provide the request with the appropriate ``access_token`` when this one is available). Eventually, the said token is added into the headers of the request.
This interceptor might be smarter to only consider request sent to authentication backend.

do not forget to add the interceptor into the core module.

````javascript
import { HttpClientModule, HTTP_INTERCEPTORS } from '@angular/common/http';
import { AuthInterceptor } from './auth.interceptor';
.
.
@NgModule({
    ...
    providers: [
        {
        provide: HTTP_INTERCEPTORS,
        useClass: AuthInterceptor,
        multi: true
        }
    ]
    ...
});
````
/
#### Enrich the behavior of the home page (user menu by hidding of showing items depending on the login status)
to start with, the AuthService shall be referenced in the 'home' component
bring it into the constructor
````javascript
constructor(... , private auth: AuthService, ...){}

````
#### MORE on MEAN
- https://jasonwatmore.com/post/2020/05/15/angular-9-role-based-authorization-tutorial-with-example
- https://www.techiediaries.com/angular-9-8-mean-stack-authentication-tutorial-and-example-with-node-and-mongodb/


#### More on Angular
- [home website](https://angular.io/) 
- [Angular CLI](https://cli.angular.io/) 
- [material](https://material.angular.io/)
  - [theming](https://medium.com/@tomastrajan/the-complete-guide-to-angular-material-themes-4d165a9d24d1)
  - [creating your theme](https://www.positronx.io/create-angular-material-8-custom-theme/)


## MORE DETAILS

### Usage of POSTMAN
Postman is a chrome extension (can also be a standalone version) which allows for the developper of API to test those and their answers.
the extension version has been deprecated years ago and now the standalone version can be downloaded from [here](https://www.postman.com/)

it can be used to test REST API
an alternative is RestEasy or even the command line Curl


### Adding logging capacity on back-end
Logging is about tracking what happens in your code.
It can be used for debug but also for maintainance purposes depending on where you are at with your development.
Very often, different level of logging are exposed (info, error, wanring, debug, verbose).
for maintainance purpose, logging requires to be searchable, indexable and is often associated to some elastic search capability
#### References
Among js library for logging, one can find
- [Winston](https://github.com/winstonjs/winston): a generic logging utility 
- [Morgan](https://github.com/expressjs/morgan): a looging utility for expressjs server. logs requests and routes for them
- also not that [ngx-logger](https://github.com/dbfannin/ngx-logger#readme) : logging mechanism for angular application (we'll come to that later on)
  
  #### using winston & Morgan
  a branch is created on the back-end of authentication for adding winston
  ````
  git branch chore_logWinston
  ````
  then switch to the said branch:
  ````
  git checkout chore_logWinston
  ```` 

A basic usage of winston is to output messages on the console.
A good practice is to identify a separate ``logger.js`` file. This one could be located wherever we want. Let's create this logger configuration within our ``config`` folder

````
npm install --save winston
````
will install the latest version of winston and save it to the ``package.json`` file.
Eventually in ``logger.js`` add the following lines

````javascript
const winston = require('winston');

//creation of the logger
// this logger sends all info level message to the console
const logger = winston.createLogger({
    level: 'debug',
    defaultMeta: { service: 'AuthLogger' },
    transports: [
        new winston.transports.Console({
             format: winston.format.combine(
                winston.format.timestamp(),
                winston.format.json(),
                winston.format.metadata({fillExcept: ['timestamp', 'service', 'level', 'message']}),
                winston.format.colorize()
           ) ,
           level: 'error'
       })
]
});

logger.info('this is an info');
logger.warn('this is a warning');
logger.error('this is an error log');
logger.debug('this is a debug log');
logger.log('info', 'this is another info log');

module.exports = logger;
````
This creates a simple logger with the name ``authLogger`` and a unique ``transports`` which the usual Console line.
It filters the level of log for this transports to ``error`` and below.
Every log is augmented with the time stamp.
The output format is set to json, more suitable for machine processing of logs when e.g. stored in database.
it can also be more human understandable with ``winston.format.json()``

Executing the server generates the output:
````
{"message":"this is an error log","level":"error","service":"AuthLogger"}
````
To use this logger within the server itself, extend the ``require`` section with 
````javascript
const logger = require('./config/logger');
````
then the logger can be used within the code with either
````javascript
 logger.info( 'Connected to database is done '+config.database);
````
or depending on how it is preferred to access each of the login level.
````javascript
logger.log('error','Database error: '+err);
````

It is then a good practice to configure the console application with log files or storage to a mongodb for instance.
This is achieve by defining the ``transports`` as ``File`` and specifying the ``filename`` propertie
````javascript
const logger = createLogger(
    ...
    transports: [...
           new winston.transports.File({
           filename: 'auth_info.log',
           level: 'info',
           format: winston.format.combine(
                winston.format.timestamp(), winston.format.json(),
                winston.format.metadata({fillExcept: ['timestamp', 'service', 'level', 'message']}),
                winston.format.colorize()//,
          // this.winstonConsoleFormat()
            )
        })
    ...
    ]
)
````
this adds a file transport which creates a file named ``authLog.log`` and outputs all log messages into it.

Last but not least, a mongodb can be used to persist the logs. A transport maintained by the community shall then added
````
npm install --save winston-mongodb 
````

then into the ``logger.js``, add a new transporter
````javascript
const winstonMdB = require('winston-mongodb');

...

       new winstonMdB.MongoDB({
           db:config.database,
           collection: config.logcollection,
           level: 'info',
           options: { 
               useUnifiedTopology: true
            },
           format: winston.format.combine(
                winston.format.timestamp(), winston.format.json(),
                winston.format.metadata({fillExcept: ['timestamp', 'service', 'level', 'message']}),
                winston.format.colorize()//,
          // this.winstonConsoleFormat()
            )
        })...
````
Note that the ``configs\config.js`` file has been modified into:
````javascript
module.exports={
    database:'mongodb://localhost:27017/meanauth',
    userscollection: 'users',
    logcollection: 'authlog',

    secret:'asecret'
}
````
#### more on winston
 - [zetcode](http://zetcode.com/javascript/winston/)
 - [Stackify](https://stackify.com/winston-logging-tutorial/)
 - [from zero to hero](https://gkoniaris.gr/nodejs/nodejs-logging-from-beginner-to-expert/)

##### Winston option
The createLogger function accepts the following options:

|Name	|Default	|Description
|---    |---        |---
|level	|info	|maximum level of log messages to log
|levels|	winston.config.npm.levels	|the set of level message types chosen
|format	|winston.format.json	|the format of log messages
|transports	|no transports	|set of logging destinations for log messages
|exitOnError	|true	|whether handled exceptions cause process.exit
|silent	|false	|if true, all logs are suppressed

##### Winston level of log (as provided in npm)
The following are the default npm logging levels:
and best practices to follow
|level log | usage | potentially exposed to user
|---        |---|---
|0: error | to log all situation where error is potentially blocking for user | YES
|1: warn| to log abnormal situation where one should not be while not preventing the execution | YES
|2: info| to log basic information informative of success of a function, evolution of program | YES
|3: http| to log http related messages | likely not
|4: verbose| everything about the program, likely used for profiling or to get very detailled information
|5: debug| used by dev team to trace program | certainly not
|6: silly| name speaks for itself | certainly not
