---
title: 'Git, Version Control'
tags: [Linux]

---

# Git, Version Control 
###### tags: `Linux`

## relativing file/directory
``` console
$ repository/.git/
$ .gitignore
$ ~/.gitconfig
$ source /usr/share/bash-completion/completions/git
```


## basic
``` console
git init --bare
git fetch <remote>
git pull = git fetch + git merge
git add -patch <file>
git blame <file>
git branch -a
git stash
git checkout <file>
git diff <file>
git bisect
(in vim)
:Gblame
git add --patch <file>
git checkout --patch <file>
git remote add <name> <url>
git clang-format
git count-objects -v
git reflog
git reflog expire --expire-unreachable=now --all
git ls-files
```

## git-archive
``` console
$ git archive --format=tar --prefix="cindy/" <ref> > ../cindy.tar
```

## git-apply
``` console
$ git apply --stat a_file.patch
$ git apply --check a_file.patch

```

## [git-am](https://man7.org/linux/man-pages/man1/git-am.1.html)
> Apply a series of patches from a mailbox

git am <*.patch>

git am -3 <001.patch>
> When the patch does not apply cleanly, fall back on 3-way merge if the patch records the identity of blobs it is supposed to apply to and we have those blobs available locally.
    
## git-branch
```
git branch -a --contains <ref>
git branch <new branch> <exist branch>
```

## git-bisect
> Use binary search to find the commit that introduced a bug
    
## git-blame
Show what revision and author last modified each line of a file 
```
git blame --ignore-rev commit-hash
```
    
## [git-config](https://man7.org/linux/man-pages/man1/git-config.1.html)
``` console
$ git config blame.ignoreRevsFile .git-blame-ignore-revs
$ git config rebase.updateRefs true
$ git config --list
```



### global (~/.gitconfig)
``` sh
$ git config --global core.editor "vim"
$ git config --global user.email "manbing3@gmail.com"
$ git config --global user.name "Manbing"

$ git config --global gc.reflogExpire "7 days"
$ git config --global gc.reflogExpireUnreachable "7 days"
    
$ git config --global pack.windowMemory "100m"
$ git config --global pack.packSizeLimit "100m"
$ git config --global pack.threads "1"

$ git config --global log.decorate auto
$ git config --global init.defaultBranch master

# ~/.git-credential
$ git config --global credential.helper store
$ git config --global credential.helper 'store --file ~/.my-credentials'
$ git config --global credential.helper 'cache --timeout 3600'
```
    
### local
``` sh
$ git config --local gc.master.reflogExpire "14 days"
$ git config --local gc.master.reflogExpireUnreachable "14 days"

$ git config --local gc.develop.reflogExpire "never"
$ git config --local gc.develop.reflogExpireUnreachable "never"
```

### system (/etc/gitconfig)
``` sh
$ git config --system core.editor "vim"
```

## [git-cherry-pick](https://git-scm.com/docs/git-cherry-pick)
``` console
$ git cherry-pick -x
```

## git-clone
``` console
# local clone
$ git clone <local> <new local>  
$ git clone <URL> <rpository name>
```

## git-checkout
``` console
$ git checkout --theirs <filename>
$ git checkout --ours <filename>
$ git checkout <pathspec> <filename>
$ git checkout -b <branch> <remote>/<branch>
$ git checkout c5f567 -- file1/to/restore file2/to/restore
```

## git-commit
``` console
$ git commit --amend --date="$(date -R)"
$ git commit --signoff
$ git commit --amend --author="Author Name <email@address.com>"
```
## [git-describe](https://git-scm.com/docs/git-describe)
`$ git describe --contains <commit-ish>`
> The command finds the most recent tag that is reachable from a commit.

``` console
$ git describe --tags --exact-match HEAD
```

    
## git-format-patch
``` console
$ git format-patch <start SHA>..<end SHA> -o <dir>
$ git format-patch [-n | --numbered | -N | --no-numbered]
$ git format-patch --root
$ git format-patch --root <SHA> (where $SHA is that first commit)
```
``` console
$ git format-patch -3 -o <dir>
```
> Extract three topmost commits from the current branch and format them as e-mailable patches:
    
## git-gc
``` console
$ git gc
$ git gc --prune=now
```


## git-log
``` console
git log --all --graph --decorate --format=fuller
git log --name-only
git log -g

git log --follow
# Continue listing the history of a file beyond renames (works only for a single file).

    
git log <hash>
git log <hash>..
git log <old hash>..<new hash2>
git log <hash>...<hash2>
```

## git-reflog
```
git reflog show --all | grep <ref first 7 character>
```


