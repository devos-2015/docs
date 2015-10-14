# How to setup a [private Docker registry](https://docs.docker.com/registry/)

The Registry is a stateless, highly scalable server side application that stores
and lets you distribute Docker images. The Registry is open-source, under the
permissive [Apache license](http://en.wikipedia.org/wiki/Apache_License).

## Setup

~~~ sh
# on node-3
docker run -d -h node-3 \
    --name=registry \
    --restart=always \
    -p 5000:5000 \
    registry
~~~

## Access

As this registry does not provide TLS, you have to configure all Docker deamons to
not refuse the connection.

If you are running Docker as a deamon on a native Linux machine, execute the following steps:

* Add the following line to the file /etc/default/docker:
  `DOCKER_OPTS="$DOCKER_OPTS --insecure-registry 46.101.193.82:5000"`
* Restart your Docker daemon: `service docker stop && service docker start`

__NOTE:__ If you are running boot2docker, the steps to edit the profile would be:

* Run `boot2docker ssh -t sudo vi /var/lib/boot2docker/profile` to open the editor
* For older versions of boot2docker you have to add the line `DOCKER_OPTS="$DOCKER_OPTS --insecure-registry 46.101.193.82:5000"`
* For newer versions of boot2docker you have to add the line `EXTRA_ARGS="$EXTRA_ARGS --insecure-registry 46.101.193.82:5000"`
* If you are not sure, it probably doesn't hurt to add both lines ;-)
* Save the file and exit the SSH connection
* Back on your VM host (your 'real' Windows/Mac box), run `boot2docker down` and then `boot2docker up`

## Frontend

This command deploys a web frontend for the registry on node-2:

~~~ sh
# on node-2
docker run -d -h node-2 \
    --name=registry-frontend \
    --restart=always \
    -e ENV_DOCKER_REGISTRY_HOST=46.101.193.82 \
    -e ENV_DOCKER_REGISTRY_PORT=5000 \
    -p 8060:80 \
    konradkleine/docker-registry-frontend
~~~
