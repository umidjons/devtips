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

Clear dangling images
```bash
docker rmi $(docker images -qf dangling=true)
```

Show Docker Daemon status:
```bash
sudo service docker status
```

Restart Docker Daemon:
```bash
sudo service docker restart
```

## Dockerization Best Practices

### Clear downloaded packages

On debian based images we can remove downloaded archives/installers/packages with the following command:
```bash
rm -rf /var/lib/apt/lists/*
```

Install packages without documentation and clear packages example:
```Dockerfile
RUN yum -y --setopt=tsflags=nodocs update && \
    yum -y --setopt=tsflags=nodocs install httpd && \
    yum clean all
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

Other solution:
```Dockerfile
# forward request and error logs to docker log collector
RUN ln -sf /dev/stdout /var/log/nginx/access.log
RUN ln -sf /dev/stderr /var/log/nginx/error.log
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

## Limit uploads and downloads

File `/etc/docker/daemon.json`:
```json
{
    "max-concurrent-downloads": 1,
    "max-concurrent-uploads": 1
}
```

## Push Image into Central Docker Host

Configure CLI
```bash
aws configure
# or
sudo aws configure
```

List Repositories
```bash
aws ecr describe-repositories
```

Create new repository
```bash
aws ecr create-repository --repository-name <repo-name>
aws ecr create-repository --repository-name aurea/df-scm
```

List images from the specified repository:
```bash
aws ecr --repository-name=<repo-name> list-images
aws ecr --repository-name=aurea/df-scm list-images
```

Tag existing image:
```bash
docker tag <image-name> 892905447879.dkr.ecr.us-east-1.amazonaws.com/aurea/<product-name>:<version or tag>
docker tag prod_testrail_app 892905447879.dkr.ecr.us-east-1.amazonaws.com/aurea/df-scm:prod_testrail_app
```

Login into AWS:
```bash
eval $(aws ecr get-login)
# or
eval $(sudo aws ecr get-login)
```

Push image into repository:
```bash
docker push 892905447879.dkr.ecr.us-east-1.amazonaws.com/<product-name>:<version or tag>
docker push 892905447879.dkr.ecr.us-east-1.amazonaws.com/aurea/df-scm:prod_testrail_app
```

Deleting the image:
```bash
aws ecr batch-delete-image --repository-name <repo-name> --image-ids imageDigest=<imageDigest>

aws ecr batch-delete-image --repository-name 892905447879.dkr.ecr.us-east-1.amazonaws.com/aurea/df-scm --image-ids imageDigest=sha256:acaa3ddbb21298145991f5333e0d76912fbe9121bb436e18dcce422e7003f4d1
```

## Find out enforcer rule violation
Get container name:
```bash
docker ps -a --filter label="com.trilogy.product=testrail"
```

Search rules by container name on http://10.69.11.89:12504/ and http://10.69.11.89:12507/:
```bash
{"id": "289dd909efb60a24222d039c4f9b172eba97abb1c228d0489cbc4435a2e06101", "name": "/festive_heisenberg", "violated_rule": "container name parts (STAGE/PRODUCT/SERVICE) must be equal to related labels com.trilogy.* labels", "count": 1, "last_timestamp": "2017-05-25T12:29:13.875542" },
{"id": "289dd909efb60a24222d039c4f9b172eba97abb1c228d0489cbc4435a2e06101", "name": "/festive_heisenberg", "violated_rule": "must have CPU limit", "count": 1, "last_timestamp": "2017-05-25T12:29:13.878972" },
```

If enfocer kills container on build time, then fetch image like this with proper container name:
```bash
export DOCKER_HOST=tcp://10.69.11.89:2376
docker run --name prod_testrail_app 892905447879.dkr.ecr.us-east-1.amazonaws.com/aurea/df-scm:prod_testrail_app
```

## Installing MySQL on Amazon Linux

Following are instruction to install mysql server and configure it to work with large amount of data:
```bash
# install mysql 5.6 server
yum install -y mysql56-server.x86_64

# configure hostname
echo HOSTNAME=`hostname` >> /etc/sysconfig/network

# set net_read_timeout, net_write_timeout and max_allowed_packet parameters
sed -ri -e "s|^\s*(\[mysqld\])|\1\nnet_read_timeout=7200|g" /etc/my.cnf
sed -ri -e "s|^\s*(\[mysqld\])|\1\nnet_write_timeout=7200|g" /etc/my.cnf
sed -ri -e "s|^\s*(\[mysqld\])|\1\nmax_allowed_packet=1G|g" /etc/my.cnf

# start mysql daemon
service mysqld start
```

