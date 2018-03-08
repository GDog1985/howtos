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
```sh
 yum  -y install tmux
 ```

## Dependency Installation

#### Explanation of the dependencies

- imagemagick -  uses imagemagick for image related operations
- nginx - nginx is our frontend web server
- epel-release -
- python
- mariadb

```sh
yum -y install epel-release
yum -y install python-imaging MySQL-python python-simplejson python-setuptools mariadb mariadb-server nginx

```
## Seafile install

```sh
mkdir -p /var/www/seafile
cd /var/www/seafile

wget https://download.seadrive.org/seafile-server_6.2.5_x86-64.tar.gz
tar -xzvf seafile-server_6.2.5_x86-64.tar.gz
mv seafile-server-6.2.5 seafile-server
cd seafile-server/
```

## Mysql  Configuration First


```sh
systemctl start mariadb
mysql_secure_installation

mysql -u root -p
```
```sh
create database ccnet_db character set = 'utf8';
create database seafile_db character set = 'utf8';
create database seahub_db character set = 'utf8';

create user seacloud@localhost identified by 'your password';

grant all privileges on ccnet_db.* to seacloud@localhost identified by 'your password';
grant all privileges on seafile_db.* to seacloud@localhost identified by 'your password';
grant all privileges on seahub_db.* to seacloud@localhost identified by 'your password';
flush privileges;
exit
```
## After Mysql is all setup

```sh
./setup-seafile-mysql.sh

./seafile.sh start
./seahub.sh start
./seafile.sh stop
./seahub.sh stop
```

```sh
cd /var/www/
chown -R nginx:nginx *
chown -R nginx:nginx /tmp/seahub_cache

```

## Nginx Configuration

