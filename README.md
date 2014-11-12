# Big Data for Chimps Example Code

## Generating Docker containers


Clusters are defined using [decking](http://decking.io), a Node.js tool to create, manage and run clusters of Docker containers.

### Prerequisites

* Docker
* Boot2Docker, if you're on OSX
* Ruby with rake
* Node with npm

### Install decking

Install :

```
( cd vendor/decking && npm install -g decking )
```

You must use that version -- others will not work (lack hostname option)

### If you're running under boot2docker

You'll probably want to forward the hadoop ports:

```
boot2docker down
rake docker:open_ports
boot2docker up
```

When you run `boot2docker up` and check that you have the env variable set that it says to 
For me, it's `export DOCKER_HOST=tcp://192.168.59.103:2375`

### Pre-seed the base images

```
docker pull phusion/baseimage:latest
docker pull blalor/docker-hosts:latest
```

### Start the helper cluster

```
rake docker:helpers
```

### Minor setup needed on the docker host

```
boot2docker ssh
mkdir -p /tmp/deb_proxy /tmp/bulk/hadoop
touch		    /var/lib/docker/hosts
chmod 0644	    /var/lib/docker/hosts
chown nobody:nobody /var/lib/docker/hosts
```

### Build the images

```
rake docker:build
```

Then create the cluster:

```
rake cluster:create
```

### Start the thing:

```
rake cluster:start
```

