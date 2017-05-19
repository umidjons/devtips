# Docker tips

# Useful Links

- [Bash Auto Completion for Docker](https://docs.docker.com/machine/completion/#bash)
- [Bash Auto Completion for Docker Compose](https://docs.docker.com/compose/completion/)
- [Dockerfile Syntax Highlighting for Sublime Text](https://github.com/asbjornenge/Docker.tmbundle)

## Common tips

Run container in a detached mode:
```bash
docker run -d <image>
```

Attach to a detached running container:
```bash
docker attach <container>
```

Add line to `/etc/hosts`:
```bash
docker run --add-host myhost:10.10.10.2 ...
```

Specify container's IP address manually:
```bash
docker run --ip=192.168.1.19 ...
```

Ssh into docker-machine:
```bash
docker-machine ssh <machine-name>
```

Run process by specified user (in docker it would be as PID=1)
```bash
# Run `ps aux` by `root` user
docker run -it --rm ubuntu:trusty gosu root ps aux
```

## Dockerization Best Practices

### Clear downloaded packages

On debian based images we can remove downloaded archives/installers/packages with the following command:
```bash
rm -rf /var/lib/apt/lists/*
```

### Apache Dockerization tips

Redirect Apache's logs and errors to `STDOUT` and `STDERR`

Update `httpd.conf` with `sed`:
```bash
sed -ri \
    -e 's!^(\s*CustomLog)\s+\S+!\1 /proc/self/fd/1!g' \
    -e 's!^(\s*ErrorLog)\s+\S+!\1 /proc/self/fd/2!g' /etc/httpd/conf/httpd.conf
```

Or edit `httpd.conf` manually:
```htaccess
# Errors to STDERR
ErrorLog /proc/self/fd/2

# Logs to STDOUT
CustomLog /proc/self/fd/1 combined
```

Set document root:
```bash
sed -ri -e 's!^(DocumentRoot)(\s*\S+)!\1 "/var/www/mysite"!g' /etc/httpd/conf/httpd.conf
```

Enable php handler:
```bash
sed -ri -e 's!^(AddType application/x-gzip .gz .tgz)!\1\nAddType application/x-httpd-php .php\n!g' /etc/httpd/conf/httpd.conf
```

Run Apache on foreground:
```bash
# Remove PID of the apache if exists
/bin/rm -f /var/run/httpd/httpd.pid

# Start apache
#/etc/init.d/httpd start
/usr/sbin/httpd -DFOREGROUND
```

Test Apache's configuration:
```bash
apachectl configtest
```

## Custom `init`

### `dumb-init` example on `alpine` image

```Dockerfile
FROM alpine:3.5

MAINTAINER Umid Almatov <umid.almatov@aurea.com>

ENV DUMB_INIT_DOWNLOAD_URL=https://github.com/Yelp/dumb-init/releases/download/v1.2.0/dumb-init_1.2.0_amd64

RUN apk update \
    && apk add ca-certificates wget \
    && update-ca-certificates \
    && wget -O /usr/local/bin/dumb-init $DUMB_INIT_DOWNLOAD_URL \
    && chmod +x /usr/local/bin/dumb-init

ENTRYPOINT ["dumb-init", "--rewrite", "15:2", "top"]
```

### `dumb-init` example on `ubuntu` image

```Dockerfile
FROM ubuntu:16.04

MAINTAINER Umid Almatov <umid.almatov@aurea.com>

ENV DUMB_INIT_DOWNLOAD_URL=https://github.com/Yelp/dumb-init/releases/download/v1.2.0/dumb-init_1.2.0_amd64

RUN set -e \
    && apt-get update -q \
    && apt-get install -y -q --no-install-recommends wget \
    && wget --no-check-certificate -O /usr/local/bin/dumb-init $DUMB_INIT_DOWNLOAD_URL \
    && chmod +x /usr/local/bin/dumb-init

ENTRYPOINT ["dumb-init", "--rewrite", "15:2", "top"]
```

## `HEALTHCHECK` example

```Dockerfile
HEALTHCHECK  --interval=1m --timeout=10s --retries=3 \
    CMD curl --fail http://localhost/ || exit 1
```

## Patching `php.ini` with `sed` example

```bash
#!/bin/bash

if [ ! -f /installed.lock ]
then
    ########################################################
    echo "Patching php.ini"
    
    # Set parameters
    # max_input_vars=10000
    # memory_limit=2048M
    # post_max_size=64M
    # upload_max_filesize=64M
    sed -ri \
        -e 's!^(\s*max_input_vars\s*=\s*)(\S+)!\110000!g' \
        -e 's!^(\s*memory_limit\s*=\s*)(\S+)!\12048M!g' \
        -e 's!^(\s*post_max_size\s*=\s*)(\S+)!\164M!g' \
        -e 's!^(\s*upload_max_filesize\s*=\s*)(\S+)!\164M!g' /etc/php.ini
fi

# Mark as installed
touch /installed.lock
```

## Installing & enabling `ionCube Loader`

```bash
#!/bin/bash

set -euxo pipefail

PHP_VERSION=5.3

if [ ! -f /installed.lock ]
then
    ########################################################
    echo "Setting up ionCube Loader"
    
    # Install wget to download ionCube Loader
    yum install -y wget

    # Download ionCube Loader
    cd /tmp
    wget http://downloads3.ioncube.com/loader_downloads/ioncube_loaders_lin_x86-64.tar.gz
    tar xfz ioncube_loaders_lin_x86-64.tar.gz
    
    echo "Enabling ionCube Loader"
    PHP_EXT_DIR=$(php-config --extension-dir)
    cp /tmp/ioncube/ioncube_loader_lin_${PHP_VERSION}.so ${PHP_EXT_DIR}
    echo "zend_extension=${PHP_EXT_DIR}/ioncube_loader_lin_${PHP_VERSION}.so" >> /etc/php.ini
    rm -rf /tmp/*
fi

# Mark as installed
touch /installed.lock
```

## Adding cron job example

```bash
#!/bin/bash

set -euxo pipefail

if [ ! -f /installed.lock ]
then
    echo "Enabling the cron task"
    crontab -l | { cat; echo "* * * * * /usr/bin/php /var/www/mysite/task.php"; } | crontab -
fi

# Mark as installed
touch /installed.lock
```