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



## Create your service file

Example `/etc/systemd/system/example.service`:

```
[Service]
ExecStart=/usr/bin/node server/server.js
WorkingDirectory=/home/example/example
Restart=always
StandardOutput=syslog
StandardError=syslog
SyslogIdentifier=example
User=example
Group=example
Environment=NODE_ENV=production

[Install]
WantedBy=multi-user.target
```
