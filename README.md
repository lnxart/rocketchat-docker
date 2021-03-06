# rocketchat-docker
The Rocketchat docker implementation with dynamic scaling.

## Getting Started

These instructions will get you a copy of the project up and running on your local machine for development and testing purposes. See deployment for notes on how to deploy the project on a live system.

## Prerequisites

Install Docker including docker-compose support.

## Installation

1. Clone this repository:
```
$ git clone https://github.com/lnxart/rocketchat-docker
$ cd rocketchat-docker
```
2. Edit the env var VIRTUAL_HOST in `docker-compose.yml` file to point to your domain. 

3. Create and start up containers using docker-compose:
```
$ docker-compose up -d
```

(It takes some minutes to prepare the containers.)

## Usage
### How to connect to [Rocket.Chat](https://rocket.chat)

This project comes with a dynamic load-balancer container which is exposed on port 8080. This load-balancer manages the traffic between our application containers, no matter how many we scale up/down.

In production, you probably still want to use the default HTTP/HTTPS ports, right? To do that simply add your certificates to Nginx reverse proxy to terminate your SSL connections.

### Scaling application dynamically

This service file supports the docker-compose builtin scaling. For example to add 1 additional rocketchat containers you can simply invoke:
```
$ docker-compose scale rocketchat=2
Starting project_rocketchat_1 ... done
Creating project_rocketchat_2 ... done
```
The Nginx reverse proxy will configure automatically and add the new container to backend servers (for further reading refer to [link](https://github.com/jwilder/nginx-proxy)).

You can test the functionality by stop the first rocketchat container and then continue to use the application in your web browser.

Also, you can scale down the rocketchat containers when your loads decrease simply by:
```
$ docker-compose scale rocketchat=1
Stopping and removing project_rocketchat_2 ... done
```
Also, there are some other solutions for load balancing the rocketchat like [Traefik](https://docs.traefik.io/) that explained in [this](https://github.com/frdmn/docker-rocketchat) project. It integrates with your existing infrastructure components (Docker, Swarm mode, Kubernetes, Marathon, Consul, Etcd, Rancher, Amazon ECS, ...) and configures itself automatically and dynamically.

### MongoDB

#### Replica set?

You probably already noticed the mongo-init-replica container. It is necessary to create the replica set in your MongoDB container and executed only once when you spin up the docker-compose.yml file initially. The replica set is necessary to run Rocket.Chat across several instances. (see [Scaling](https://github.com/lnxart/rocketchat-docker#scaling-application-dynamically))

## Requirements / Dependencies

- Docker

## Version

- 1.0.0

## Acknowledgments

* Thanks to @jwilder and @frdmn
