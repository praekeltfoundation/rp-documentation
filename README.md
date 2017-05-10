Documentation for working with RapidPro
=======================================

At Praekelt.org we use self-hosted versions RapidPro to provide the messaging tooling of our Maternal Health programs. This documentation serves as a starting point for our team members in getting up to speed with running — and developing with — RapidPro.

## Basics

In our hosting environments we run RapidPro in Docker containers. The RapidPro community — with Nyaruka — has added support for building Docker images of RapidPro via the [rapidpro-docker][rpd] repository.

We build our own images of RapidPro that we run in production and QA, so have [created a fork of the rapidpro-docker repo][rpd-fork] that our images are built from.

There are a few reasons we use our fork to build our own images:

- To give us full control of the RapidPro Django settings file
- To allow us to add or change what is configured via environment variables without effecting upstream users
- To allow us to use our internal build and caching tools where needed

## Getting Started

In the `docker` folder of this repo there are some docker-compose files that make it easier to get the required RapidPro containers up and running on a local machine.

At the moment there are two options for running the containers locally:

1. Running just the RapidPro containers (docker-compose.yml)
2. Running all the dependencies as containers (docker-compose-with-deps.yml)

The main difference between this two is that option one requires you to run Redis and PostgreSQL outside of Docker and make those services available to the containers. Which you choose is a matter of preference.


### Option One: Just RapidPro

The `docker-compose.yml` compose file provides two containers: the RapidPro web interface and the RapidPro Celery workers and scheduler. This means that you need to provide the PostgreSQL and Redis connection parameters as environment variable.

By default this compose file will pull the latest image from `praekeltfoundation/rapidpro` Docker Hub repo but this can be overridden by setting the image name in the `RAPIDPRO_IMAGE` envvar.

Note: If you are having troubling getting the containers network access to the PostgreSQL and Redis servers running on the host, you can bind an IP address as an alias to localhost and make sure that both PostgreSQL and Redis accept connections on that address (or all addresses).

On a Mac:

```
sudo ifconfig lo0 alias 10.200.10.1/24
```

Steps:

1. Set the envars:

    ```bash
    export RAPIDPRO_DB_URL="postgresql://postgres:password@10.200.10.1/rapidpro"
    export RAPIDPRO_REDIS_URL="redis://10.200.10.1:6379/0"
    ```

2. Start the containers:

    ```bash
    docker-compose up
    ```

3. Browse to `http://localhost:8000` to access the RapidPro UI and setup the first Organisation


### Option Two: RapidPro + Dependencies

The `docker-compose-with-deps.yml` compose file provides four containers: the RapidPro web interface, the RapidPro Celery workers and scheduler, the PostgreSQL container and the Redis container.

By default this compose file will pull the latest image from `praekeltfoundation/rapidpro` Docker Hub repo but this can be overridden by setting the image name in the `RAPIDPRO_IMAGE` envvar.

Steps:

1. Start the containers:

    ```bash
    docker-compose up
    ```

3. Browse to `http://localhost:8000` to access the RapidPro UI and setup the first Organisation


[rpd]: https://github.com/rapidpro/rapidpro-docker
[rpd-fork]: https://github.com/praekeltfoundation/rapidpro-docker
