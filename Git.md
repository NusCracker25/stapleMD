# GIT
## WHAT THIS IS
a versionning file system largely used in code development
GitHUB acquired by MSFT in 2018 is widely used in the open source community

it relies on the notion of branches.

This is a client/Server kind of code management where a repository hosts the unique source of truth. interestingly, the client side allows for branches creation as well.

## WHERE to get
got to https://git-scm.com/
where the installer can be downloaded from 

note in linux
````
sudo apt-get install git
````

## BASIC COMMANDS

### to configure locally
git --version
shows the current verison of the client
````
git config --global user.name `your name`
````
to configure globally the user.name for git
note this can also apply to the local folder only

````
git config --local ...
````
will store the configuration into the git file of the current workspace


### working with local repository
````
git init
````
git init: initialize a local repostiroy
a bunch hidden files and folder are created, so take care so show hidden files.

````
git add *
````
git add <file> add file (s) to the index

git add *.html adds all html file to the repository

````
git rm file
````
removes the referenced file from the list of observed ones

````
git status
````
check the status of the working tree

```` 
git commit
````
 commit changes to the index
note this will open a vim editor which can be used for editing the commit messages

````
git commit -m "here your commit message"
````
commit the changes with the commit message as expressed above

### working with remote repository
````
git push
````
push the local repository to remote repository
(remote service will have to added and secures)

````
git pull
````
get the latest from the remote repository

````
git clone
````
clones the repository into a new directory

````
git branch name
````
creates the branch (local) with the name <name>. it is recommended to define some naming convention for branches


### usage of the gitignore
in this file, one can list out all the files/folders which we never want to include
If its content is
````
error.logs
````
then the error.logs file will not be added and therefore not be monitored.
this is pretty usefull when using 'compiling' engines or templating ones. In such situation, only the source code is to be monitored.

## CODING ENVIRONMENT

### VISUAL STUDIO CODE
plug in: GitLens GitHD (*****)

### DockerHub
a reference one shall exist with a repository configured on it.