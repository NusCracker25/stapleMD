# LOOPBACK.io

## what is it?
Loopback is a powerful REST API generator which is highly configurable and allows for multi database storage management

[Home Page](https://loopback.io/)

## how to install
with the Node Package Manager ``npm`` tool, it is recommended to install it as ``global``
````ps
npm install -g @loopback/cli
````

## Creation of the app

The command will create a folder into which the files associated to the app will be created.

simply enter
````ps
lb4
````
Answering the question will vreate a new application.

A standard loopback application is composed of 4 main artifacts:
- the Model: it describes what the object is made of. this are his properties
- the datasource: which controls how data are persisted on the server side
- the repository: is a service that proved strong-typed access to data and operation on a domain model: it typically perfoms the CRUD operation for a given Model into a given repository
- the controller: this is where the magic happens. It stitches together all of the above and exposes the endpoints of our API

The Lb4 command followed by the appropriate keyword will help in defining each of them
````ps
lb4 model

lb4 datasource

lb4 repository

lb4 controller
````
Then, with ``npm start`` one will start the server and made operationnal all of the point:
once a server is up and running, on can navigate to the [API End point Explorer](http://127.0.0.1:3000/api/explorer) (pay attention to the address default port is 3000 but it can be modified)