## git-prune
```
git prune
```
  

## git-push
``` console
$ git push <remote> :<remote branch>
$ git push <remote> --delete <remote branch>
$ git push <remote> <local branch>:<remote branch>
```

## gitlab merge request
``` console
$ git push -o merge_request.create -o merge_request.target=<my-target-branch> <remote> <my-branch>
```


## git-patch
``` console
$ git apply --check <patch>
# Check if patch is ok or not

$ git am <*.patch>
# Apply a series of patches from a mailbox
```


## git-remote
``` console
$ git remote add <name> <URL>
$ git remote show <name>
$ git remote update --prune
$ git remote set-url <name> <url>
```

## git-rebase
```
$ git rebase --update-refs
```

## git-switch

    
## git-stash
``` console
$ git stash list
$ git stash pop <stash@{0}>
$ git stash drop <stash@{0}>
$ git stash clear
$ git stash save <name>
$ git stash show -p <stash@{0}>
```

## git-tagging
``` console
$ git tag
(Annotated)
$ git tag -a <tagname> -m "my version 1.4"
(Lightweight)
$ git tag -l
$ git tag <tagname>
$ git tag -a <tagname> 9fceb02
$ git show <tagname>
$ git tag -d <tagname>
$ git push <remote> <tagname>
$ git push <remote> --tags
$ git push <remote> :refs/tags/<tagname>
$ git push <remote> --delete <tagname>
```

## Aliases
``` console
$ git config --global alias.co checkout
$ git config --global alias.br branch
$ git config --global alias.ci commit
$ git config --global alias.st status
$ git config --global alias.visual '!gitk'
```

## git hooks
git commit --no-verify

## git send-email

## appendices
### Case: fix or improve a previous commit
```
# Create a fixup commit targeting a specific commit
$ git commit --fixup=<commit-hash>

# Later, during an interactive rebase, automatically squash fixup commits
$ git rebase -i --autosquash <base-branch>
```

### Case: patch commit to specific file
```
# Commit modification on GIT

# Generate patch files
$ git format-patch HEAD^ -o patchs/
    
# Patch to specific file
$ patch -i patchs/001-test.patch
```
    
