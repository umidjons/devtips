# Dockerize PHP-FPM via Nginx

File `docker-compose.yml`:
```yml
version: "3"

services:
  web:
    image: nginx:latest
    ports:
      - "9999:80"
    volumes:
      - ./:/usr/share/nginx/html
      - ./site.conf:/etc/nginx/conf.d/default.conf

  php:
    image: php:7-fpm
    volumes:
      - ./:/var/www/html
    ports:
      - "9000:9000"
```

File `site.conf`:
```nginx
server {
    listen 80 default_server;
    listen [::]:80 default_server ipv6only=on;

    error_log  /var/log/nginx/error.log;
    access_log /var/log/nginx/access.log;

    root /usr/share/nginx/html;
    index index.php index.html index.htm;

    location ~ \.php$ {
        try_files $uri =404;
        fastcgi_split_path_info ^(.+\.php)(/.+)$;
        fastcgi_param SCRIPT_FILENAME /var/www/html$fastcgi_script_name;
        fastcgi_param PATH_INFO $fastcgi_path_info;
        fastcgi_pass php:9000;
        fastcgi_index index.php;
        include fastcgi_params;
    }
}
```

File `index.php`:
```php
<?php
phpinfo();
```

Run containers:

```bash
docker-compose up -d
```

Stop containers:

```bash
docker-compose down
```