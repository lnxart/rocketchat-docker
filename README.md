# rocketchat-docker
The Rocketchat docker implementatoin with dynamic scaling.

## Getting Started

These instructions will get you a copy of the project up and running on your local machine for development and testing purposes. See deployment for notes on how to deploy the project on a live system.

### Prerequisites

Install Docker including docker-compose support.

### Installation

1. Clone this repository:
```
$ git clone https://github.com/lnxart/rocketchat-docker
$ cd /rocketchat-docker
```
2. Create and start up containers using docker-compose:
```
$ docker-compose up -d
```
### Usage
#### How to connect to Rocketchat

This project comes with a dynamic load balancer container which is exposed on port 8080. This load balancer manages the traffic between our application containers, no matter how many we scale up/down.

In production you probably still want to use the default HTTP/HTTPS ports, right? To do that simply add your certificates to nginx reverse proxy to terminate your SSL connections.

#### Upgrade to a new Rocket.Chat version

To update your Rocket.Chat server you simply need to make sure the docker-compose.yml reflects the version you're trying to update to, pull the new image from Docker hub, stop and destroy your existing application container and recreate them:
```
git pull
docker-compose pull rocketchat
docker-compose stop rocketchat
docker-compose rm rocketchat
docker-compose up -d rocketchat
```
#### Scaling in case of performance issues

This service file supports the docker-compose builtin scaling. For example to add 1 additional rocketchat containers you can simply invoke:
```
$ docker-compose scale rocketchat=2
Creating project_rocketchat_2 ... done
```
The nginx reverse proxy will configure automatically and add the new container to backend servers.
You can test the functionality by stop the first rocketchat container and then continue to use the application in your web browser.
Also you can scale down the rocketchat containers when your loads decrease simply by:
```
$ docker-compose scale rocketchat=1
Stopping and removing project_rocketchat_2 ... done
```
