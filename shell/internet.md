# Internet

## Curl

```
curl -H "Accept: application/json" localhost:1337/api/poops
```

## DNS

Use `host` to query DNS entries (`bind-tools` package in Arch Linux)

```
$ host -a example.io
```

Use `dnsget` to query DNS entries (udns package in Arch Linux)

```
$ dnsget example.io -a
```

Trace DNS servers (dnstracer package in Arch Linux)

```
$ dnstracer example.io
```

