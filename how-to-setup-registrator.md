# How to setup [Registrator](http://gliderlabs.com/registrator/latest/)

Registrator automatically registers and deregisters services for any Docker container
by inspecting containers as they come online. Registrator supports pluggable service
registries, which currently includes [Consul](http://www.consul.io/),
[etcd](https://github.com/coreos/etcd) and [SkyDNS 2](https://github.com/skynetservices/skydns/).

## Setup

~~~ sh
# on node-1
docker run -d -h node-1 \
    --name=registrator \
    --restart=always \
    --volume=/var/run/docker.sock:/tmp/docker.sock \
    gliderlabs/registrator:latest -ip 46.101.245.190 consul://46.101.245.190:8500

# on node-2
docker run -d -h node-1 \
    --name=registrator \
    --restart=always \
    --volume=/var/run/docker.sock:/tmp/docker.sock \
    gliderlabs/registrator:latest -ip 46.101.132.55 consul://46.101.132.55:8500

# on node-3
docker run -d -h node-1 \
    --name=registrator \
    --restart=always \
    --volume=/var/run/docker.sock:/tmp/docker.sock \
    gliderlabs/registrator:latest -ip 46.101.193.82 consul://46.101.193.82:8500
~~~
