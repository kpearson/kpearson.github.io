Docker Swarm
============

Using and maintaining Docker swarm requires a few key concepts which drives
how tools are used. In a Docker swarm it's important to keep in mind that
connecting via ssh to a single instance gives you access to info about the
cluster but it only gives a partial view of the requests coming into to the
swarm.

My goal here is to go over some use full tools for looking into and maintaining
a Docker swarm. The tool is the docker-cli and some of it's sub-commands.

What I go over below assumes you have shell access to at least one of the
servers the swarm is running on. In the examples below I'm ssh-ed into an AWS
instance.

## Terminology

* __Swarm__  
A swarm is a cluster of one or more Docker Engines running in swarm mode.
* __Service__  
The definition of how you want to run your application containers in a swarm.
* __Stack__  
A composed group of services.
* __Task__  
Individual running instances of a service. A stack may have many replicas of a
given service for purposes like load balancing and failover.

## Connect to Docker

List running docker instances

```shell
docker ps
```

Create an interactive shell into a running Docker instance

```shell
docker exec -it {docker-id or name}
```

## Working with the swarm

List services

```shell
docker service ls
```

List the __tasks__ of one or more services

```shell
docker service ps {srevice_name}
```

List stacks

```shell
docker stack ls
```

List the tasks in the stack

```shell
docker stack ps {stack}
```

> With `docker stack ps` and `docker service ps` you'll see the current tasks
in use as well as a log of previous images. Previously used images are
removed after a rolling update.

Scale up or down the tasks of a service

```shell
docker service scale {service_name}=4 --detach=false
```

> Using the `detach=false` option lets us see the result.

