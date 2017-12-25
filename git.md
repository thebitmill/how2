# GIT

## Checkout previously checked out branch

```
$ git checkout -
```

which is a shortcuts for

```
$ git checkout @{-1}
```

<https://stackoverflow.com/a/7207542>

## Remove a remote tag

```
$ git tag -d 12345
$ git push origin :refs/tags/12345
```

## Remove all local tags

(make sure to remove all remote tags before calling this command if you intend to
remove both remote and local)
```
$ git tag | xargs git tag -d
```
To delete remote tags (before deleting local tags) simply do:

## Remove all remote tags

```
$ git tag -l | xargs -n 1 git push --delete origin
```

faster:

```
$ git tag | xargs -L 1 | xargs git push origin --delete
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

## Diff


Latest 
```
$ git diff HEAD^ HEAD
```

or

```
$ git diff HEAD^!
```

Current branch, diff between commits 2 and 3 times back

```
$ git diff HEAD~3 HEAD~2
```

## Output a version of a file

```
$ git show COMMIT:./filename
```

## Checkout remote branch

```
$ git checkout -b experimental origin/experimental
```

## List commits for a specific file

```
$ git log --follow filename
```

## Merging detached HEAD back into branch

Well, you arent actually mergin anything. You just need
to point master to the detached HEAD commit.

```
$ git branch -f master HEAD && git checkout master
```

Source: <http://stackoverflow.com/a/5772882>


## List all tracked files

```
$ git ls-tree -r master --name-only
```

Source: <https://stackoverflow.com/a/15606995>
