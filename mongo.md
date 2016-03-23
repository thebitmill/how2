# Mongo

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
