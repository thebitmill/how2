# Deploying Node With Nginx

First you need a server or a VPS (Virtual Private Server).
Follow the [Digital Ocean how2](linux/digital-ocean.md) if you are
using Digital Ocean.

The following guide uses systemctl services to start, stop
and log Node applications. Another very good options is
the [PM2](https://github.com/Unitech/pm2) module. (If you
use PM2 you will probably still need to set up Nginx, see below)

Another solution is simply using a terminal multiplexer (like TMUX) to have
persistant sessions.

## 1. Create a user

At TCB we create a user for each Node application we run on a server.
A password is not added to the account, which prevents SSH access.
This means one must ssh in with another account first, and then `sudo su [username]`
to login as that user. Obviously you can add a password if you prefer.

```sh
# useradd liverpooltravel -m
# su liverpootravel
```

If you are using GitHub or Gitlab we recommend adding a deploy key
to the project you are hosting.

```
$ cd ~
$ ssh-keygen
$ cat .ssh/id_rsa.pub
```

Copy the resulting public key (DO NOT USE `.ssh/id_rsa`, that is your private
key!)

## 2. Clone your project

```
$ git clone [URL]
$ cd [project]
$ npm install
```

## 3. Setup Nginx

Example `nginx.conf`

```nginx
user  nginx;
worker_processes  1;

error_log  /var/log/nginx/error.log warn;
pid        /var/run/nginx.pid;


events {
    worker_connections  1024;
    multi_accept on;
}


http {
    include       /etc/nginx/mime.types;
    default_type  application/octet-stream;
    server_names_hash_bucket_size   128;

    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
        '$status $body_bytes_sent "$http_referer" '
        '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;

    sendfile        off;
    #tcp_nopush     on;

    #======Define proxy address=======
    upstream example {
        server 127.0.0.1:4043;
        keepalive 64;
    }

    #======Security settings=======
    server_tokens           off;
    client_max_body_size    4096k;
    client_header_timeout   10;
    client_body_timeout     10;
    keepalive_timeout       10 10;
    send_timeout            10;

    #======GZIP settings========
    gzip            on;
    gzip_disable    "msie6";
    gzip_min_length 1100;
    gzip_vary       on;
    gzip_proxied    any;
    gzip_buffers    16 8k;
    gzip_types      text/plain text/css application/json application/javascript application/x-javascript text/xml application/xml application/rss+xml text/javascript image/svg+xml application/x-font-ttf font/opentype application/vnd.ms-fontobject;


    include '/etc/nginx/sites-enabled/*.conf';
}
```

Example sites (place it in `/etc/nginx/sites-available`, and then create a symlink to it in `/etc/nginx/sites-enabled`)

```nginx
server {
    listen 80; 
    server_name  example.com www.example.com;
    rewrite ^/(.*) https://example.com/$1 permanent;
}

server {
    listen       443 ssl;
    server_name  example.com www.example.com;

    ssl_certificate /etc/letsencrypt/live/example.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/example.com/privkey.pem;

    root /home/example/example/public;
    index index.html;

    # for lets encrypt (--webroot)
    location /.well-known {
        allow all;
        root /usr/share/nginx/html;
    }

    location / {
        proxy_redirect      off;
        proxy_set_header    X-Real-IP           $remote_addr;
        proxy_set_header    X-Forwarded-For     $proxy_add_x_forwarded_for;
        proxy_set_header    X-Forwarded-Proto   $scheme;
        proxy_set_header    Host                $http_host;
        proxy_set_header    X-NginX-Proxy       true;
        proxy_set_header    Connection          "";
        proxy_http_version  1.1;
        #proxy_cache    one;
        #proxy_cache_key    sfs$request_uri$scheme;
        proxy_pass  http://127.0.01:1337;
    }

    #======Do not serve files starting with . or $=======
    #location ~ /\. { access_log off; log_not_found off; deny all; }
    location ~ ~$  { access_log off; log_not_found off; deny all; }

}
```

## Create your service file

Example `/etc/systemd/system/example.service`:

```
[Service]
ExecStart=/usr/bin/node server/server.js
WorkingDirectory=/home/bitmill/bitmill
Restart=always
StandardOutput=syslog
StandardError=syslog
SyslogIdentifier=bitmill
User=bitmill
Group=bitmill
Environment=NODE_ENV=production

[Install]
WantedBy=multi-user.target
```
