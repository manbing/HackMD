# [repo](https://gerrit.googlesource.com/git-repo)

## Install
You can install it manually as well as it's a single script.
```
$ mkdir -p ~/.bin
$ PATH="${HOME}/.bin:${PATH}"
$ curl https://storage.googleapis.com/git-repo-downloads/repo > ~/.bin/repo
$ chmod a+rx ~/.bin/repo
```

## Manifest


## Command
```
$ repo forall -vc "git stash"
$ repo start --all ${branch}
$ repo checkout ${branch}
$ repo sync
$ repo rebase
```


## Reference
[Repo command reference](https://source.android.com/docs/setup/reference/repo?hl=zh-tw)

[repo-init](https://manpages.ubuntu.com/manpages/noble/man1/repo-init.1.html)