## Transfer data from one db to another

```bash
# create database
mysqladmin create mydestinationdb
# transfer data from mysourcedb into mydestinationdb
mysqldump -h my-source-host -umyuser -pmypassword mysourcedb --compress --verbose | mysql mydestinationdb
# transfer mytable data from mysourcedb into mydestinationdb
mysqldump -h my-source-host -umyuser -pmypassword mysourcedb --tables mytable --compress --verbose | mysql mydestinationdb
```

## Find out versions

```bash
httpd -v
Server version: Apache/2.2.3
Server built: Mar 27 2010 13:52:45
```

```bash
httpd -V
# OR
apachectl -V

Server version: Apache/2.2.3
Server built:   Jan 31 2011 17:50:30
Server''s Module Magic Number: 20051115:3
Server loaded:  APR 1.2.7, APR-Util 1.2.7
Compiled using: APR 1.2.7, APR-Util 1.2.7
Architecture:   64-bit
Server MPM:     Prefork
  threaded:     no
    forked:     yes (variable process count)
Server compiled with....
 -D APACHE_MPM_DIR="server/mpm/prefork"
 -D APR_HAS_SENDFILE
 -D APR_HAS_MMAP
 -D APR_HAVE_IPV6 (IPv4-mapped addresses enabled)
 -D APR_USE_SYSVSEM_SERIALIZE
 -D APR_USE_PTHREAD_SERIALIZE
 -D SINGLE_LISTEN_UNSERIALIZED_ACCEPT
 -D APR_HAS_OTHER_CHILD
 -D AP_HAVE_RELIABLE_PIPED_LOGS
 -D DYNAMIC_MODULE_LIMIT=128
 -D HTTPD_ROOT="/etc/httpd"
 -D SUEXEC_BIN="/usr/sbin/suexec"
 -D DEFAULT_PIDLOG="run/httpd.pid"
 -D DEFAULT_SCOREBOARD="logs/apache_runtime_status"
 -D DEFAULT_LOCKFILE="logs/accept.lock"
 -D DEFAULT_ERRORLOG="logs/error_log"
 -D AP_TYPES_CONFIG_FILE="conf/mime.types"
 -D SERVER_CONFIG_FILE="conf/httpd.conf"
```

Show apache's loaded modules:
```bash
apachectl -M
```

```bash
ruby -v
ruby 1.8.7
```

```bash
rails -v
rails 2.3.8
```

```bash
gem -v
gem 1.3.7
```

```bash
rvm -v
rvm 1.9.2
```

## Find out necessary packages for rvm

`rvm requirements`

## Find out installed gems

`gem list`

Sample output:
```
  LOCAL GEMS ***
actionmailer (2.3.8, 2.2.2)
actionpack (2.3.8, 2.2.2)
activerecord (2.3.8, 2.2.2)
activeresource (2.3.8, 2.2.2)
activesupport (2.3.8, 2.2.2)
builder (3.0.0)
calendar (1.11.4)
calendar_date_select (1.16.1)
daemons (1.1.4)
fastercsv (1.5.3)
fastthread (1.0.7)
hpricot (0.8.4)
httpclient (2.2.1)
libxml-ruby (2.2.2)
mysql (2.8.1)
open4 (1.0.1)
passenger (2.2.15)
Platform (0.4.0)
popen4 (0.1.2)
rack (1.1.0)
rails (2.3.8, 2.2.2)
rake (0.8.7)
rufus-scheduler (2.0.6)
```

## Find out mysql databases size

```mysql
SELECT table_schema "DB Name", 
Round(Sum(data_length + index_length) / 1024 / 1024, 1) "DB Size in MB" 
FROM information_schema.tables 
GROUP BY table_schema;
```

Sample output:
```
+--------------------+---------------+
| DB Name            | DB Size in MB |
+--------------------+---------------+
| gbuildserver       |         264.3 | 
| information_schema |           0.0 | 
| mysql              |           0.5 | 
+--------------------+---------------+
3 rows in set (0.35 sec)
```

## Patching Apache's configuration

```bash
sed -ri -e '\%^<Directory /usr/share>%,\%^</Directory>% s|Require all granted|Require all denied|g' /etc/apache2/apache2.conf
```

`\%START%,\%END%` - this pattern limits matching context to the lines from START until END

After execution above lines
```htaccess
<Directory /usr/share>
    AllowOverride None
    Require all granted
</Directory>
```

becomes

```htaccess
<Directory /usr/share>
    AllowOverride None
    Require all denied
</Directory>
```