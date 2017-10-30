title: Git Summary
date: 2017-09-03 21:24:57
tags: [linux, notes, git]
categories: summary
description: Git summary。
---

# Git summary

## git reset --hard
未commit的chagne会随着branch的切换而移动，因此，当在某一个branch中`git reset --hard`的时候， changes就会消失。

## git 删除remote branch
`git push origin --delete branch_name`

## git rm files in repo
`git rm --cache files`

## show git track files
`git ls-files` 

## git diff one file in two different commit ids:
`git diff HEAD(^^)(two commits before current) commit_id XX.java`
`git diff HEAD^^ HEAD main.c`

## Git RSA key fingerprint

The newer SSH commands will list fingerprints as a SHA256 Key.

For example

`ssh-keygen -lf ~/.ssh/id_rsa.pub `
`1024 SHA256:19n6fkdz0qqmowiBy6XEaA87EuG/jgWUr44ZSBhJl6Y (DSA)`

If you need to compare it against a old fingerprint you also need to specify to use the md5 fingerprint hashing function.

`ssh-keygen -E md5 -lf ~/.ssh/id_rsa.pub`
`2048 MD5:4d:5b:97:19:8c:fe:06:f0:29:e7:f5:96:77:cb:3c:71 (DSA)`

## Git submodule:

### Git submodule delete:
1. Delete the relevant section from the .gitmodules file.
2. Stage the .gitmodules changes git add .gitmodules
3. Delete the relevant section from .git/config.
4. Run git rm --cached path_to_submodule (no trailing slash).
5. Run rm -rf .git/modules/path_to_submodule
6. Commit git commit -m "Removed submodule <name>"
7. Delete the now untracked submodule files
8. rm -rf path_to_submodule


--- 