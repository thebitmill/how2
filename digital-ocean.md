# Setting Up a CentOS Droplet

`#` means to execute commands as root.

## Register

First you need to register an account at Digital Ocean. If you register using
our referal link (<https://www.digitalocean.com/?refcode=a3eb2e3ed7ce>) you get
$10 for free. If you activate your Github Student Developer Pack
(<https://education.github.com/pack>), you get an additional $50.

## Create a Droplet

We have decided to use CentOS for all our production environments. In order to
get familiar with the Fedora-based Linux distributions, we recommend using
Fedora or CentOS on your droplets. All guides relating to servers and droplets
will most likely be Fedora/CentOS specific.

The smallest droplet is sufficient for most purposes.

## Setup Swapfile

You probably have the smallest droplet on digital ocean, which has 512mb RAM.
For reasons unknown, this quickly becomes too litte and yum and npm have a
tendency to start failing. There for it is highly recommended to create a
swapfile and enable the virtual memory.

A general rule of thumb is twice as much swap space as you have RAM. Run

```
# fallocate -l 1G /swapfile
```

or

```
# dd if=/dev/zero of=/swapfile bs=1024k count=1000
```

Set the permissions:

```
# chmod 600 /swapfile
```

Then run the following to initialize the swap filesystem, and activate it

```
# mkswap /swapfile
# swapon /swapfile
```

To get status of your memory, run

```
$ free
```

To enable the new swapfile on boot, add the following line to /etc/fstab

```
/swapfile   swap    swap    sw  0   0
```

or run

```
# echo "/swapfile   swap    swap    sw  0   0" >> /etc/fstab
```

If you do not know your root password, but have sudo access, run the following instead of the above command

```
$ sudo su -c 'echo "/swapfile   swap    swap    sw  0   0" >> /etc/fstab'
```

For a more thorough guide to Virtual Memory, see
<https://www.digitalocean.com/community/tutorials/how-to-configure-virtual-memory-swap-file-on-a-vps>

## EPEL

Extra Packages for Enterprise Linux (or EPEL) is a Fedora Special Interest
Group that creates, maintains, and manages a high quality set of additional
packages for Enterprise Linux, including, but not limited to, Red Hat
Enterprise Linux (RHEL), CentOS, Scientific Linux (SL) and Oracle Linux (OL).

EPEL packages are usually based on their Fedora counterparts and will never
conflict with or replace packages in the base Enterprise Linux distributions.
EPEL uses much of the same infrastructure as Fedora, including buildsystem,
bugzilla instance, updates manager, mirror manager and more.

### Yum install

In CentOS, EPEL is available as a package from the official repositories. Simply run:

```
# yum install epel-release
```

### Manual Install

If you wanna be crazy cool, you can install EPEL in the good old fashion manual
way. First head to
http://ftp.lysator.liu.se/pub/epel/7/x86_64/repoview/epel-release.html and copy
the url from the link to the latest package. Download the package to your
droplet with wget:

```
$ wget http://ftp.lysator.liu.se/pub/epel/7/x86_64/e/epel-release-7-5.noarch.rpm
```

In the same directory, run

```
# rpm -ivh epel-release-7-5.noarch.rpm
```

## Install RPM packages

(gcc gcc-c++ is to be able to install node from nodesource repos)

```
# yum install git vim redis ntp nginx mongodb mongodb-server tmux gcc gcc-c++
```

## Set timezone

We currently set the timezone for the server depending on where the market
for the delivered services are, not where the servers are. And for some
reason we always use Copenhagen instead of Stockholm.

```
# ln -fs /usr/share/zoneinfo/Europe/Copenhagen /etc/localtime
```

(-f flag removes existing, if any, symlink)

And start NTP (to keep clocks insync)

```
# systemctl start ntpd
# systemctl enable ntpd
```

## Install Ranger

Ranger is an awesome terminal filemanager. As per usual with our tools, it has
a relatively steep learning curve; although the basic commands are pretty quick
to grasp. Ranger is not in the offical repos.

```
$ wget http://nongnu.org/ranger/ranger-stable.tar.gz
$ tar xvzf ranger-stable.tar.gz
$ cd ranger-XXX
# make install
```

## Install Node from Nodesource

Since the Node & NPM versions in EPEL are painfully old, we use Nodesource
repos to install them. More information can be found at
<https://github.com/nodesource/distributions>.

```
# curl -sL https://rpm.nodesource.com/setup_4.x | bash -
```
or
```
# curl -sL https://rpm.nodesource.com/setup_5.x | bash -
```

CAUTION: In general it is VERY dangerous to run an external script like the one
above as root.  Only do this from trusted sources!

When the script is done, simply

```
# yum install nodej
```

## Install global Node modules

```
# npm install -g thecodebureau/tcb-gulp nodemon
```

## Setup Firewall & SSH

Something good about security.

### Open port for Mongo

```
# firewall-cmd --add-port=27017/tcp --permanent
# systemctl restart mongod
```

## Setup Mongo

Change `/etc/mongod.conf` line 6 or `bind_ip = 127.0.0.1` to `bind_ip = 0.0.0.0`.

## Add user (with sudo access)

```
# useradd poopr -m -g users -G wheel
# passwd poopr
```

## Setup Redis

Redis does not need any special setup.

```
# systemctl start redi
# systemctl start redi
```

## Deploying Node Applications with Systemd and Nginx

Digital Ocean has a great guide on deployinig Node applications on their droplets:
<https://www.digitalocean.com/community/tutorials/how-to-deploy-node-js-applications-using-systemd-and-nginx>

### Sample systemd unit file:

```
[Service]
ExecStart=/usr/bin/node /home/ams/anderssonmotorsport.nu/server/server.js
WorkingDirectory=/home/ams/anderssonmotorsport.nu
Restart=always
StandardOutput=syslog
StandardError=syslog
SyslogIdentifier=ams
User=ams
Group=ams
Environment=NODE_ENV=production PWD=/home/ams/anderssonmotorsport.nu

[Install]
WantedBy=multi-user.target
```
