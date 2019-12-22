# PostGres

## Installation

1. Find correct yum repo (right click and save link address) from https://yum.postgresql.org/repopackages.php
2. Install it, eg `# yum install https://download.postgresql.org/pub/repos/yum/10/redhat/rhel-7-x86_64/pgdg-redhat10-10-2.noarch.rpm`
3. `# yum list postgres*`
4. `# yum install postgresql10-server`
3. `# /usr/pgsql-10/bin/postgresql-10-setup initdb`
4. `# systemctl start postgresql-10`
5. `# systemctl enable postgresql-10`

### Firwall: UFW

Allow access from all remotes:

```
# ufw allow from any to any port 5432
```

Allow access from specific ip:

```
# ufw allow from 15.15.15.0/24 to any port 5432
```

### Firewall: firewalld

Allow access from all remotes:

```
# firewall-cmd --add-port=5432/tcp --permanent
# systemctl restart firewalld
```

## Enable Login with password instead of relying on Unix username

1. `# su postgres`
2. `$ psql`
3. `\password`
4. `\q`
5. `# vi /var/lib/pgsql/10/data/pg_hba.conf` and switch all ident/peer to md5
6. `# systemctl restart postgresql-10`
7. Add `host  all  all  all  md5` if you want to allow remote connections with password

## Setup new User & Database

1. `# su postgres`
2. `$ psql`
3. execute `CREATE USER user_name ENCRYPTED PASSWORD 'password';`
4. execute `CREATE DATABASE db_name OWNER user_name;`
5. If you want to edit the new database as the new user, run `\c db_name user_name`.

NOTE: difference between `CREATE USER` and `CREATE ROLE` is that with CU `LOGIN` is assumed by default.

## Change user credentials

From <https://stackoverflow.com/a/12721095>

1. Login in as postgres user
2. `ALTER USER user_name WITH PASSWORD 'new_password';`
3. Delete psql history `$ rm ~/.psql_history`

Rename user:

`ALTER USER myuser RENAME TO newname;`

## Dump Table Structure (no data)

```
$ pg_dump -h postgres.example.io -p 1337 -d poopr -U poopr_supreme -t regions -s -O > dump.sql
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

## Copy All Databases between servers

if you dont have to enter a password for the source server, this works well:

```
$ pg_dumpall -h db.bitmill.io | psql
```

otherwise:

```
$ pg_dumpall > dump_file
$ scp dump_file user@server:~/
$ psql -f dump_file
```

## Reset sequence

```
ALTER SEQUENCE seq RESTART WITH 1;
```

## types

timestamptz is shorthand for timestamp without time zone
