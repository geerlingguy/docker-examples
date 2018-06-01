# Traefik proxy in front of web app

## Quick Start

  1. Bring up the Traefik proxy: `docker-compose -f traefik.yml up -d`
  2. Bring up an example web app: `docker-compose up -d`
  3. Edit your `/etc/hosts` file and add `127.0.0.1  whoami.local.test`
  4. Visit `http://whoami.local.test/` in your browser to see the example web app.
  5. Visit `http://localhost:8080/` in your browser to see the Traefik dashboard.

## Description

[Traefik](https://traefik.io) is a Go-based proxy and request router.

Just like the previous examples, we'll use `docker-compose` to orchestrate multiple containers on our host; though this time we'll use a few new tricks:

  - We have two compose files; one for a Traefik proxy container and network, and one for an example web app.
  - We will use an `.env` file to set the `COMPOSE_PROJECT_NAME`, which is used as a prefix for project resources (e.g. networks, volumes, containers, etc.) in lieu of the directory name.

## Docker Compose

Notes:

  - [`docker-compose`](https://docs.docker.com/compose/reference/)
  - [`up`](https://docs.docker.com/compose/reference/up/)
  - [`COMPOSE_PROJECT_NAME`](https://docs.docker.com/compose/reference/envvars/#compose_project_name)
  - [`.env` file](https://docs.docker.com/compose/environment-variables/#the-env-file)
