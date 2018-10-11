Sources: <https://www.rosehosting.com/blog/generate-password-linux-command-line/>

All examples generate a 16 character long password
## OpenSSL

```
$ openssl rand -base64 16 | colrm 17
```

## GPG

```
$ gpg --gen-random --armor 1 16 | colrm 17
```

## /dev/urandom

```
$ head /dev/urandom | tr -dc '[:graph:]' | fold -w16 | sed '$d' | shuf -n1
```

or maybe (piping all above commands to wc gives 17, this below will give 16 characters)

```
$ cat /dev/urandom | tr -dc A-Za-z0-9 | head -c16
```

