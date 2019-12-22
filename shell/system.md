##

Check what process is listening on a port

```
$ netstat -nlp | grep :8080
```

netstat is obsolete, replaced with ss and other tools

```
$ ss -nlp | grep :8080
```
