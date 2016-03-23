# SED

## HTML

<http://stackoverflow.com/a/29530617/4273291>

Delete all lines between tags leaving tags:

```
$ sed '/<tag>/,/<\/tag>/{//!d}' input.txt
```

Delete all lines between tags including tags:

```
$ sed '/<tag>/,/<\/tag>/d' input.txt
```

To change in place use sed -i .... To change in place while backing up original
sed -i.bak ... which will save the original as input.txt.bak.
