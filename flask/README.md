# Flask App with MySQL Database

## Quick Start

  1. Run `docker-compose up`
  2. Visit `http://localhost/` in your browser.

## Description

[Flask](http://flask.pocoo.org/) is a web development microframework for Python. It's efficient, easy-to-use, and easy-to-deploy!

I often use Flask when building small webservices or dynamic websites and feel like trying something fresh and different from PHP, my daily driver. Most webservices and sites require some sort of database to store data, so we need to create not one but _two_ Docker containers, and link them together:

  1. Flask container (with Python).
  2. MySQL container

In early examples, we would build or import just one container and work with it to run some code. In real-world usage, you'll often need two, three, or even _dozens_ of containers to support the project you're working on!

It would get quite unweildy to have to manage all of these containers with separate `run` and `stop` commands, so Docker has an answer: `docker-compose`.

## Docker Compose

Notes:

  - [`docker-compose`](https://docs.docker.com/compose/reference/)
  - [`up`](https://docs.docker.com/compose/reference/up/)

## Persisting Data

One thing repeated early and often in the world of Docker is _data stored inside a container is not persisted_. So... this creates quite the conundrum: what if you need data to persist after a container exits?

Traditionally, Docker allowed the use of 'data containers' which would contain a `VOLUME` and nothing else, based on a really tiny base image, then you use `volumes_from` or `-v` to link the volume at the specified path into a container.

In our case, we want to explicitly state that we want the path `/var/lib/mysql` to persist as a volume separate from the general `db` volume. This way if you build the Database container, store some data inside a MySQL database that's stored within that path, then exit, you can run the Database container again later, and the data you stored earlier won't have vanished!

We did this inside the `docker-compose.yml` file here:

    services:
      ...
      db:
        ...
        volumes:
          - /var/lib/mysql

The official MySQL image adds it's own `VOLUME` directive in its Dockerfile, so it's not necessary for us to specify the volume in our own `docker-compose` configuration... but I like to be explicit about things like _where my important data is stored_!

After you bring up the containers, you can inspect the `db` container to ensure the volume is configured correctly: `docker inspect flask_db_1` — this displays all the container info, including a list of 'Volumes'.

You can also mount a directory on the host as a vlume using the syntax `[host-path]:[container-path]`, so if you want to mount a `database` directory in your project folder as `/var/lib/mysql`, change the the volume like so:

    services:
      ...
      db:
        ...
        volumes:
          - ./database:/var/lib/mysql

If you change the volume settings, then you will need to `stop` any running containers, then `rm` them and rebuild before the changes take effect (remember, containers are immutable!).

Read more: [Manage data in containers](https://docs.docker.com/engine/tutorials/dockervolumes/)

> Note for Mac/Windows users: If you encounter performance issues with your app running inside Docker containers, and your app has many (e.g. 1,000+) files, it could be related to some filesystem performance issues with current versions of Docker. See [File access in mounted volumes extremely slow](https://forums.docker.com/t/file-access-in-mounted-volumes-extremely-slow-cpu-bound/8076/107).
> 
> This is a hard problem to solve, though the situation should improve as time goes on. For now, find ways to use volumes that aren't shared to your host whenever possible. You can also look into using tools like [docker-sync](https://docker-sync.io/) if you _must_ sync large numbers of files.
