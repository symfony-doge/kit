
# Symfony Doge's Kit

This container-based kit used in the [veslo](https://github.com/symfony-doge/veslo) project contains the following GUI tools
for managing data & processes of your web application:

- pgAdmin (https://www.pgadmin.org/) for [postgres](https://hub.docker.com/_/postgres).
- Redis Commander (https://joeferner.github.io/redis-commander/) for [redis](https://hub.docker.com/_/redis/).

## Installation

### Docker

You need to have a [Docker](https://docs.docker.com/install) daemon at least [17.05.0-ce](https://docs.docker.com/engine/release-notes/#17050-ce)
(with build-time `ARG` in `FROM`) and the [docker-compose](https://docs.docker.com/compose) tool to successfully cook all containers.

Copy and adjust parameters for your environment: 

```
$ cp .env.dist .env
$ cp .docker-compose.yml.dist docker-compose.yml
```

Ensure users inside docker containers have permissions to the data persistence directories.
Quick workaround can be (assume we are at the repository root on some testing server):

```
$ chmod o+w -R data/
```

Run the services through `docker-compose`:

```
$ docker-compose up -d
```

## Changelog
All notable changes to this project will be documented in [CHANGELOG.md](CHANGELOG.md).