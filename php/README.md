# PHP-FPM with Nginx

## Quick Start

  1. Run `docker-compose up`
  2. Visit `http://localhost/` in your browser.

## Description

[PHP](http://php.net/) is a popular programming language for web projects. It is usually run behind a webserver (like Apache or Nginx) to serve web traffic, API requests, etc.

This example is straightforward, running PHP-FPM behind a plain Nginx container, and Nginx routes requests to a PHP script that prints the environment information on a webpage. To set this up, we will run three containers:

  1. Source code container (a data container to mount the source code we're running).
  2. PHP container
  2. Nginx container

We'll use `docker-compose` to orchestrate multiple containers on our host; see notes elsewhere in this repository for more information about `docker-compose`. There are a couple other unique things being done in the Nginx container's `Dockerfile` which are detailed below.

## Installing Packages

In order to run a health check (see the next section) to determine if Nginx is running properly, we need to install curl, a command line utility to make HTTP requests, inside the container.

Because the upstream [`nginx:latest`](https://github.com/nginxinc/docker-nginx/blob/master/mainline/jessie/Dockerfile) container uses Debian Jessie as a base image, we can easily install `curl` via `apt-get`. But to keep our Docker image more compact and efficient, we also need to clean up apt resources after the installation.

You might think to do this in a few steps, e.g.:

    RUN apt-get update
    RUN apt-get install -y curl
    RUN rm -rf /var/lib/apt/lists/* && rm -Rf /usr/share/doc && rm -Rf /usr/share/man
    RUN apt-get clean

However, if you do it this way, Docker will save not one but _four_ extra image layers, taking up more space on your host, and slightly more time to build. Instead, it's recommended to do operations like installing new software in one giant `RUN` command, e.g.:

    RUN apt-get update \
        && apt-get install -y curl \
        && rm -rf /var/lib/apt/lists/* \
        && rm -Rf /usr/share/doc && rm -Rf /usr/share/man \
        && apt-get clean

This way, when we build the Docker image via `docker-compose`, the `curl` installation will only add one layer, which is more efficient.

## Health Check

One feature that can be useful in certain situations is Docker's built-in `HEALTHCHECK` functionality. In the `nginx` container's `Dockerfile`, there's a line that provides a command Docker can use to check whether the container is healthy:

    HEALTHCHECK CMD curl --fail http://localhost/ || exit 1

Since we installed `curl` previously, we can use it to fetch `localhost`. If the request fails, `curl` will exit with an error, and Docker can use this to indicate whether the container is `healthy` or `unhealthy` while it's running.

You can check the status (after `docker-compose up`) using `docker ps`, or individually for this container with `docker inspect [container_id]`. Docker will provide one of the following statuses:

  - `starting`
  - `healthy`
  - `unhealthy`

Note that if you want to disable a base image's `HEALTHCHECK`, you can add a line `HEALTHCHECK NONE` in your Dockerfile. There are also more options available to control the `HEALTHCHECK` interval, timeout, and retries. Read [more about `HEALTHCHECK` in Docker's documentation](https://docs.docker.com/engine/reference/builder/#/healthcheck).
