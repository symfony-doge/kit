
# Symfony Doge's Kit

This container-based kit (was initially used in the [Veslo](https://github.com/symfony-doge/veslo) project)
contains the following collection of configurations to manage data & processes of your web application:

- pgAdmin (https://www.pgadmin.org/) for [postgres](https://hub.docker.com/_/postgres).
- phpMyAdmin (https://www.phpmyadmin.net/) for [mysql](https://hub.docker.com/_/mysql).
- Redis Commander (https://joeferner.github.io/redis-commander/) for [redis](https://hub.docker.com/_/redis).

The kit is designed to provide access from your **personal machine** and supports configuration for managing both local
and remote services. It is focused on lightweight **web interfaces** rather than their desktop alternatives. 

## Installation

### Docker

You need to have a [Docker](https://docs.docker.com/install) daemon at least [17.05.0-ce](https://docs.docker.com/engine/release-notes/#17050-ce)
(with build-time `ARG` in `FROM`) and the [docker-compose](https://docs.docker.com/compose) tool to successfully cook all containers.

You can choose one of the out-of-box templates for your compose project:

|         | development     | production    |
| :------ | :------ | :------ |
|         | `docker-compose.dev.yml.dist` | `docker-compose.prod.yml.dist` |
| network | external, using the existing one by `APP_NETWORK_NAME` to access services | custom, using [SSH tunnels](https://github.com/symfony-doge/docker-ssh-tunnel) to production servers       |
| ports   | `5050` - pgAdmin<br />`6644` - phpMyAdmin<br />`7070` - Redis Commander | `5151` - pgAdmin<br />`6645` - phpMyAdmin<br />`7171` - Redis Commander |

Copy and adjust parameters for your environment: 

```
$ cp .env.dist .env
$ cp .docker-compose.dev.yml.dist docker-compose.yml
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

## Tips

### pgAdmin

For dashboard statistics in production (sessions, transactions per second and other stuff)
you need to setup an SSH tunnel directly in the server configuration menu* of your pgadmin web app (not docker). A quick solution can be:

1. Copy your `~/.ssh/id_rsa` into persistence directory of the compose project, for example, `data/pgadmin-ssh-import/id_rsa` and
set owning to `5050:5050` (check uid/gid of pgadmin user within docker container)

2. Add a volume mapping to `docker-compose.yml` like that:

```
...
    pgadmin:
        ...
        volumes:
            - ${PWD}/data/pgadmin-ssh-import/id_rsa:/pgadmin4/ssh-import/id_rsa:ro
            - pgadmin_data:/var/lib/pgadmin
```

\* — See also [servers.json](https://www.pgadmin.org/docs/pgadmin4/latest/export_import_servers.html) file for the pre-install stage.

## Changelog
All notable changes to this project will be documented in [CHANGELOG.md](CHANGELOG.md).