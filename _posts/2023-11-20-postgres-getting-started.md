---
layout: post
title: PostgreSQL - getting started
date: 2023-11-20
---

I am looking into the internals of PostgreSQL (finally! 🎉), I'm writing down what I (re-)learnt here.

## Resources

- The PostgreSQL official [doc](https://www.postgresql.org/docs/current/index.html) is pretty good.
- This [website](https://www.interdb.jp/pg/pgsql01.html) is super useful.
- This [series](https://www.youtube.com/playlist?list=PL1XF9qjV8kH0ghGRGo3_f-FWqWvAbv1dh) is what I am using to understand the internals of PostgreSQL, it's super fun and easy to watch.

## Terminology/Concepts

- The **server** is basically the main PostgreSQL process that runs on a machine.
- A **connection** is a process spawn/forked from the main process.
- A **database** is a directory with a bunch a file.
- A **table** is a bunch of data stored in files (called heapfiles).

## Set up

The easiest to run Postgres is using Docker and mount a volume to persist the data.

Here is a docker-compose file

```
version: "3.5"

services:
  postgres:
    container_name: pg16
    image: "postgres:16.1"
    environment:
      POSTGRES_USER: ${POSTGRES_USER:-postgres}
      POSTGRES_HOST_AUTH_METHOD: trust # no password required
      PGDATA: /data/postgres
    volumes:
      - ./persisted-data:/data/postgres
    ports:
      - "5460:5432"
    restart: unless-stopped

volumes:
  postgres:

```

And then just run it

```
$ docker-compose up --detach
```

Exec into the container ready to explore what's inside 🙂

```
$ docker exec -it pg16 /bin/sh
```

💡 Once the docker container is up and running, we can see a bunch of directories/files under `./persisted-data` on the local machine (or whatever volume was mounted onto the docker container).

`psql`` comes installed with Postgres so can be used to run all sorts of queries :)

```
$ psql -U postgres -d postgres
```
