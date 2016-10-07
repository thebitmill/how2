# RPM

## List files belonging to a package

Use following syntax to list the files for already INSTALLED package:

```
$ rpm -ql package-name
```

Use following syntax to list the files for RPM package:

```
$ rpm -qlp package.rpm
```

## Remove a repo

```
$ rpm -qf /etc/yum.repos.d/rpmforge.repo
rpmforge-release-0.5.1-1.el5.rf
# yum remove rpmforge-release
```
