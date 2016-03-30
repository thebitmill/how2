# Ranger

## Shell Commands

NOTE: ! is a shortcut to quickly go into command mode and type initial `:shell `.
The following are available when running shell commands.

+ %f   the highlighted file
+ %d   the path of the current directory
+ %s   the selected files in the current directory.
+ %t   all tagged files in the current directory
+ %c   the full paths of the currently copied/cut files

IE. to create hard links for all copied/cut files:

```
$ shell ln %c
```

To zip selected files in current directory.

```
$ shell zip filename %s
```
