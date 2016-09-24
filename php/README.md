# PHP-FPM with Nginx

## Quick Start

  1. Run `docker-compose up`
  2. Visit `http://localhost/` in your browser.

## Description

[PHP](http://php.net/) is a popular programming language for web projects. It is usually run behind a webserver (like Apache or Nginx) to serve web traffic, API requests, etc.

This example is as barebones as we can get, running vanilla PHP behind a plain Nginx container, and all we will do is display the PHP environment information on a webpage. To do this, we will run three containers:

  1. Source code container (a data container to mount the source code we're running).
  2. PHP container
  2. Nginx container

We'll use `docker-compose` to orchestrate multiple containers on our host.

## Docker Compose

Notes:

  - [`docker-compose`](https://docs.docker.com/compose/reference/)
  - [`up`](https://docs.docker.com/compose/reference/up/)
