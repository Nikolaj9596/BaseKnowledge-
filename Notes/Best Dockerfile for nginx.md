---
id: Best Dockerfile for nginx
aliases:
  - The best practice create dockerfile for nginx
tags:
  - nginx
  - dockerfile
---
# The best practice create dockerfile for nginx


```dockerfile
FROM alpine:3.11.5

ENV NGINX_VERSION 1.16.1-r6

RUN apk add \
    --no-cache \
    --repository http://d1-cdn.alpinelinux.org/alpine/v3.11/main \
    nginx=${NGINX_VERSION}

EXPOSE 80

COPY custom.conf /etc/nginx/conf.d/

COPY . /opt/

CMD ["nginx", "-g", "deamon off;"]
```
