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
