# Seafile Production Guide

**Disclaimer:**

This guide was written for [Centos 7.x](https://www.centos.org/), you may run into issues if you are using another operating system. We are welcoming contributions for guides to other distributions.

This document is also written with the expectation that you have a technical level high enough to administrate Linux servers.

## What is this guide?

This guide is a walk through of the setup process of a [Seafile](https://www.seafile.com/en/home/) instance.

We use example.com to represent a domain or sub-domain. Example.com should be replaced with your instance domain or sub-domain.

## Prerequisites

You will need the following for this guide:

- A server running [Centos 7.x](https://www.Centos.org).
- Root access to the server.
- A domain or sub-domain to use for the instance.

## DNS

DNS records should be added before anything is done on the server.

The records added are:

-  A record (IPv4 address) for example.com
-  AAAA record (IPv6 address) for example.com

> ### A Helpful And Optional Note
>
> Using `tmux` when following through with this guide will be helpful.
>
>
> Not only will this help you not lose your place if you are disconnected, it will let you have multiple terminal windows open for switching contexts (root user versus the mastodon user).
>
> You can install [tmux](https://github.com/tmux/tmux/wiki) from the package manager:
>
> ```sh
> yum  -y install tmux
> ```

## Dependency Installation

#### Explanation of the dependencies

- imagemagick - Mastodon uses imagemagick for image related operations
- nginx - nginx is our frontend web server
- epel-release -
- python
- mariadb

```sh
yum -y install epel-release
yum -y install python-imaging MySQL-python python-simplejson python-setuptools mariadb mariadb-server nginx

```

**Note** that we do not set up a password of any kind, this is because we will be using ident authentication. This allows local users to access the database without a password.

## nginx Configuration

You need to configure [nginx](http://nginx.org) to serve your [Mastodon](https://github.com/tootsuite/mastodon/) instance.

**Reminder: Replace all occurrences of example.com with your own instance's domain or sub-domain.**

`cd` to `/etc/nginx/sites-available` and open a new file:

`nano /etc/nginx/sites-available/example.com.conf`

Copy and paste the following and make edits as necessary:

```nginx
map $http_upgrade $connection_upgrade {
  default upgrade;
  ''      close;
}

server {
  listen 80;
  listen [::]:80;
  server_name example.com;
  # Useful for Let's Encrypt
  location /.well-known/acme-challenge/ { allow all; }
  location / { return 301 https://$host$request_uri; }
}

server {
  listen 443 ssl http2;
  listen [::]:443 ssl http2;
  server_name example.com;

  ssl_protocols TLSv1.2;
  ssl_ciphers HIGH:!MEDIUM:!LOW:!aNULL:!NULL:!SHA;
  ssl_prefer_server_ciphers on;
  ssl_session_cache shared:SSL:10m;

  ssl_certificate     /etc/letsencrypt/live/example.com/fullchain.pem;
  ssl_certificate_key /etc/letsencrypt/live/example.com/privkey.pem;

  keepalive_timeout    70;
  sendfile             on;
  client_max_body_size 0;

  root /home/mastodon/live/public;

  gzip on;
  gzip_disable "msie6";
  gzip_vary on;
  gzip_proxied any;
  gzip_comp_level 6;
  gzip_buffers 16 8k;
  gzip_http_version 1.1;
  gzip_types text/plain text/css application/json application/javascript text/xml application/xml application/xml+rss text/javascript;

  add_header Strict-Transport-Security "max-age=31536000";

  location / {
    try_files $uri @proxy;
  }

  location ~ ^/(emoji|packs|system/accounts/avatars|system/media_attachments/files) {
    add_header Cache-Control "public, max-age=31536000, immutable";
    try_files $uri @proxy;
  }

  location /sw.js {
    add_header Cache-Control "public, max-age=0";
    try_files $uri @proxy;
  }

  location @proxy {
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto https;
    proxy_set_header Proxy "";
    proxy_pass_header Server;

    proxy_pass http://127.0.0.1:3000;
    proxy_buffering off;
    proxy_redirect off;
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection $connection_upgrade;

    tcp_nodelay on;
  }

  location /api/v1/streaming {
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto https;
    proxy_set_header Proxy "";

    proxy_pass http://127.0.0.1:4000;
    proxy_buffering off;
    proxy_redirect off;
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection $connection_upgrade;

    tcp_nodelay on;
  }

  error_page 500 501 502 503 504 /500.html;
}
```

Activate the [nginx](http://nginx.org) configuration added:

```sh
cd /etc/nginx/sites-enabled
ln -s ../sites-available/example.com.conf
```

This configuration makes the assumption you are using [Let's Encrypt](https://letsencrypt.org) as your TLS certificate provider.

**If you are going to be using Let's Encrypt as your TLS certificate provider, see the
next sub-section. If not edit the `ssl_certificate` and `ssl_certificate_key` values
accordingly.**

## Let's Encrypt

This section is only relevant if you are using [Let's Encrypt](https://letsencrypt.org/)
as your TLS certificate provider.

### Generation Of The Certificate

We need to generate Let's Encrypt certificates.

**Make sure to replace any occurrence of 'example.com' with your Mastodon instance's domain.**

Make sure that [nginx](http://nginx.org) is stopped at this point:

```sh
systemctl stop nginx
```

We will be creating the certificate twice, once with TLS SNI validation in standalone mode and the second time we will be using the webroot method. This is required due to the way
[nginx](http://nginx.org) and the [Let's Encrypt](https://letsencrypt.org/) tool works.

```sh
letsencrypt certonly --standalone -d example.com
```

After that successfully completes, we will use the webroot method. This requires [nginx](http://nginx.org) to be running:

```sh
systemctl start nginx
# The letsencrypt tool will ask if you want issue a new cert, please choose that option
letsencrypt certonly --webroot -d example.com -w /home/mastodon/live/public/
```

### Automated Renewal Of Let's Encrypt Certificate

[Let's Encrypt](https://letsencrypt.org/) certificates have a validity period of 90 days.

You need to renew your certificate before the expiration date. Not doing so will make users of your instance unable to access the instance and users of other instances unable to federate with yours.

We can create a cron job that runs daily to do this:

```sh
nano /etc/cron.daily/letsencrypt-renew
```

Copy and paste this script into that file:

```sh
#!/usr/bin/env bash
letsencrypt renew
systemctl reload nginx
```

Save and exit the file.

Make the script executable and restart the cron daemon so that the script runs daily:

```sh
chmod +x /etc/cron.daily/letsencrypt-renew
systemctl restart cron
```

That is it. Your server will renew your [Let's Encrypt](https://letsencrypt.org/) certificate.

## Seafile Application Configuration

We will configure the Seafile application.

Change directory to `/var/www/html/seafile` and edit the [Seafile](https://seafile.com/) application configuration:

```sh
cd /var/www/html/seafile
```

Congratulations and welcome to the fediverse!
