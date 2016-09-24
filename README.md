# Docker Examples - by geerlingguy

[![Build Status](https://travis-ci.org/geerlingguy/docker-examples.svg?branch=master)](https://travis-ci.org/geerlingguy/docker-examples)

The web is full of Docker examples and tutorials and repos.

There are many like it, but this one is mine.

## Philosophy

I like learning from first principles. Docker masks a surprising amount of complexity, and most tutorials try to gloss over them to show 'the cool shiny things' before you can even understand what's going on.

I'd rather start really simple, and build from there until I fully understand what's going on. Therefore the examples in this repo build on each other until we get to some actual 'this could do something useful' kinds of infrastructure.

## Installation

  1. Install [Docker for Mac](https://www.docker.com/products/docker#/mac).
  2. Start Docker.app
  3. Open Terminal, make sure it's running with `docker --version`.

## Getting Started

### First Docker command

To kick the tires and make sure things are working, run:

    docker run hello-world

This command is doing the following:

  - [`docker`](https://docs.docker.com/engine/reference/commandline/cli/) - The main Docker command.
  - [`run`](https://docs.docker.com/engine/reference/run/) - Run a container.
  - `hello-world` - The name of the Docker Hub repository to pull from. In this case, we'll get the latest [`hello-world`](https://hub.docker.com/_/hello-world/) Docker image. If you don't specify a version, this is interpreted as `hello-world:latest`.

If things are working correctly, you should see some output, then the container will exit.

### First real-world example - Simple Nginx Webserver

Docker's [tutorial](https://docs.docker.com/docker-for-mac/) provides a simple example of running an Nginx webserver on localhost with the command:

    docker run -d -p 80:80 --name webserver nginx

This command is doing the following:

  - [`docker`](https://docs.docker.com/engine/reference/commandline/cli/) - The main Docker command.
  - [`run`](https://docs.docker.com/engine/reference/run/) - Run a container.
  - [`-d`](https://docs.docker.com/engine/reference/run/#/detached-d) - Run a container detached; when the process (nginx, in this case) exits, the container will exit.
  - [`-p 80:80`](https://docs.docker.com/engine/reference/commandline/run/#/publish-or-expose-port-p-expose) - Publish or expose a port (`[host-port]:[container-port]`, so in this case bind the container's port `80` to the host's port `80`).
  - [`--name webserver`](https://docs.docker.com/engine/reference/commandline/run/#/assign-name-and-allocate-pseudo-tty-name-it) - Assign a name to a container.
  - `nginx` - The name of the Docker Hub repository to pull from. In this case, we'll get the latest [`nginx`](https://hub.docker.com/_/nginx/) Docker image. If you don't specify a version, this is interpreted as `nginx:latest`.

The first time you run this command, it will download the Nginx Docker image (it actually downloads a few 'layers' which build up the official image), then run a container based on the image.

Run the command, then access `http://localhost:80/` in a web browser. You should see the 'Welcome to nginx!' page.

#### Playing with the Nginx container

  - Run `docker ps` to see a list of running containers; you should see the Nginx container you just started in the list.
  - Run `docker stop webserver` to stop the named container (you can also use the 'container ID' if you want).
  - Run `docker ps -a` to see a list of all containers on the system, including stopped containers.
  - Run `docker start webserver` to start the named container again.
  - Run `docker rm webserver` to delete the container entirely (you can also pass `--rm` to the `run` command if you want the container deleted after it exits).

> Note: Starting and stopping a container is usually quicker than building it from scratch with `docker run`, so if possible, it's best to generate the container with `run` once and use `start`/`stop` until you need to rebuild the container.

### Second real-world example - Simple Python Flask App

Docker has another [tutorial](https://docs.docker.com/engine/tutorials/usingdocker/) that digs a little deeper into Docker CLI usage, but for our purposes, we'll just run the main command, and this time allow Docker to map an ephemeral port (any available high port number on our host) to the port configured in the container's configuration:

    docker run -d -P training/webapp python app.py

Besides the obvious, this command is doing a couple new things:

  - [`-P`](https://docs.docker.com/engine/reference/run/#/expose-incoming-ports) - Publish all ports that are `EXPOSE`d by the docker container to ephemeral ports on the host (unlike `-p`, which requires specification of each port mapping).
  - `training/webapp` - The name of the Docker Hub repository to pull from. In this case we'll get the latest [`training/webapp`](https://hub.docker.com/r/training/webapp/) Docker image.
  - `python app.py` - This is the command that will be run inside the container when it's launched. Until the `app.py` exits, or you `docker stop` or `docker kill` the container, it will keep running.

Once the container is started, run `docker ps` to see what host port the container is bound to, then visit that port in your browser, e.g. `http://localhost:32768/`. You should see the text "Hello world!" in your browser.

Since we didn't specify a `--name` when we ran this `docker run` command, Docker assigned a random name to the container (in my case, `romantic_bell`), so to `stop`, `rm`, or otherwise interact with the container, you have to use the generated name or the container ID.

### Other Essential commands

At this point, you should be somewhat familiar with the main Docker CLI. Some other commands that come in handy at this point are:

  - `docker images`: Show a list of all images you have downloaded locally.
  - `docker rmi [image-name]`: Remove a particular image (save some disk space!).
  - `docker logs [container-name]`: Tail the logs (stdout) of a container (try this on the Flask app while refreshing the page!).

## Diving Deeper

Other examples warrant their own dedicated directories, with example code and individual detailed README's explaining how they work. Included examples:

  - [`/flask`](/flask) - Python Flask and MySQL.
  - [`/php`](/php) - PHP-FPM and Nginx.
  - [`/symfony`](/symfony) - Symfony and SQLite.

## License

This project is licensed under the MIT open source license.

## About the Author

[Jeff Geerling](http://www.jeffgeerling.com/) is the author of [Ansible for DevOps](https://www.ansiblefordevops.com/) and manages tons of infrastructure, as well as open source projects like [Drupal VM](https://www.drupalvm.com/).
