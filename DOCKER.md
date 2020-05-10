# DOCKER
Website: https://www.docker.com/products/docker-desktop 
is a container managment system
it does replace VM though it helps in agnostizing the behavior of application from the actual OS they are deployed.
Containers are processes running on the machine and they are sharing the kernel of the OS.

download the latest stable installer and get it installed this comes easy.

Open a command window and type in the command:
```
docker
```
it shows of the commands available.
to get to know more visit
[pdf cheatsheet]( https://www.docker.com/sites/default/files/d8/2019-09/docker-cheat-sheet.pdf) or [here](https://github.com/wsargent/docker-cheat-sheet)

## First commands and their usage
Prior to any action involving an access to the dockerhub, use the command
````
docker login
````

```
docker container run -it -publish 80:80 <name of the image>
```
runs a container in the foregroudn (interactive)
publish option allows to identify to which port of the physical machine the said port of the container is to be mapped
if the required image is not found locally, then docker connects to dockerhub (a kind of github of docker container to find it: http://hub.docker.com)

Ctrl+C allows to stop a runnig container

````
docker container ls
````
returns the current status of running containers on the machine

````
docker container rm <id>
````
deletes the referenced container from the container on the system (watch out it only removes them from the list of available container on the system). It does not delete the corresponding files from the system.
should you need to do so then use the command
````
docker images
````
to see what are the images on the system
````
docker image rm <id>
````
to actually delete the image <id> from the system

````
docker pull somthg
````
pulls an image from docker hub on the system

````
docker container -d -p 8080:80 --name truc img 

docker contaner -detach -publish --name truc 8080:80 img
````
are equivalently creating an instance of img but detached, running in the background (output on cmd line will not be seen)
the instance will be known as truc

````
docker ps
docker container ls
````
are listing the running containers

Environment variable of the container

````
docker container run -d -p 3306:3306 --name mysql --env MYSQL_ROOT_PASSWORD=123456 mysql
````
creates an image of mysql with the name mysql  which port 3306 is mapped onto port 3306 and  environment variable MYSQL_ROOT_PASSWORD is set to 123456
````
docker container stop mysql
````
stops a the containe mysql

remove all containers
````
docker rm $(docker ps -aq) -f
````
removes a list of container obtained thanks to a command listing all of them


## Edition of files within a container
one can bash within a container
```
    docker container exec -it container_id command_line
```
 a part of the container can be "mapped" to a folder into local system

 create a container with a bind mount
 ````
 docker container run -d -p 8080:80 -v $(pwd):/usr/... -name a_name image
 ````
`-v`  creates a binded folder to the first parameter (here references to the current folder in local system) second parameter after the colon.
then all modification done in the local folder will be mirrored within the instance of the container

creation of a new docker image

create a file Dockerfile

````docker
FROM nginx:latest
WORKDIR /usr/blablabl
COPY . .
````
creates a new image from nginx latest reference
nagivates into the usr/blabla, then copy all the files from within the folder the Dockerfile is in into the /usr/blabla folder of the image

````
docker image build -t dockerusername/a_name .
````
does then create the new image and upload it publishes it onto the dockerhub
-t is for defining name

then the image is available locally for instanciation

push an image to dockerhub
````
docker login
docker push myimage
````
docker login allows to log into docker hub

## Usage of Docker Compose
one can create a constellation of services interconnected to each still maintained and developped separately

in the ongoing example, the appication (serverside) uses node, experss, ejs (for the presentation layer) and mongoose to access a mongo db, database

solution is decomposed into two services:
1. for the presentation layer (based on NodeJS)
2. the second one is having a mongodb database with appropriate exposed port

application running on node can be created from an docker image that is stable, hence instead of using the tag `latest` in the  docker file, one can refer to a specific version of the image
(node:10 instead of `node:latest`)
````docker
FROM node:10

WORKDIR /usr/src/app

COPY package.json ./

RUN npm install

COPY . .

EXPOSE 3000

CMD ["npm" , "start"]
````
from the 10th version of nodejs docker image, move to directory usr/src/app and copy the file `package.json` from local machine to the image folder.  
then run the `npm install` command  
Copy all what is inside the current folder of the machine within the selected folder of the image  
exposes the port 3000 (the one selected in the app, note that this could have been an environment variable set from within the docker command line)  
eventually execute the Command (npm start): list of argument are given as an array of strings
this application could be built (one could create a container image from here) (not mandatory when using the docker compose)

Next to the docker file, a compose file shall be created

docker-compose.yml (pay attentio indentation is important) 
````yaml
version: `3`
services:
    app:
        container_name: docker-node-mongo
        restart: always
        build: .
        ports:
            - '80:3000'
        link:
            - mongo
    mongo:
        container_name: mongo
        image: mongo
        ports:
            - `27017:27017`

````
pay attention to indentation. All services shall be on the same level.


creates a list of services:
1 for app
    container name is arbitrary
    one wants the service to automatically restart in case of failure
    shall be instantiated from the docker file found in the current directory. note the image is not built here, rather instantiated on the fly from the existing docker file
    links creates the link to the mongo container

1 for the database 
    (here name chosen is mongo, could have been db... or anything). The image is taken from dockerhub (image:mongo) 
Note: the links option in first service, indicates that the mongo service is know as "mongo" in the app service. (this seems to act as some king of local DNS)



.dockerignore
    allows for ignoring files, and so. pretty much the same role and purposes as the gitignore file
````
node_modules
npm-debug.log
````



Once the docker compose file is created, one can run the compose file
````cmd
docker-compose up
````
to run in background
````
docker-compose up -d
````
to stop the constellation
````
docker-compose down
````