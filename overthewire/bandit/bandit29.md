# CTF Writeup - **OverTheWire Bandit29 - Bandit30**

**Author**: spiketyson  

---

## Challenge Details

- **Challenge Name:** Bandit29 - Bandit30
- **Category:** Linux
- **Difficulty Level:** Easy
- **Date:** 26-11-2024

---

## Summary

This challenge is the level29 for the Bandit series on OverTheWire, focusing on basic Linux command-line skills. There is a git repository at ssh://bandit29-git@localhost/home/bandit29-git/repo via the port 2220. The password for the user bandit29-git is the same as for the user bandit29.
---

## Approach

Since this is an easy challenge, I planned to use basic Linux commands like `ls`, `git`, `cat` to explore the server and retrieve the flag.

---

## Solution Walkthrough

Connect to the server using the `ssh` command as below as:

```bash
ssh bandit29@bandit.labs.overthewire.org -p 2220 
# Password: 4pT1t5DENaYuqnqvadYs1oE4QLCdjmJ7
```

After loggin in, Lets create a temporary directory using `mktemp` and then git clone the directory using `git` as shown below.

```bash
mktemp -d
# /tmp/tmp.ij52O9p7gv
cd /tmp/tmp.ij52O9p7gv
git clone ssh://bandit29-git@localhost:2220/home/bandit29-git/repo
# Output:
# bandit29-git@localhost's password: 
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
cat README
# # Bandit Notes
# Some notes for bandit30 of bandit.
# ## credentials
# - username: bandit30
# - password: <no passwords in production!>
```

Here we can see that the the file has the credentials of bandit30 `<no passwords in production!>` but the password is not useful for us. We know that git is a version control system. We can see if there was any previous version existed using `git` as shown below.

```bash
git log --reflog
# commit 6ac7796430c0f39290a0e29a4d32e5126544b022 (HEAD -> master, origin/master, origin/HEAD)
# Author: Ben Dover <noone@overthewire.org>
# Date:   Thu Sep 19 07:08:41 2024 +0000
#     fix username

# commit e65a928cca4db1863b478cf5e93d1d5b1c1bd6b2
# Author: Ben Dover <noone@overthewire.org>
# Date:   Thu Sep 19 07:08:41 2024 +0000
#     initial commit of README.md
```
Here we can see that we have totally 2 commits. We are at the latest commit let go back to the previous commit and see the contents of the file as shown below.

```bash
git reset --hard 3621de89d8eac9d3b64302bfb2dc67e9a566decd
# HEAD is now at 3621de8 add missing data
ls -la 
README.md
cat README.md
# # Bandit Notes
# Some notes for bandit30 of bandit.
# ## credentials
# - username: bandit30
# - password: <no passwords in production!>
```

Even after moving to previous commit, we are still not able to see the passwrod. Then lets see the alternative method as there might be many branches in the git. lets explore the branches as shown below.

```bash
git branch -r
#   origin/HEAD -> origin/master
#   origin/dev
#   origin/master
#   origin/sploits-dev
```

Lets try getting pull the code from the `dev` branch and the explore it. 

```bash
git clone ssh://bandit29-git@localhost:2220/home/bandit29-git/repo -b dev
# Output:
# bandit29-git@localhost's password: 
# remote: Enumerating objects: 9, done.
# remote: Counting objects: 100% (9/9), done.
# remote: Compressing objects: 100% (6/6), done.
# remote: Total 9 (delta 2), reused 0 (delta 0), pack-reused 0
# Receiving objects: 100% (9/9), done.
# Resolving deltas: 100% (2/2), done.
cd repo
```

Here we can find there are 2 files. The password for the flag is in README.md as it had the credentials. Lets display the contents of it as shown below.

```bash
ls -la 
# README.md 
# gif2ascii.py
cat README.md
# # Bandit Notes
# Some notes for bandit30 of bandit.
# ## credentials
# - username: bandit30
# - password: qp30ex3VLz5MDG1n91YowTv4Q8l7CDZL
```

---

## Flag

**Flag:**  qp30ex3VLz5MDG1n91YowTv4Q8l7CDZL

---

## Lessons Learned
It was a easy challenge, as I already had some basic knowledge about the `git` it helped me identify how to get the password from the file. The main learning here was getting familiar with `git` commands.