# How to setup [Shipyard](http://shipyard-project.com/)

Built on [Docker Swarm](https://docs.docker.com/swarm), Shipyard gives you the ability to
manage Docker resources including containers, images, private registries and more.

Shipyard differs from other management applications in that it promotes composability and
is 100% compatible with the Docker Remote API. Shipyard manages containers, images, nodes,
private registries cluster-wide as well as providing authentication and role based access control.

## Setup

~~~ sh
# on node-1
curl -sSL https://shipyard-project.com/deploy | bash -s

# on node-2
curl -sSL https://shipyard-project.com/deploy | ACTION=node DISCOVERY=etcd://46.101.245.190:4001 bash -s

# on node-3
curl -sSL https://shipyard-project.com/deploy | ACTION=node DISCOVERY=etcd://46.101.245.190:4001 bash -s
~~~

You should now be able to log into the Shipyard UI running on
[http://46.101.245.190:8080/](http://46.101.245.190:8080/) using the following default credentials:

Username: __admin__  
Password: __shipyard__

## Create a service key

In order to access Shipyard's API it's best to authenticate using a service key. The easiest way to create
a service key is using the [Shipyard command line container](https://hub.docker.com/r/shipyard/shipyard-cli/):

~~~ sh
docker run -it shipyard/shipyard-cli

# Type the command `shipyard` for further instructions...
~~~

The service key for our environment is: __DnqWOkAiUb7YKn6htbJk8RkB8auuJ6fIs1A2__
