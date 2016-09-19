# Ranger

## Previews

### JSX

Add this line under the `case "$extension" in` in `scope.sh`

```
jsx)
    try highlight --out-format=ansi --syntax=js "$path" && { dump | trim; exit 5; } || exit 2;;
```

### Archives

Make sure you install `atool` package, as well as executables for the specific
archives you want to preview (unrar, tar etc)

## Shell Commands

NOTE: `!` and `s` are shortcuts to quickly go into command mode and type initial `:shell `.
The following are available when running shell commands.

+ %f   the highlighted file
+ %d   the path of the current directory
+ %s   the selected files in the current directory.
+ %t   all tagged files in the current directory
+ %c   the full paths of the currently copied/cut files

IE. to create hard links for all copied/cut files:

```
:shell ln %c
```

To zip selected files in current directory.

```
:shell zip filename %s
```

To uglify and gzip a js file

```
:shell uglifyjs %f -c -m | gzip -c9 > bundle.min.js.gz
```

Cat and uglify all selected files

```
:shell cat %s | uglifyjs - -c -m > bundle.min.js
```
