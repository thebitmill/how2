# Remote Windows From Linux

## rdesktop

Seemed dead for a while (no updates since 2014), but development seems
to have been continued now (2016-03).

rdesktop does not seem to have support for using a `.rdp` file
(<https://github.com/rdesktop/rdesktop/issues/30>), but i like it
better otherwise.

```
$ rdesktop -u username -p - somedomain.io
```

Note: not passing `-p -` will give you a windows prompt where you can choose between the username
supplied, or switch to another.

## xfreerdp

Very ugly CLI syntax. But supports `.rdp` files.

```
$ xfreerdp /u:username /v:somedomain.io
```

```
$ xfreerdp file.rdp
```

