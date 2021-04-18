# Installing PDO Postgres SQL driver on official PHP Alpine Docker image

Docker images based on Alpine Linux are great when you want a small final container. Here's how to install the Postgres PDO driver.

```bash
FROM php:fpm-alpine

RUN apk add --update --no-cache libpq && \
    apk add --no-cache --virtual .build-deps \
        $PHPIZE_DEPS \
        postgresql-dev \
        g++ && \
    docker-php-ext-install pdo_pgsql pgsql && \
    apk del .build-deps
```
