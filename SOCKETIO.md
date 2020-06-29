# Socket.io

[Home page](https://socket.io/)

## What is it it
socket.io is a javascript websocket wrapper.

socket.io describes itself as a real-time bidirectional and event based communication framework.
Very useful for pushing data to client in real time (counter, charts....)
It is great for chat application and enables also concurrent edition of document

Alternatives can be found [here](https://blog.bitsrc.io/8-node-js-web-socket-libraries-for-2018-818e7e5b67cf)

And nice tutorial are 
- [traversy](https://www.youtube.com/watch?v=jD7FnbI76Hg) chat app ith users and room (express, node and socket.io)
- [traversy](https://www.youtube.com/watch?v=MZ6wMonyVyY) polling app, mongo and mongoose
- [extended chat app](https://www.youtube.com/watch?v=oTTOyiwTO2Y&list=PLrwNNiB6YOA1a0_xXvogmvSHrLcanVKkF)
- [with react](https://www.youtube.com/watch?v=ZwFA3YMfkoc) or [here](https://www.youtube.com/watch?v=iKGPXkMcbbY&list=PLYPFxrXyK0ByCS-KG6BZYEoXOkRugZuLD)
- [no video](https://www.tutorialspoint.com/socket.io/index.htm)
- [with mongo](https://www.youtube.com/watch?v=8Y6mWhcdSUM) or [here](https://www.youtube.com/watch?v=MZ6wMonyVyY)
- [with angular](https://vexxhost.com/resources/tutorials/mean-socket-io-integration-tutorial/)
- https://www.youtube.com/watch?v=SZcY8cB8nhQ (something else than a chat)
- 
## get to know more
so as to get to know more about it, let's create a small chat capability in ``Stapler``

Create the project and configure git to get connected to github
````powershell 
git init
git config --local user.email '...'
git config --local user.name '...'
git remote add origin '...'
git pull origin master

npm init
````
then bring the dependencies in
- express: as the perfect back end server
- socket.io: for realtime exchanges
- moment: which formats dates

````powershell
npm install --save express socket.io moment
````
add a ``.gitignore`` file which specifies to ignore the node_modules folder

````powershell
.gitignore
.log
#ignore logs folder
/logs
#ignore all node modules
/node_modules
````

add logging support to the application with winston
````powershell
npm install --save winston morgan app-root-path
````

to support development install also ``nodemon`` as a dev dependency
````
npm instal -D nodemon
````
finishing the preparation of dev environement, in package.json change the script session with 
````json
...
  "scripts": {
    "start": "node server",
    "dev" : "nodemon server"
  }
  ...
````
with these lines, one can now use the ``npm start`` or ``npm dev`` command lines.

Initial setup
have a ``config`` folder created into which a ``config.js`` file is created.

```config.js``
  ````javascript
  module.exports={
      server:{
          port: 3000
      }
      logging:{
          store: {
              db: false
          },
          folder : 'logs'
      }
  }
  ````
Likewise, get a ``logger.js`` file created with as in 

### Creation of the server
using express, the app we create for the socket is a slightly different code as we need to register the socket in the server itself.

hence, the code is different: instead of having the ``app`` to ``listen`` on the port, this the server.
````javascript
  /**
   * creation of server
   */

  const app = express();
  // we need to actually access the server itself for socket.io to access it
  const server = http.createServer(app);
  // create of io connect
...
 /**
  * launches the server
  */
  server.listen(config.server.PORT, () =>{
      logger.log('info', 'Chat server started on port '+config.PORT);
  });
````
thanks to the hand we now have on the server itself, one can register the socket in it

````javascript
 // we need to actually access the server itself for socket.io to access it
const server = http.createServer(app);
// create of io connect
const io = socketio(server);
````

the require section of our code is as follow
````javascript
/**
 * requires statement
 */
const express = require('express');
const root = require('app-root-path');
const path = require(`path`);
const http = require(`http`);
const socketio = require(`socket.io`);
/**
 * settings and config
 */
const config = require('./config/config');
const logger = require('./config/logger');
````
we are using winston as a logger, it is configure in our ``./config/logger.js``

### dealing with messages on server
a websocket is a preferred channel opened between two part of the application (unlike http request it is bidirectionnal).

websocket allows for ``1 to 1`` (with ``emit``) or one to many (``broadcast``).
upon connection of a new user, let's welcome the new user and broadcast to all that someone is joining.
likewise upon disconnection of the user, let's inform everyone that somebody left.

In ``server.js``, before launching the server
````javascript
/**
 * creates the connection and listens to these event
 */
io.on('connection', socket=>{
    logger.log('info', 'New WS connection...');
    // send message to newly connected user
    socket.emit('message', 'Welcome to chat!');
    //broadcast to everyone but the new comer that someone new has connected
    socket.broadcast.emit('message' , 'A new user has joined the chat');

    //on disconnection, we let e
    socket.on('disconnect', ()=>{
        io.emit('message', 'A user has left the chat');
    })
});
````
### dealing with messages on client side
on the client side, one must listen to everything
so a ``main.js`` script file is added to the html (for test) page.

in html page though pay attention to include the socket.io.js file.

````html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
        <link rel="stylesheet" href="css/style.css" />
    <title>Chat</title>
  </head>
  <body>
   <script
      src="https://cdnjs.cloudflare.com/ajax/libs/qs/6.9.2/qs.min.js"
    ></script>
    <script src="/socket.io/socket.io.js"></script>
    <script src="js/main.js"></script>
  </body>
</html>
````

receiving the message on the client side and then emitting a new one
````javascript
const socket = io();

socket.on('message', message =>{
  console.log(message);
})
...
// capturing the message to be sent to the server as 'msg'
socket.emit('chatMessage', msg);
...
````

### receives message from the server
the message emitted by the client is of nature ``chatMessage``, this one is then captured and treated on the server 
````javascript
// receives message from client
    socket.on('chatMessage', (msg)=>{
       // message is sent to everynody (including the emitting user)
        io.emit('message',)
    })
````

### more elaborated message with objects
up to now, we've been passing along ``string`` only as messages. This can be more educated and we could have objects instead.
Structure of the object can be determined within their specific folder ``utils`` .

so in ``app_root`` folder create an ``utils`` folder and in it have a ``messages.js`` created, in it a method will structure information into a js object
(using the ``moment`` package to get the proper format for time)
````javascript
const moment = require('moment');

function formatMessage(username, text){
    return {
        username,
        text,
        time: moment().format('h:mm a')
    }
}

module.exports = formatMessage;
````
Then everywhere a message is sent, replace the string by the formatMessage call.

on the client side, one can emit message with more structure in it as well. For instance when a user joins a specific room, one needs to send over to the server the proper information
``client.js``
````javascript
const socket = io();

// Join chatroom
socket.emit('joinRoom', { username, room });
````

the proper event is then caught onto the server 
``server.js``, better to catch this as the first thing
````javascript
/**
 * creates the connection and listens to these event
 */
io.on('connection', socket=>{
    logger.log('info', 'New WS connection...');
    // user joins a specific room
    socket.on('joinRoom' , ({username, room}) =>{
// DO SOMETHING SMART
    });
````
### rooms (or group)

## with Angular and Rsjs
reference: [here](https://www.techiediaries.com/angular-9-realtime-chat-example-with-nodejs-socketio-and-websocket/)