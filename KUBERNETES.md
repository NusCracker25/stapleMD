# micro service
**pros**
replacement of big monolith application into smaller ones
separate business logic functions (e.g. user idenfiticaiot, db access)
communication between microservices is ensures through http
user interface can remain unique but triggers differents service on the background
advantage of http communication is that each microservice can be written in its own language. allows for team separation and concerns split
support fault isolation. having one service failing, does not block the whole application
architecture are highly scalable at runtime

**drawbacks** still:
complexity of the application. which presents a very complex network of services with many dependencies.
potential overhead in databases and servers.
need to have a container orchestrator.
dynamic create databases and server. manages containers, containerize applciation.

** docker ** is good for containers.
see docker from scratch as the example.
docker is light on the computer where it operates (unlike a VM)