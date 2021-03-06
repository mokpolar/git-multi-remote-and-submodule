# Git: Multi-remote and submodule settings


## Multi-remote Config.: 1 local: N remote management


### Clone a repo

```bash
git clone https://github.com/pydemia/git-multi-remote-and-submodule
cd git-multi-remote-and-submodule
```

### Check the current remote setting

```bash
$ git remote -v
origin  https://github.com/pydemia/git-multi-remote-and-submodule (fetch)
origin  https://github.com/pydemia/git-multi-remote-and-submodule (push)
```

### Add new remotes as alias

```bash
# Alias: github & gitlab
git remote add github https://github.com/pydemia/git-multi-remote-and-submodule
git remote add gitlab https://gitlab.com/pydemia/git-multi-remote-and-submodule

# Set an alias `all`: your main remote: github
git remote add all https://github.com/pydemia/git-multi-remote-and-submodule
```

### Add push URLs: github & gitlab

```bash
git remote set-url --add --push all https://github.com/pydemia/git-multi-remote-and-submodule
git remote set-url --add --push all https://gitlab.com/pydemia/git-multi-remote-and-submodule
```

### Check your setting

```console
$ git remote show all
* remote all
  Fetch URL: https://github.com/pydemia/git-multi-remote-and-submodule  # <- Your original remote
  Push  URL: https://github.com/pydemia/git-multi-remote-and-submodule  # <- Your original remote
  Push  URL: https://gitlab.com/pydemia/git-multi-remote-and-submodule  # <- An alternative remote
  HEAD branch: main
  Remote branch:
    main new (next fetch will store in remotes/all)
  Local ref configured for 'git push':
    main pushes to main (up to date)
```

It's done!

### Usage

Show your remote branches:

```bash
git branch -r
```


`git pull` can be done seperately, one at a time:

```bash
git pull gitlab master # --allow-unrelated-histories
git pull github main
```

`git fetch` can be done at once:

```bash
git fetch --all
```

`git push` can be done at once:

```bash
git push all

# with submodules: recursive push
git push all --recurse-submodules=on-demand
```

---

## Git Submodule Setting

### Start from scratch

```bash
# Get inside the repo
cd git-multi-remote-and-submodule

# Clean start if exists
git rm -r --cached .
rm -f .gitmodules
```

### Add submodules

Run:

```bash
git submodule add https://github.com/pydemia/springboot-demo springboot
git submodule add -b main https://github.com/pydemia/ldap ldap
```

The output would be:

```console
$ git submodule add https://github.com/pydemia/springboot-demo springboot
Cloning into '/home/pydemia/git/git-multi-remote-and-submodule/springboot'...
remote: Enumerating objects: 10, done.
remote: Counting objects: 100% (10/10), done.
remote: Compressing objects: 100% (6/6), done.
remote: Total 10 (delta 0), reused 7 (delta 0), pack-reused 0
Unpacking objects: 100% (10/10), 1.16 KiB | 197.00 KiB/s, done.
warning: LF will be replaced by CRLF in .gitmodules.
The file will have its original line endings in your working directory

$ git submodule add -b main https://github.com/pydemia/ldap ldap
Cloning into '/home/pydemia/git/git-multi-remote-and-submodule/ldap'...
remote: Enumerating objects: 8, done.
remote: Counting objects: 100% (8/8), done.
remote: Compressing objects: 100% (6/6), done.
remote: Total 8 (delta 0), reused 5 (delta 0), pack-reused 0
Unpacking objects: 100% (8/8), 5.19 KiB | 885.00 KiB/s, done.
warning: LF will be replaced by CRLF in .gitmodules.
The file will have its original line endings in your working directory
```

### Check via `ls`

Check if submodules are copied:

```bash
$ ls
README.md  ldap  springboot
```

`.gitmodules`:

```console
$ cat .gitmodules
[submodule "springboot"]
        path = springboot
        url = https://github.com/pydemia/springboot-demo
[submodule "ldap"]
        path = ldap
        url = https://github.com/pydemia/ldap
        branch = main
```

### Initiate submodules

```bash
git submodule update --init --recursive
```

### Update submodules

```bash
git submodule update --recursive --remote
```

or

```bash
git pull --recurse-submodules
```

Done.

---

## Extra: push to `all`:

Commit changes:

```console
# Start commit
$ git add .
warning: LF will be replaced by CRLF in README.md.
The file will have its original line endings in your working directory

$ git commit -m 'doubling remotes'
[main 9a0976f] doubling remotes
 5 files changed, 185 insertions(+), 1 deletion(-)
 create mode 100644 .gitignore
 create mode 100644 .gitmodules
 rewrite README.md (100%)
 create mode 160000 ldap
 create mode 160000 springboot
```

Then push it:

```
$ git push all
Username for 'https://github.com': pydemia
Password for 'https://pydemia@github.com':
Enumerating objects: 7, done.
Counting objects: 100% (7/7), done.
Delta compression using up to 8 threads
Compressing objects: 100% (4/4), done.
Writing objects: 100% (5/5), 1.62 KiB | 1.62 MiB/s, done.
Total 5 (delta 0), reused 0 (delta 0)
To https://github.com/pydemia/git-multi-remote-and-submodule
   72d7d74..9a0976f  main -> main
Username for 'https://gitlab.com': pydemia
Password for 'https://pydemia@gitlab.com':
warning: redirecting to https://gitlab.com/pydemia/git-multi-remote-and-submodule.git/
Enumerating objects: 8, done.
Counting objects: 100% (8/8), done.
Delta compression using up to 8 threads
Compressing objects: 100% (5/5), done.
Writing objects: 100% (8/8), 2.21 KiB | 753.00 KiB/s, done.
Total 8 (delta 0), reused 0 (delta 0)
To https://gitlab.com/pydemia/git-multi-remote-and-submodule
 * [new branch]      main -> main
```

Done.