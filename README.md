## Overview

Deploys and starts PingFederate, PingAccess, PingDirectory and Console on your docker host to quickly get started with a Proof of Concept or Demo.

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

By default, your data will persist within the volumes/ directory, which is created during your initial startup.  To ensure your configuration is not overwritten during a restart, after your initial startup you will edit **docker-compose.yaml** and comment out the environment section of each product.  With these sections commented out, no server profile will overwrite your data.  Explained a different way:

1. Start your containers using `docker-compose up -d`
2. Comment out `environment` sections within **docker-compose.yaml**
3. Make any needed configuration changes
4. Stop and start your containers as needed
5. Repeat steps 3 & 4 as required without losing your data

## What You Get

## Making It Your Own