You need to configure [nginx](http://nginx.org) to serve your [Seafile](https://seaflie.com) instance.

**Reminder: Replace all occurrences of example.com with your own instance's domain or sub-domain.**

`cd` to `/etc/nginx/sites-available` and open a new file:

```sh
nano /etc/nginx/sites-available/example.com.conf`
```
Copy and paste the following and make edits as necessary:

```sh
map $http_upgrade $connection_upgrade {
  default upgrade;
  ''      close;
}


server {
        listen        80;
        server_name   seafile.example.com;
        return 301  https://$host$request_uri;
}

server {
    listen 443 ssl;
    server_name seafile.example.com;
    ssl on;
    ssl_protocols           TLSv1 TLSv1.1 TLSv1.2;
    ssl_certificate         /etc/nginx/ssl/server.crt;
    ssl_certificate_key    /etc/nginx/ssl/server.key;

    ssl_ciphers  'ECDHE-RSA-AES256-GCM-SHA384:ECDHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384:DHE-RSA-AES128-GCM-SHA256:ECDHE-RSA-AES256-SHA384:ECDHE-RSA-AES128-SHA256:ECDHE-RSA-AES256-SHA:ECDHE-RSA-AES128-SHA:DHE-RSA-AES256-SHA256:DHE-RSA-AES128-SHA256:DHE-RSA-AES256-SHA:DHE-RSA-AES128-SHA:ECDHE-RSA-DES-CBC3-SHA:EDH-RSA-DES-CBC3-SHA:AES256-GCM-SHA384:AES128-GCM-SHA256:AES256-SHA256:AES128-SHA256:AES256-SHA:AES128-SHA:DES-CBC3-SHA:HIGH:!aNULL:!eNULL:!EXPORT:!CAMELLIA:!DES:!MD5:!PSK:!RC4';
    ssl_dhparam   /etc/nginx/ssl/dhparam.pem;
    ssl_prefer_server_ciphers  on;

    location / {
        fastcgi_pass    127.0.0.1:8000;
        fastcgi_param   SCRIPT_FILENAME     $document_root$fastcgi_script_name;
        fastcgi_param   PATH_INFO           $fastcgi_script_name;

        fastcgi_param   SERVER_PROTOCOL        $server_protocol;
        fastcgi_param   QUERY_STRING        $query_string;
        fastcgi_param   REQUEST_METHOD      $request_method;
        fastcgi_param   CONTENT_TYPE        $content_type;
        fastcgi_param   CONTENT_LENGTH      $content_length;
        fastcgi_param   SERVER_ADDR         $server_addr;
        fastcgi_param   SERVER_PORT         $server_port;
        fastcgi_param   SERVER_NAME         $server_name;
        fastcgi_param   REMOTE_ADDR         $remote_addr;

        access_log      /var/log/nginx/seahub.access.log;
        error_log       /var/log/nginx/seahub.error.log;
        fastcgi_read_timeout 36000;
    }

    # Reverse Proxy for seahub
    location /seafhttp {
        rewrite ^/seafhttp(.*)$ $1 break;
        proxy_pass http://127.0.0.1:8082;
        client_max_body_size 0;
        proxy_connect_timeout  36000s;
        proxy_read_timeout  36000s;
        proxy_send_timeout  36000s;
        send_timeout  36000s;
    }

    #CHANGE THIS PATH WITH YOUR OWN DIRECTORY
    location /media {
        root /var/www/seafile/seafile-server/seahub;
    }
    #SEAFDAV ENABLE HERE BY DEFAULT
    location /seafdav {
        fastcgi_pass    127.0.0.1:8080;
        fastcgi_param   SCRIPT_FILENAME     $document_root$fastcgi_script_name;
        fastcgi_param   PATH_INFO           $fastcgi_script_name;

        fastcgi_param   SERVER_PROTOCOL     $server_protocol;
        fastcgi_param   QUERY_STRING        $query_string;
        fastcgi_param   REQUEST_METHOD      $request_method;
        fastcgi_param   CONTENT_TYPE        $content_type;
        fastcgi_param   CONTENT_LENGTH      $content_length;
        fastcgi_param   SERVER_ADDR         $server_addr;
        fastcgi_param   SERVER_PORT         $server_port;
        fastcgi_param   SERVER_NAME         $server_name;
        fastcgi_param   HTTPS               on;
        fastcgi_param   HTTP_SCHEME         https;

        client_max_body_size 0;
        proxy_connect_timeout  36000s;
        proxy_read_timeout  36000s;
        proxy_send_timeout  36000s;
        send_timeout  36000s;

        # This option is only available for Nginx >= 1.8.0. See more details below.
        proxy_request_buffering off;

        access_log      /var/log/nginx/seafdav.access.log;
        error_log       /var/log/nginx/seafdav.error.log;
        }

    }


```


## Activate the [nginx](http://nginx.org) configuration added:

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

**Make sure to replace any occurrence of 'example.com' with your Seafile instance's domain.**

Make sure that [nginx](http://nginx.org) is stopped at this point:

```sh
systemctl stop nginx
```

We will be creating the certificate twice, once with TLS SNI validation in standalone mode and the second time we will be using the webroot method. This is required due to the way
[nginx](http://nginx.org) and the [Let's Encrypt](https://letsencrypt.org/) tool works.

```sh
 sudo certbot --nginx
 or
 sudo certbot --nginx certonly

```



### Automated Renewal Of Let's Encrypt Certificate

[Let's Encrypt](https://letsencrypt.org/) certificates have a validity period of 90 days.

You need to renew your certificate before the expiration date. Not doing so will make users of your instance unable to access the instance and users of other instances unable to federate with yours.

We can create a cron job that runs monthly to do this:

```sh
nano /etc/cron.monthly/letsencrypt-renew
```

Copy and paste this script into that file:

```sh
#!/usr/bin/env bash
certbot renew
systemctl reload nginx
```

Save and exit the file.

Make the script executable and restart the cron daemon so that the script runs daily:

```sh
chmod +x /etc/cron.monthly/letsencrypt-renew
systemctl restart cron
```


That is it. Your server will renew your [Let's Encrypt](https://letsencrypt.org/) certificate.


## Seafile systemd Service Files

cd /etc/systemd/system/

```sh
 nano  seafile.service
```

```sh
[Unit]
Description=Seafile Server
Before=seahub.service
After=network.target mariadb.service

[Service]
Type=oneshot
ExecStart=/var/www/seafile/seafile-server/seafile.sh start
ExecStop=/var/www/seafile/seafile-server/seafile.sh stop
RemainAfterExit=yes
User=nginx
Group=nginx

[Install]
WantedBy=multi-user.target

```

```sh
nano seahub.service
```
```sh
[Unit]
Description=Seafile Hub
After=network.target seafile.target mariadb.service

[Service]
Type=oneshot
ExecStart=/var/www/seafile/seafile-server/seahub.sh start-fastcgi
ExecStop=/var/www/seafile/seafile-server/seahub.sh stop
RemainAfterExit=yes
User=nginx
Group=nginx

[Install]
WantedBy=multi-user.target
```

## Seafile Application Configuration

We will configure the Seafile application.

Change directory to `/var/www/html/seafile` and edit the [Seafile](https://seafile.com/) application configuration:

```sh
cd /var/www/html/seafile/

```
```sh
nano conf/ccnet.conf
SERVICE_URL = https://
```
## ldap / freeipa configuration

```sh
[LDAP]
HOST = ldap://
BASE = dc=example,dc=org
USER_DN = cn=Directory Manager
PASSWORD = your password
LOGIN_ATTR = mail

[LDAP_SYNC]
ENABLE_USER_SYNC = true
DEACTIVE_USER_IF_NOTFOUND = true
SYNC_INTERVAL = 60
USER_OBJECT_CLASS = userOfNames
ENABLE_EXTRA_USER_INFO_SYNC = true
FIRST_NAME_ATTR = givenName
LAST_NAME_ATTR = sn
```
## SEAFDAV

nano conf/seafdav.conf

```sh
[WEBDAV]
enabled = true
port = 8080
fastcgi = true
share_name = /seafdav
```
