# GIT

## Remove a remote tag

```
$ git tag -d 12345
$ git push origin :refs/tags/12345
```

## Remove a remote branch

```
$ git push origin --delete <branchName>
```

or

```
$ git push origin :<branchName>
```

## Remove a submodule

1. `$ git submodule deinit [module]`
2. Delete the relevant line from the `.gitmodules` file.
3. Delete the relevant section from `.git/config`.
4. `$ rm -rf .git/modules/[module]`
5. `$ git rm --cached path_to_submodule (no trailing slash)`.
6. Commit the superproject.
7. Delete the now untracked submodule files.

## Change timestamp on Git Commit

If you wanted to change the dates of commit
119f9ecf58069b265ab22f1f97d2b648faf932e0, you could do so with something like
this:

```
git filter-branch --env-filter \
    'if [ $GIT_COMMIT = 119f9ecf58069b265ab22f1f97d2b648faf932e0 ]
     then
         export GIT_AUTHOR_DATE="Fri Jan 2 21:38:53 2009 -0800"
         export GIT_COMMITTER_DATE="Sat May 19 01:01:01 2007 -0700"
     fi'
```


Source: <http://stackoverflow.com/a/454750/4273291>

## Add an earlier version of a file in a earlier commit

1. `$ git rebase -i COMMIT`
2. change 'pick' to 'edit' on the commit where you want to add the file
3. add the file, `git commit --amend`
4. `$ git rebase --continue`
5. you will get conflict (both added)
6. `$ git checkout --theirs filename`
7. `$ git add filename`
8. `$ git rebase --continue`

## Output a version of a file

```
$ git show COMMIT:./filename
```

## List commits for a specific file

```
$ git log --follow filename
```
