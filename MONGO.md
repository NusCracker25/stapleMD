# MONGO

## WHAT IS IT
MongoDB is a document based database. very powerful and easy to use it is widely used in webapp to support the back capability of storage.

## SETUP

## HELLO WORLD

### setup mongodb
first download and install
then select the directory where data are to be stored

Then you need to specify set the dbpath to the created directory in mongod.exe. For the same, issue the following commands.

In the command prompt, navigate to the bin directory current in the MongoDB installation folder. Suppose my installation folder is ``C:\Program Files\MongoDB``²
````shell
C:\Users\XYZ>d:cd C:\Program Files\MongoDB\Server\4.2\bin
C:\Program Files\MongoDB\Server\4.2\bin>mongod.exe --dbpath "C:\data" 
````

### command line interface


## MORE DETAILS
### MEAN
see MEAN.md
### MERN

### LOOPBACK.IO

### NODE + EXPRESS
useof express and mongoose, stapauthentication

### REALTIME DATABASE
- in this section, we will see how to enrich a basic chat application with realtime database access and storage
all messages will be stored in a given collection
- we will also look at how to use authentication in transaction realized with the chat back-end

this is serving a greater purpose of "realtime app" with standard concurrent and collaborative CRUD capabilities



branch from chatapp


#### dependencies
we will use mongoose and socket.io on the server.js file
````shell
npm install --save mongoose socket.io
````

#### prerequisite on mongodb
read [here](https://hackernoon.com/using-mongodb-as-a-realtime-database-with-change-streams-213cba1dfc2a) what needs to be done on mongodb
````shell
mongod --replSet "rs"
````
or more specific if not using the default data path for storing db
````shell
mongod --dbpath <DATA_PATH> --replSet "rs"
````

then run the mongo client (command line) and initiate replicate on rs
````shell
eh@eh:~/Documents/mongodb-linux-x86_64-3.6.4$ bin/mongo
MongoDB shell version v3.6.4
connecting to: mongodb://127.0.0.1:27017
MongoDB server version: 3.6.4
...
> rs.initiate()
````