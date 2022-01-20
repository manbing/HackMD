# Git, Version Control 
###### tags: `Linux`


## basic
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

## git-gc
git gc
    
## git-prune
git prune

## git-config
git config --global core.editor "vim"
git config = ~/.gitconfig
git config --global user.email "manbing3@gmail.com"
git config --global user.name "Manbing"
    
## git-log
git log --all --graph --decorate --format=fuller
git log --name-only

    
git log --follow
>Continue listing the history of a file beyond renames (works only for a single file).


## git-reflog
git reflog show --all | grep <ref first 7 character>

## git-branch
git branch -a --contains <ref>

## git-remote
git remote add <name> <URL>
git remote show <name>
git remote update --prune
git remote set-url <name> <url>

## git-checkout
git checkout --theirs <filename>
git checkout --ours <filename>
git checkout <pathspec> <filename>
git checkout -b <name> <remote>/<name>
git checkout c5f567 -- file1/to/restore file2/to/restore

## git-commit
git commit --amend --date="$(date -R)"
git commit --signoff

## git-push
git push <remote> :<remote branch>
git push <remote> --delete <remote branch>
git push <remote> <local branch>:<remote branch>

## git-format-patch
git format-patch <start SHA>..<end SHA> -o <dir>
git format-patch [-n | --numbered | -N | --no-numbered]
git format-patch --root
git format-patch --root <SHA> (where $SHA is that first commit)


## git-ptch
git apply --check <patch> - Check if patch is ok or not
git am <*.patch> - Apply a series of patches from a mailbox

## git-tagging
git tag
(Annotated)
git tag -a <tagname> -m "my version 1.4"
(Lightweight)
git tag -l
git tag <tagname>
git tag -a <tagname> 9fceb02
git show <tagname>
git tag -d <tagname>
git push <remote> <tagname>
git push <remote> --tags
git push <remote> :refs/tags/<tagname>
git push <remote> --delete <tagname>

## Aliases
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.st status
git config --global alias.visual '!gitk'

## Dotfiles
.gitignore
~/.gitconfig


## git hooks
git commit --no-verify


## Else
git count-objects -v
git reflog
git reflog expire --expire-unreachable=now --all
git gc --prune=now


## Reference
[Lecture 6: Version Control (git) (2020)](https://www.youtube.com/watch?v=2sjqTHE0zok&t=1329s)
