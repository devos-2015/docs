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

As this registry does not provide TLS, you have to configure all Docker deamons in order to access it:

* Edit the file /etc/default/docker so that there is a line that reads:
  `DOCKER_OPTS="--insecure-registry 46.101.193.82:5000"` (or add that to existing DOCKER_OPTS)
* Restart your Docker daemon: `service docker stop && service docker start`
