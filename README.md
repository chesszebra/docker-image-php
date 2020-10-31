# docker-image-php

This repository contains the Docker image used to run PHP CLI containers.

## Usage

### Apache

We override the default vhost and map a volume to `/var/www/html`. In there we expect a directory called `public`.

The apache container is usually used within a `docker-compose.yml` file like this:

```
version: '3.0'

services:
  webserver:
    image: chesszebra/php:7.1-apache
    volumes:
      - ./:/var/www/html
```

Note that the id of user `www-data` has been changed to the owner of directory `/var/www/html`. The same applies to 
the group `www-data`. The reason we do this is because this way we can safely create files that are owned by the user 
that is starting the container. Since we map a volume to `/var/www/html`, it will always be the correct id.

If you want to change the root directory of your vhost, specify an environment variable `VHOST_DIRECTORY`. In this 
example the root of the project is also considered the root of the vhost:

```
version: '3.0'

services:
  webserver:
    image: chesszebra/php:7.1-apache
    environment:
      VHOST_DIRECTORY: /var/www/html
    volumes:
      - ./:/var/www/html
```

To use the Apache images without `docker-compose`, you could do this:

```
docker run --rm -it -v $(pwd):/var/www/html chesszebra/php:7.1-apache
``` 

### CLI

The `php` command is already specified in the image. Therefor you can specify flags that `php` would expect directly 
after starting the container. Make sure you enable the interactive flags and map a volume to `/data`.

```
docker run --rm -it -v $(pwd):/data chesszebra/php:7.3-cli -f file.php
```

### Disabling extensions

The images come fully equipped with extensions to make our development easier. In certain situations you want to
disable extensions. This can be done via the `php` executable but it's not a fun ride. PHP defines the `-n` flag which 
can be used to ignore all extensions. The `-d` flag can be used to enable a specific extension so to disable a specific
extension, you have to disable *all* extensions and than enable the others explicitly.

For example, the following command will not give any output:

```
docker run \
    --rm \
    -it \
    chesszebra/php:7.1-cli \
    -n \
    -r 'phpinfo();' \
    | grep swoole 
```

While the following will:

```
docker run \
    --rm \
    -it \
    chesszebra/php:7.1-cli \
    -n \
    -d extension=swoole.so \
    -r 'phpinfo();' \
    | grep swoole 
```
