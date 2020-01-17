# Rsync

## Archive


```
$ rsync -av source dest
```

```
-a, --archive               archive mode; equals -rlptgoD (no -H,-A,-X)
-v, --verbose               increase verbosity
```

So the archive flag means:

```
-r, --recursive             recurse into directories
-l, --links                 copy symlinks as symlinks
-p, --perms                 preserve permissions
-t, --times                 preserve modification times
-g, --group                 preserve group
-o, --owner                 preserve owner (super-user only)
-D                          same as --devices --specials

    --devices               preserve device files (super-user only)
    --specials              preserve special files

-H, --hard-links            preserve hard links
-O, --omit-dir-times        omit directories from --times
-X, --xattrs                preserve extended attributes
```

So to simply copying the files (without owner, permissions etc):

```
$ rsync -a source dest
```
