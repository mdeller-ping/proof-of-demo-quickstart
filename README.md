## Overview

Deploys and starts PingFederate, PingAccess, PingDirectory and Console on your docker host to quickly get started with a Proof of Concept or Demo.

As configured, PingAccess will act as a reverse proxy to PingFederate (for both Runtime and Admin).

![Network Overview](https://github.com/mdeller-ping/proof-of-demo-quickstart/blob/master/Network%20Overview.png)

## Prerequisites

* [Docker](https://docs.docker.com/get-docker/)
* [Docker-Compose](https://docs.docker.com/compose/install/)
* [Ping Identity DevOps User and Key](https://pingidentity-devops.gitbook.io/devops/getstarted/devopsregistration)

## Getting Started

Update **devops** with your `PING_IDENTITY_DEVOPS_USER` and `PING_IDENTITY_DEVOPS_KEY`.  Also provide your response to `PING_IDENTITY_ACCEPT_EULA`.  I suggest **YES**, but make sure you review the terms for yourself.

## Starting First Time

```
docker-compose up -d
docker-compose logs -f
```

## Persisting Your Configuration

By default, your data will persist within the volumes/ directory, which is created during your initial startup.  To ensure your configuration is not overwritten during a restart, after your initial startup you will edit **docker-compose.yaml** and comment out the environment section of each product.  With these sections commented out, no server profile will overwrite your data.

Explained a different way:

1. Start your containers using `docker-compose up -d`
2. Comment out `environment` sections within **docker-compose.yaml**
3. Make any needed configuration changes
4. Stop and start your containers as needed
5. Repeat steps 3 & 4 as required without losing your data

```
services:
  pingfederate:
    image: pingidentity/pingfederate:10.0.2-alpine-edge
    command: wait-for pingdirectory:389 -t 600 -- entrypoint.sh start-server
    # environment:
      # - SERVER_PROFILE_URL=https://github.com/pingidentity/pingidentity-server-profiles.git
      # - SERVER_PROFILE_PATH=getting-started/pingfederate
    volumes:
      - ./volumes/pingfederate:/opt/out
    secrets:
      - devops-secret
    ports:
      - "8031:9031"
      - "8999:9999"
    networks:
      - pingnet
```

## Making It Your Own

Public APIs within each product can be leveraged to quickly customize the deployment.  Provided are three Postman collections to get you started.

### PingAccess

[Postman Collection](https://www.getpostman.com/collections/ad7419cdaa178a76e80f)

* Deletes Port 3000 engine listener, creates listeners on Ports 80 and 443
* Switches to Production ACME Servers from Let's Encrypt
* Creats a Keypair for both PingAccess and PingFederate
* Proxies requests to PingFederate, taking advantage of the trusted certificate

### PingFederate

[Postman Collection](https://www.getpostman.com/collections/57db4b3addf4693be1b8)

* Enabled OIDC Protocol, updates friendly hostname
* Builds PingDirectory Data Store, PCV, Login Form, Policy Contract, AuthN Policy and sample SAML Connection

### PingDirectory

[Postman Collection](https://www.getpostman.com/collections/7c6234cfd5a61ad41d94)

* Creates sample user (michael@example.com / 2FederateM0re)

## Too Long, Didn't Read (TLDR)

Assuming that we are on a Centos Host and I have created 3 DNS CNAMEs that point to my host:

```
git clone https://github.com/mdeller-ping/proof-of-demo-quickstart
cd proof-of-demo-quickstart
vi devops
sudo yum install -y docker docker-compose
sudo service docker start
sudo docker-compose up -d
```

Run the three Postman Collections
