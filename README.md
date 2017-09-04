# docker-image-php

[![Build Status](https://travis-ci.org/chesszebra/docker-image-php.svg?branch=master)](https://travis-ci.org/chesszebra/docker-image-php)

This repository contains the Docker image used to run PHP CLI containers.

## Usage

Run a PHP console:

```bash
docker run --rm -it -v $(pwd):/data chesszebra/php
```

Run a PHP file:

```bash
docker run --rm -it -v $(pwd):/data chesszebra/php -f my-file.php
```
