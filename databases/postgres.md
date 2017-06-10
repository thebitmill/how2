# PostGres

## Installation

1. Find correct yum repo (right click and save link address) from https://yum.postgresql.org/repopackages.php
2. Install it, eg `# yum install https://download.postgresql.org/pub/repos/yum/9.6/redhat/rhel-7-x86_64/pgdg-centos96-9.6-3.noarch.rpm`
3. `# yum list postgres*`
4. `# yum install postgresql96-server`
3. `# /usr/pgsql-9.6/bin/postgresql96-setup initdb`
4. `# systemctl start postgresql-9.6`
5. `# systemctl enable postgresql-9.6`

## Enable Login with password instead of relying on Unix username

1. `# su postgres`
2. `$ psql`
3. `\password`
4. `\q`
5. `# vi /var/lib/pgsql/9.6/data/pg_hba.conf` and switch all ident/peer to md5
6. `# systemctl restart postgresql-9.6 `
7. Add `host    all             all             all                     md5` if you want to allow remote connections with password


## Setup new User & Database

1. `# su postgres`
2. `$ psql`
3. execute `CREATE USER user_name ENCRYPTED PASSWORD 'password';`
4. execute `CREATE DATABASE db_name OWNER user_name;`

NOTE: difference between `CREATE USER` and `CREATE ROLE` is that with CU `LOGIN` is assumed by default.


## Dump Table Structure (no data)

```
$ pg_dump -h postgres.example.io -p 6543 -d poopr -U poopr_supreme -t regions -s -O > dump.sql
```

\-h: host
\-d: database
\-t: table
\-s: schema only (no data)
\-O: no owner information

# Copy Table Structure Between Databases

```
$ pg_dump -h db.example.io -d db01 -U db01_user -s -O -F c | pg_restore -h db02.example.io -F c -c -O -d db02 -U db02_user
```

pg\_dump options
\-F: format (c: Output a custom-format archive suitable for input into pg\_restore)

pg\_restore options
\-F: format (c: The archive is in the custom format of pg\_dump.)

# Copy Database

(same as above, just omit `-s` flag in `pg_dump` command.)

```
$ pg_dump -h example.io -d db01 -U db01_user -O -F c | pg_restore -h db02.example.io -F c -c -O -d db02 -U db02_user
```

pg\_dump options
\-F: format (c: Output a custom-format archive suitable for input into pg\_restore)

pg\_restore options
\-F: format (c: The archive is in the custom format of pg\_dump.)
