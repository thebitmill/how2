# Mongo

## Securing Mongo

First, start your mongo deamon without authentication.

1. `$ mongo admin`
2. `db.createUser({ user: 'admin-supreme', pwd: 'a-good-password', roles: [ 'root' ] })
3. `use newdatabase`
4. `db.createUser({ user: 'nd-supreme', pwd: 'a-good-password', roles: [ 'dbOwner' ] })

Uncomment the line

```
auth=true
```

and make sure

```
noauth=true
```

is commented out.

## Dump

Dumping a database is easy:

```
$ mongodump -d dbName
```

With auth:

```
$ mongodump -d dbName -u dbUser -p
```

## Restore a databse

```
$ mongorestore -d dbName dump/dbName
```
