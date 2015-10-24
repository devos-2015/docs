# How to setup [Consul](https://consul.io/)

Consul makes it simple for services to register themselves and to discover
other services via a DNS or HTTP interface. Register external services such
as SaaS providers as well.

Pairing service discovery with health checking prevents routing requests to
unhealthy hosts and enables services to easily provide circuit breakers.

## Setup

~~~ sh
# on node-1
docker run -d -h node-1 -v /data/consul/data:/data \
    --name consul-server \
    --restart=always \
    -p 46.101.245.190:8300:8300 \
    -p 46.101.245.190:8301:8301 \
    -p 46.101.245.190:8301:8301/udp \
    -p 46.101.245.190:8302:8302 \
    -p 46.101.245.190:8302:8302/udp \
    -p 46.101.245.190:8400:8400 \
    -p 46.101.245.190:8500:8500 \
    -p 172.17.42.1:53:53/udp \
    -e SERVICE_IGNORE=true \
    46.101.193.82:5000/consul-cors:latest -server -advertise 46.101.245.190 -bootstrap-expect 3

# on node-2
docker run -d -h node-2 -v /data/consul/data:/data  \
    --name consul-server \
    --restart=always \
    -p 46.101.132.55:8300:8300 \
    -p 46.101.132.55:8301:8301 \
    -p 46.101.132.55:8301:8301/udp \
    -p 46.101.132.55:8302:8302 \
    -p 46.101.132.55:8302:8302/udp \
    -p 46.101.132.55:8400:8400 \
    -p 46.101.132.55:8500:8500 \
    -p 172.17.42.1:53:53/udp \
    -e SERVICE_IGNORE=true \
    46.101.193.82:5000/consul-cors:latest -server -advertise 46.101.132.55 -join 46.101.245.190

# on node-3
docker run -d -h node-3 -v /data/consul/data:/data  \
    --name consul-server \
    --restart=always \
    -p 46.101.193.82:8300:8300 \
    -p 46.101.193.82:8301:8301 \
    -p 46.101.193.82:8301:8301/udp \
    -p 46.101.193.82:8302:8302 \
    -p 46.101.193.82:8302:8302/udp \
    -p 46.101.193.82:8400:8400 \
    -p 46.101.193.82:8500:8500 \
    -p 172.17.42.1:53:53/udp \
    -e SERVICE_IGNORE=true \
    46.101.193.82:5000/consul-cors:latest -server -advertise 46.101.193.82 -join 46.101.245.190
~~~

You should now be able to access the Consul UI using one of the following URLs:

* [http://46.101.245.190:8500/](http://46.101.245.190:8500/)
* [http://46.101.132.55:8500/](http://46.101.132.55:8500/)
* [http://46.101.193.82:8500/](http://46.101.193.82:8500/)