## Reference
[Lecture 6: Version Control (git) (2020)](https://www.youtube.com/watch?v=2sjqTHE0zok&t=1329s)
# Git, Version Control 
###### tags: `Linux`

## relativing file/directory
``` console
$ repository/.git/
$ .gitignore
$ ~/.gitconfig
$ source /usr/share/bash-completion/completions/git
```


## basic
``` console
git init --bare
git fetch <remote>
git pull = git fetch + git merge
git add -patch <file>
git blame <file>
git branch -a
git stash
git checkout <file>
git diff <file>
git bisect
(in vim)
:Gblame
git add --patch <file>
git checkout --patch <file>
git remote add <name> <url>
git clang-format
git count-objects -v
git reflog
git reflog expire --expire-unreachable=now --all
git ls-files
```

## git-archive
``` console
$ git archive --format=tar --prefix="cindy/" <ref> > ../cindy.tar
```

## git-apply
``` console
$ git apply --stat a_file.patch
$ git apply --check a_file.patch

```

## [git-am](https://man7.org/linux/man-pages/man1/git-am.1.html)
> Apply a series of patches from a mailbox

git am <*.patch>

git am -3 <001.patch>
> When the patch does not apply cleanly, fall back on 3-way merge if the patch records the identity of blobs it is supposed to apply to and we have those blobs available locally.
    
## git-branch
```
git branch -a --contains <ref>
git branch <new branch> <exist branch>
```

## git-bisect
> Use binary search to find the commit that introduced a bug
    
## git-blame
Show what revision and author last modified each line of a file 
```
git blame --ignore-rev commit-hash
```
    
## [git-config](https://man7.org/linux/man-pages/man1/git-config.1.html)
``` console
$ git config blame.ignoreRevsFile .git-blame-ignore-revs
$ git config rebase.updateRefs true
$ git config --list
```



### global (~/.gitconfig)
``` sh
$ git config --global core.editor "vim"
$ git config --global user.email "manbing3@gmail.com"
$ git config --global user.name "Manbing"

$ git config --global gc.reflogExpire "7 days"
$ git config --global gc.reflogExpireUnreachable "7 days"
    
$ git config --global pack.windowMemory "100m"
$ git config --global pack.packSizeLimit "100m"
$ git config --global pack.threads "1"

$ git config --global log.decorate auto
$ git config --global init.defaultBranch master

# ~/.git-credential
$ git config --global credential.helper store
$ git config --global credential.helper 'store --file ~/.my-credentials'
$ git config --global credential.helper 'cache --timeout 3600'
```
    
### local
``` sh
$ git config --local gc.master.reflogExpire "14 days"
$ git config --local gc.master.reflogExpireUnreachable "14 days"

$ git config --local gc.develop.reflogExpire "never"
$ git config --local gc.develop.reflogExpireUnreachable "never"
```

### system (/etc/gitconfig)
``` sh
$ git config --system core.editor "vim"
```

## [git-cherry-pick](https://git-scm.com/docs/git-cherry-pick)
``` console
$ git cherry-pick -x
```

## git-clone
``` console
# local clone
$ git clone <local> <new local>  
$ git clone <URL> <rpository name>
```

## git-checkout
``` console
$ git checkout --theirs <filename>
$ git checkout --ours <filename>
$ git checkout <pathspec> <filename>
$ git checkout -b <branch> <remote>/<branch>
$ git checkout c5f567 -- file1/to/restore file2/to/restore
```

## git-commit
``` console
$ git commit --amend --date="$(date -R)"
$ git commit --signoff
$ git commit --amend --author="Author Name <email@address.com>"
```
## [git-describe](https://git-scm.com/docs/git-describe)
`$ git describe --contains <commit-ish>`
> The command finds the most recent tag that is reachable from a commit.

``` console
$ git describe --tags --exact-match HEAD
```

    
## git-format-patch
``` console
$ git format-patch <start SHA>..<end SHA> -o <dir>
$ git format-patch [-n | --numbered | -N | --no-numbered]
$ git format-patch --root
$ git format-patch --root <SHA> (where $SHA is that first commit)
```
``` console
$ git format-patch -3 -o <dir>
```
> Extract three topmost commits from the current branch and format them as e-mailable patches:
    
## git-gc
``` console
$ git gc
$ git gc --prune=now
```


## git-log
``` console
git log --all --graph --decorate --format=fuller
git log --name-only
git log -g

git log --follow
# Continue listing the history of a file beyond renames (works only for a single file).

    
git log <hash>
git log <hash>..
git log <old hash>..<new hash2>
git log <hash>...<hash2>
```

## git-reflog
```
git reflog show --all | grep <ref first 7 character>
```


## git-prune
```
git prune
```
  

## git-push
``` console
$ git push <remote> :<remote branch>
$ git push <remote> --delete <remote branch>
$ git push <remote> <local branch>:<remote branch>
```

## gitlab merge request
``` console
$ git push -o merge_request.create -o merge_request.target=<my-target-branch> <remote> <my-branch>
```


## git-patch
``` console
$ git apply --check <patch>
# Check if patch is ok or not

$ git am <*.patch>
# Apply a series of patches from a mailbox
```


## git-remote
``` console
$ git remote add <name> <URL>
$ git remote show <name>
$ git remote update --prune
$ git remote set-url <name> <url>
```

## git-rebase
```
$ git rebase --update-refs
```

## git-switch

    
## git-stash
``` console
$ git stash list
$ git stash pop <stash@{0}>
$ git stash drop <stash@{0}>
$ git stash clear
$ git stash save <name>
$ git stash show -p <stash@{0}>
```

## git-tagging
``` console
$ git tag
(Annotated)
$ git tag -a <tagname> -m "my version 1.4"
(Lightweight)
$ git tag -l
$ git tag <tagname>
$ git tag -a <tagname> 9fceb02
$ git show <tagname>
$ git tag -d <tagname>
$ git push <remote> <tagname>
$ git push <remote> --tags
$ git push <remote> :refs/tags/<tagname>
$ git push <remote> --delete <tagname>
```

## Aliases
``` console
$ git config --global alias.co checkout
$ git config --global alias.br branch
$ git config --global alias.ci commit
$ git config --global alias.st status
$ git config --global alias.visual '!gitk'
```

## git hooks
git commit --no-verify

## git send-email

## appendices
### Case: fix or improve a previous commit
```
# Create a fixup commit targeting a specific commit
$ git commit --fixup=<commit-hash>

# Later, during an interactive rebase, automatically squash fixup commits
$ git rebase -i --autosquash <base-branch>
```

### Case: patch commit to specific file
```
# Commit modification on GIT

# Generate patch files
$ git format-patch HEAD^ -o patchs/
    
# Patch to specific file
$ patch -i patchs/001-test.patch
```
    
## Reference
[Lecture 6: Version Control (git) (2020)](https://www.youtube.com/watch?v=2sjqTHE0zok&t=1329s)
