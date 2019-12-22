## UUIDs

All:

```
$ lsblk -o +UUID
```

or

```
$ ls -lah /dev/disk/by-uuid
```

Single

```
# blkid /dev/sdXY
# blkid -s UUID -o value /dev/sdXY
```

## Test Drive Speed

```
sudo hdparm -Tt /dev/sda
```
