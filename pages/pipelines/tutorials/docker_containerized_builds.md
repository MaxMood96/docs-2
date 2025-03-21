---
keywords: docs, pipelines, tutorials, docker
---

# Containerized builds with Docker

Buildkite has built-in support for running your builds in Docker containers. Running your builds with Docker allows each pipeline to define and document its testing environment, greatly simplifying your build servers, and provides build isolation when [parallelizing your build](parallel-builds).


## Overview

To run your steps using Docker, there are two official [Buildkite plugins](/docs/pipelines/integrations/plugins): the [Docker Compose plugin](https://github.com/buildkite-plugins/docker-compose-buildkite-plugin), and the [Docker plugin](https://github.com/buildkite-plugins/docker-buildkite-plugin).

The [Docker Compose plugin](https://github.com/buildkite-plugins/docker-compose-buildkite-plugin) supports repositories with `docker-compose.yml` files, projects that use multiple containers or have dependent services, and building docker images inside pipeline steps.

The [Docker plugin](https://github.com/buildkite-plugins/docker-buildkite-plugin) supports single container applications, and involves less setup than the Docker Compose plugin. It doesn't allow the use of `docker-compose.yml` files, and doesn't support advanced operations like building images.

## Docker Hub rate limits

If you're using Docker with Docker images hosted on Docker Hub, note that as of 2nd November 2020 there are [strict rate limits](/docs/pipelines/integrations/other/docker-hub) for image downloads.


## Creating a Docker Compose configuration file

For most projects we recommend using [Docker Compose](https://docs.docker.com/compose/) as it allows each pipeline to define its own `docker-compose.yml` with dependent containers and environment variables to be passed through.

Here's a example of a `docker-compose.yml` file for a Ruby on Rails application that depends on Postgres, Redis and Memcache:

```yml
version: '2'
services:
  db:
    image: postgres
  redis:
    image: redis
  memcache:
    image: memcached
  app:
    build: .
    working_dir: /app
    volumes:
      - .:/app
    depends_on:
      - db
      - redis
      - memcache
    environment:
      PGHOST: db
      PGUSER: postgres
      REDIS_URL: redis://redis
      MEMCACHE_SERVERS: memcache
```
{: codeblock-file="docker-compose.yml"}


Mounting `.` (the directory of current build) as a volume in the container allows any changes from inside the container to be visible to the agent. This is required if you want to upload artifacts that were created in the container.

## Configuring the build step

This example runs a test suite in the `app` service using the [Docker Compose plugin](https://github.com/buildkite-plugins/docker-compose-buildkite-plugin):

```yml
- name: "Docker Test %n"
  command: test.sh
  plugins:
    - docker-compose#v3.0.3:
        run: app
```
{: codeblock-file="pipeline.yml"}


This is the equivalent of running:

```bash
docker-compose run app test.sh
```

For more examples and information on using this plugin, have a look at the [Docker Compose Plugin on GitHub](https://github.com/buildkite-plugins/docker-compose-buildkite-plugin).

The Buildkite Agent also has support for running build steps in an existing Docker image. To use the Docker plugin, add the `plugins` attribute to your command step.

In this example, the `yarn` commands will be run inside a Docker container using the `node:8` Docker image:

```yml
steps:
  - command: yarn install && yarn run test
    plugins:
      - docker#v3.2.0:
          image: "node:8"
          workdir: /app
```
{: codeblock-file="pipeline.yml"}

There are many configuration options available for the Docker plugin. For the complete list, see the readme for the [Docker Buildkite Plugin on GitHub](https://github.com/buildkite-plugins/docker-buildkite-plugin).

>📘 Pinning plugin versions
> Specifying the version of your plugin using the <code>plugin-name#vx.x.x</code> format is recommended, to ensure that no changes are introduced without your knowledge.

## Pipeline templates using Docker

To see more examples of how Docker is used in Buildkite pipelines, browse the [Docker templates](https://buildkite.com/pipelines/templates?platform=docker).


## Creating your own Docker infrastructure

If your team has significant Docker experience you might find it worthwhile invoking your own runner scripts rather than using the simpler built-in Docker support.

To do this, see the [job lifecycle hooks](/docs/agent/v3/hooks#job-lifecycle-hooks)
and [parallel builds](parallel-builds) documentation.

## Adding buildkite-agent to the Docker group

On the agent machine, to allow `buildkite-agent` to use the Docker client, you'll need to ensure its user has the necessary permissions. For most platforms this means adding the `buildkite-agent` user to your system's `docker` group, and then restarting the Buildkite Agent to ensure it is running with the correct permissions. See your platform's [Docker installation instructions](https://docs.docker.com/installation/) for more details.

## Adding a cleanup task

Over time your Docker host's file system can fill up with unused images. It's recommended to schedule Docker's [system prune](https://docs.docker.com/engine/reference/commandline/system_prune) command to run on a daily basis, which can remove all unused containers, networks, and images.
