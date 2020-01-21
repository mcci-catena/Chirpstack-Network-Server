# ChirpStack Docker example

This repository contains a skeleton to setup the [ChirpStack](https://www.chirpstack.io)
open-source LoRaWAN Network Server stack using [Docker Compose](https://docs.docker.com/compose/).

**Note:** Please use this `docker-compose.yml` file as a starting point for testing
but keep in mind that for production usage it might need modifications. 

## Directory layout

* `docker-compose.yml`: the docker-compose file containing the services
* `docker-compose-env.yml`: alternate docker-compose file using environment variables, can be run with the docker-compose `-f` flag
* `configuration/chirpstack*`: directory containing the ChirpStack configuration files, see:
    * https://www.chirpstack.io/gateway-bridge/install/config/
    * https://www.chirpstack.io/network-server/install/config/
    * https://www.chirpstack.io/application-server/install/config/
    * https://www.chirpstack.io/geolocation-server/install/config/
* `configuration/postgresql/initdb/`: directory containing PostgreSQL initialization scripts

## Configuration

The ChirpStack stack components are pre-configured to work with the provided
`docker-compose.yml` file and defaults to the IN865 LoRaWAN band. Please refer
to the `configuration/chirpstack-network-server/examples` directory for more configuration
examples.

## Data persistence

PostgreSQL and Redis data is persisted in Docker volumes, see the `docker-compose.yml`
`volumes` definition.

## Requirements

Before using this `docker-compose.yml` file, make sure you have [Docker](https://www.docker.com/community-edition) and [Docker-composse](https://docs.docker.com/compose/install/ )
installed.
## Build Setup

To build the docker-compose.yml file, there are some variables required in the `.env` file

### Create and edit the `.env` file

1. Edit the `.env` file as follows:

    1. `IOT_NETWORK_CHIRPSTACK_FQDN=myhost.example.com`
    It must be a fully-qualified domain name (FQDN) that resolves to the IP address of the container host.
    2. `IOT_NETWORK_DATA=./data/`
    The trailing slash is required!
   This will put all the data file for this instance as subdirectories of the specified path. If you leave this undefined, `docker-compose` will print error messages and quit.

### Generate a Let's Encrypt Standalone SSL Certs to secure Apps
```bash
certbot certonly --standalone -d IOT_NETWORK_CHIRPSTACK_FQDN> < --email <username@example.com>
```
To start the ChirpStack open-source LoRaWAN Network Server stack, simply run:

```bash
$ docker-compose up -d --build
```

**Note:** during the startup of services, it is normal to see the following errors:

* ping database error, will retry in 2s: dial tcp 172.20.0.4:5432: connect: connection refused
* ping database error, will retry in 2s: pq: the database system is starting up


After all the components have been initialized and started, you should be able
to open http://<IOT_NETWORK_CHIRPSTACK_FQDN>/ in your browser.

### Add Network Server

When adding the Network Server in the ChirpStack Application Server web-interface
(see [Network Servers](https://www.chirpstack.io/application-server/use/network-servers/)),
you must enter `chirpstack-network-server:8000` as the Network Server.
