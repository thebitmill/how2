# Node With Nginx

First you need a server or a VPS (Virtual Private Server).
Follow the [VPS Guide](hosting/vps.md) if you need help.

The following guide uses systemctl services to start, stop and log Node
applications. Another very good option is the
[PM2](https://github.com/Unitech/pm2) module. (If you use PM2 you will probably
still need to set up Nginx, see below) Another solution is simply using a
terminal multiplexer (like TMUX) to have persistant sessions.

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

Ensure you can start your project.

## 3. Create your service file

```
# wget https://raw.githubusercontent.com/thebitmill/rc-vps/master/example.service -O /etc/systemd/system/foobar.service 
# vim $_
```

Change ExecStart, WorkingDirectory, User, Group and/or Environment to your
likings.

Ensure the service starts fine:

```
# systemctl start foobar
$ systemctl status foobar
```

## 3. Setup Nginx

## 3.1 Install

```
# yum install nginx
```

## 3.2 Setup

```
# cd /etc/nginx
```

Use Bitmill default nginx conf:

1. Backup original `# mv /etc/nginx/nginx.conf /etc/nginx/nginx.conf.original`
2. `# wget https://raw.githubusercontent.com/thebitmill/rc-vps/master/nginx.conf -O /etc/nginx/nginx.conf`
3. `# mkdir /etc/nginx/sites-{available,enabled}`
4. `# systemctl start nginx`
4. `# systemctl enable nginx`

With this setup, site configurations are places in sites-available. 
To enable a site symlink its configuration inte sites-enabled:

```
# ln -rs sites-available/foobar.conf sites-enabled/
```

The following command fixes SELinux problems:

```
# setsebool -P httpd_can_network_connect 1
```

See <https://stackoverflow.com/a/24830777/4273291> for more details.

## 3.3.A: HTTPS site with Lets Encrypt

1. `# wget https://raw.githubusercontent.com/thebitmill/rc-vps/master/https.conf -O /etc/nginx/sites-available/example.conf`
2. Comment out lines 4 - 22 (<https://github.com/thebitmill/rc-vps/blob/master/https.conf#L4-L22>)
3. `# ln -s /etc/nginx/sites-available/example.conf /etc/nginx/sites-enabled/` 
4. `# systemctl reload nginx`
5. `# certbot --email email@example.com certonly -a webroot --webroot-path=/usr/share/nginx/html -d examples.com -d www.examples.com`
6. Activate lines 4 - 22 again
7. `# systemctl reload nginx`
8. Certbot will setup cron jobs automatically (if cron is installed, not sure
   how it handles other job runners)

Verify with a modified verion at this address:

<https://www.ssllabs.com/ssltest/analyze.html?d=example.com&latest>

## 3.3.B: HTTP site

1. `# wget https://raw.githubusercontent.com/thebitmill/rc-vps/master/http.conf -O /etc/nginx/sites-available/example.conf`
3. `# vim $_`
2. `# ln -s /etc/nginx/sites-available/example.conf /etc/nginx/sites-enabled/` 
3. `# systemctl reload nginx`

### Firwall: UFW

```
# ufw allow proto tcp from any to any port 80,443
```

### Firewall: firewalld

```
# firewall-cmd --add-port=80/tcp --permanent
# firewall-cmd --add-port=443/tcp --permanent
```
