# Symfony App with SQLite Database

## Quick Start

  1. Run `docker-compose up`
  2. Visit `http://localhost/` in your browser.

## Description

[Symfony](https://symfony.com/) is a PHP framework for web projects.

Symfony is a robust framework of components that power some of the largest PHP-based websites and apps on the web. Most apps and sites require some sort of database to store data, so we need to create not one but _two_ Docker containers, and link them together:

  1. Symfony container (with PHP and Composer).
  2. SQLite container

Just like the Flask example, we'll use `docker-compose` to orchestrate multiple containers on our host.

## Docker Compose

Notes:

  - [`docker-compose`](https://docs.docker.com/compose/reference/)
  - [`up`](https://docs.docker.com/compose/reference/up/)

## Persisting Data

The same notes apply for our SQLite container here as applied to the MySQL container in the Flask example.
