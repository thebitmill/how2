# Shell

Commands

Recursively search all files in a folder for a pattern with 'grep'

```
$ grep -r [pattern]
```
If you want to limit your scope:

```
$ grep -r --include=*.js --exclude-dir=node_modules/ [patter]
```

or use find:

```
$ find . -iname '*.js' -not -path './node_modules/*'| xargs grep -r console.log
```

## Search & replace in files

You can use find.

```
$ find ./ -type f -exec sed -i -e 's/windows/linux/g' {} \;
```

Piping to xargs instead of -exec if you need more control, but is often OP.

```
$ find . -name "*.txt" | xargs -0 sed -i '' -e 's/windows/linux/g'
```

If you more effecient, and only search in files with the string, use grep and pipe to xargs

```
$ grep -rl 'windows' | xargs sed -i 's/windows/linux/g'
```

## Delete lines:

```
$ grep -rl 'console.log' | xargs sed -i '/console.log/d'
```

## Rename files

Change extension of files:

```
$ find . -name '*.md' -exec rename .md .txt {} +
```

With find and sed (oops does not work):

```
$ find /tmp -iname "*.txt" -exec bash -c 'mv $0 $(echo "$0" | sed -r \"s|\.txt|.cpp|g\")' '{}' \;
```


## Change permissions of files with 'find'

To change permissions of all folders in current directory (recursive)

```
$ find . -type d -exec chmod 777 {} +
```

To change permission of all folders in current directory (recursive)

```
$ find . -type d -exec chmod 777 {} +
```

## Check battery status

Full info 

```
$ upower -e
$ upower -i /org/freedesktop/UPower/devices/battery_BAT0
```

Or if upower is too much bloatware and lower level is demanded, first

```
$ ls /sys/class/power_supply/BAT0
```

then, for example

```
$ cat /sys/class/power_supply/BAT0/status
```

## Generate Pi

Source: <http://superuser.com/a/275567/435917>

You will need bc (bench calculator). To get Pi to a precision of 10e-1000:

```
$ echo "scale=1000; 4*a(1)" | bc -l
```

List most used commands

Note, you can usually use gawk, awk nor nawk.

```
$ history | awk '{print $2}' | awk 'BEGIN {FS="|"}{print $1}' | sort | uniq -c | sort -n | tail | sort -nr
```

## Remove Line From Huge File

```
$ tail -n+STARTLINE | head -nANTALRADER
```

\+ ska vara med
