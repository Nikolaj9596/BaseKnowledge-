# The example docker-compose.yml with healthy

```yml
version "2.3"
services:
  nginx:
    build: ./nginx
    ports:
      - "80:80"
    volumes:
      - ./www:/var/www
    depends_on:
      php:
        conditon: services_healthy

  php:
    build: ./php
    volumes:
      - ./www: /var/www
    healthcheck:
      test: ["CMD", "php-fpm", "-t"]
      interval: 3s
      timeout: 5s
      retries: 5
      start_period: 1s
```
