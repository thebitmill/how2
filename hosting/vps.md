# Setting Up a CentOS Droplet

## Grow or create swapfile

Coming soon...

A general rule of thumb is twice as much swap space as you have RAM.

For a more thorough guide to Virtual Memory, see
<https://www.digitalocean.com/community/tutorials/how-to-configure-virtual-memory-swap-file-on-a-vps>

## Update system

```
# yum update
```

## Set timezone

We currently set the timezone for the server depending on where the market
for the delivered services are, not where the servers are. And for some
reason we always use Copenhagen instead of Stockholm.

```
# timedatectl set-timezone Europe/Stockholm
```

Sync clock with internet time:

```
# yum install -y ntp
# timedatectl set-ntp true
```

## EPEL

Extra Packages for Enterprise Linux (or EPEL) is a Fedora Special Interest
Group that creates, maintains, and manages a high quality set of additional
packages for Enterprise Linux, including, but not limited to, Red Hat
Enterprise Linux (RHEL), CentOS, Scientific Linux (SL) and Oracle Linux (OL).

EPEL packages are usually based on their Fedora counterparts and will never
conflict with or replace packages in the base Enterprise Linux distributions.
EPEL uses much of the same infrastructure as Fedora, including buildsystem,
bugzilla instance, updates manager, mirror manager and more.

In CentOS, EPEL is available as a package from the official repositories. Simply run:

```
# yum install -y epel-release
```

## Setup Firewall

Remove firewalld:

```
# systemctl stop firewalld
# yum remove -y firewalld
```

Install [ufw](https://wiki.archlinux.org/index.php/Uncomplicated_Firewall)

```
# yum install -y ufw
```

Setup basic rules

```
# ufw default deny
# ufw limit SSH
```

Enable rules

```
# ufw enable
```

## Install Some goodies

```
# yum install -y tmux vim git wget zsh
```

## Create a user

```
# useradd -m -s /bin/zsh -g users -G wheel foo
# passwd foo
```

On your local machine:

If you dont have an ssh key yet, run

```
$ ssh-keygen
```

Copy pubkey to vps:

```
$ ssh-copy-id foo@url
```

## Secure SSH

```
# vim /etc/ssh/sshd_config
```

Ensure these settings are set:

```
PermitRootLogin no
PubkeyAuthentication yes
PasswordAuthentication no
```

Make sure you can login with your user using pubkey before restarting the sshd
daemon!

```
# systemctl restart sshd
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
# curl -sL https://rpm.nodesource.com/setup_10.x | bash -
```

or if you are wimp and want to play it safe:

```
# curl -sL https://rpm.nodesource.com/setup_8.x | bash -
```

CAUTION: In general it is VERY dangerous to run an external script like the one
above as root. Only do this from trusted sources!

When the script is done, simply

```
# yum install -y nodejs
```

# Setup Databases

## Setup Postgres

See first sections of [the postgres section](../databases/postgres.html) for
installation and setup.

Open firewall ports if you want to allow remote connections

```
# ufw allow 
```

### Setup Redis

```
# yum install -y redis
```

Redis does not need any special setup.

```
# systemctl start redis
# systemctl enable redis
```

## Add user (with sudo access)

```
# useradd poopr -m -g users -G wheel
# passwd poopr
```

### Send SSH Keys

If you don't want to type the password in everytime you login
to the droplet (do this on your own computer, NOT the server).

```
$ ssh-keygen
$ ssh-copy-id poopr@13.37.13.37
```

## Deploying Node Applications with Systemd and Nginx

See [Node With Nginx](node-with-nginx.html).

Digital Ocean also has a great guide on deployinig Node applications on their droplets at 
<https://www.digitalocean.com/community/tutorials/how-to-deploy-node-js-applications-using-systemd-and-nginx>.
