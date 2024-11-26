# CTF Writeup - **OverTheWire Bandit30 - Bandit31**

**Author**: spiketyson  

---

## Challenge Details

- **Challenge Name:** Bandit30 - Bandit31
- **Category:** Linux
- **Difficulty Level:** Easy
- **Date:** 26-11-2024

---

## Summary

This challenge is the level30 for the Bandit series on OverTheWire, focusing on basic Linux command-line skills. There is a git repository at ssh://bandit30-git@localhost/home/bandit30-git/repo via the port 2220. The password for the user bandit29-git is the same as for the user bandit30.
---

## Approach

Since this is an easy challenge, I planned to use basic Linux commands like `ls`, `git`, `cat` to explore the server and retrieve the flag.

---

## Solution Walkthrough

Connect to the server using the `ssh` command as below as:

```bash
ssh bandit30@bandit.labs.overthewire.org -p 2220 
# Password: fb5S2xb7bRyFmAvQYQGEqsbhVyJqhnDy
```

After loggin in, Lets create a temporary directory using `mktemp` and then git clone the directory using `git` as shown below.

```bash
mktemp -d
# /tmp/tmp.7U98gMxIAq
cd /tmp/tmp.7U98gMxIAq
git clone ssh://bandit30-git@localhost:2220/home/bandit30-git/repo
# Output:
# bandit30-git@localhost's password: 
# remote: Enumerating objects: 9, done.
# remote: Counting objects: 100% (9/9), done.
# remote: Compressing objects: 100% (6/6), done.
# remote: Total 9 (delta 2), reused 0 (delta 0), pack-reused 0
# Receiving objects: 100% (9/9), done.
# Resolving deltas: 100% (2/2), done.
```

Now that we have the repository. Lets explore it by using `ls` and `cat` to read the content of the files inside the directory.

```bash
cd repo
ls -la
# total 16
# drwxrwxr-x 3 bandit27 bandit27 4096 Nov 25 14:10 .
# drwx------ 3 bandit27 bandit27 4096 Nov 25 14:10 ..
# drwxrwxr-x 8 bandit27 bandit27 4096 Nov 25 14:10 .git
# -rw-rw-r-- 1 bandit27 bandit27   68 Nov 25 14:10 README.md
```

Here we can see that there is a file named `README.md`. Lets see the contents of this file using `cat` command.

```bash
cat README.md
# just an epmty file... muahaha
```

Here we can see that the the file is useless. We know that git is a version control system. We can see if there was any previous version existed using `git` as shown below.

```bash
git log --reflog
# commit acfc3c67816fc778c4aeb5893299451ca6d65a78 (HEAD -> master, origin/master, origin/HEAD)
# Author: Ben Dover <noone@overthewire.org>
# Date:   Thu Sep 19 07:08:44 2024 +0000

#     initial commit of README.md
```

Here we can see that we have totally 1 commits. That is also not useful for us. Then lets see the alternative method as there might be many branches in the git. lets explore the branches as shown below.

```bash
git branch -r
#   origin/HEAD 
#   origin/master
```

Lets try getting pull the code from the `master` branch and the explore it. 

```bash
git clone ssh://bandit30-git@localhost:2220/home/bandit30-git/repo -b master
# Output:
# bandit30-git@localhost's password: 
# remote: Enumerating objects: 9, done.
# remote: Counting objects: 100% (9/9), done.
# remote: Compressing objects: 100% (6/6), done.
# remote: Total 9 (delta 2), reused 0 (delta 0), pack-reused 0
# Receiving objects: 100% (9/9), done.
# Resolving deltas: 100% (2/2), done.
cd repo
```

Here we can find there are 1 file. The flag must be in README.md. Lets display the contents of it as shown below.

```bash
cat README.md
# just an epmty file... muahaha
```

Still no progress. We also know that there could be different version of the repository. Lets explore
the tags available in the repo and see the metadata of the tags as shown below. Suprisingly  we can find the password here.

```bash
git tag 
# secret
git show secret
# FLAG{fb5S2xb7bRyFmAvQYQGEqsbhVyJqhnDy}
```

---

## Flag

**Flag:**  fb5S2xb7bRyFmAvQYQGEqsbhVyJqhnDy

---

## Lessons Learned
It was a easy challenge, as I already had some basic knowledge about the `git` it helped me identify how to get the password from the file. The main learning here was getting familiar with `git` commands.