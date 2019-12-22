# Pacman

## Find package owning file

Local:

```
$ pacman -Qo /usr/bin/which
```

Remote

```
# pacman -Fy
$ pacman -Fo /usr/bin/which
```

## If you get errors like

```
error: krb5: signature from "Levente Polyak (anthraxx) <levente@leventepolyak.net>" is unknown trust
:: File /var/cache/pacman/pkg/krb5-1.13.7-1-x86_64.pkg.tar.xz is corrupted (invalid or corrupted package (PGP signature)).
```

```
# pacman -Sy archlinux-keyring
```


## Restore all packages to a specific date

<https://wiki.archlinux.org/index.php/Arch_Linux_Archive#How_to_restore_all_packages_to_a_specific_date>
