# CTF Writeup - **OverTheWire Bandit28 - Bandit29**

**Author**: spiketyson  

---

## Challenge Details

- **Challenge Name:** Bandit28 - Bandit29
- **Category:** Linux
- **Difficulty Level:** Easy
- **Date:** 25-11-2024

---

## Summary

This challenge is the level28 for the Bandit series on OverTheWire, focusing on basic Linux command-line skills. There is a git repository at ssh://bandit28-git@localhost/home/bandit28-git/repo via the port 2220. The password for the user bandit28-git is the same as for the user bandit28.
---

## Approach

Since this is an easy challenge, I planned to use basic Linux commands like `ls`, `git`, `cat` to explore the server and retrieve the flag.

---

## Solution Walkthrough

Connect to the server using the `ssh` command as below as:

```bash
ssh bandit28@bandit.labs.overthewire.org -p 2220 -t sh
# Password: Yz9IpL0sBcCeuG7m9uQFt8ZNpS4HZRcN
```

After loggin in, Lets create a temporary directory using `mktemp` and then git clone the directory using `git` as shown below.

```bash
mktemp -d
# /tmp/tmp.JDGgRG6jO9
cd /tmp/tmp.JDGgRG6jO9
git clone ssh://bandit28-git@localhost:2220/home/bandit28-git/repo
# Output:
# bandit28-git@localhost's password: 
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
# -rw-rw-r-- 1 bandit27 bandit27   68 Nov 25 14:10 README
```

Here we can see that there is a file named `README`. Lets see the contents of this file using `cat` command.

```bash
cat README
# # Bandit Notes
# Some notes for level29 of bandit.
# ## credentials
# - username: bandit29
# - password: xxxxxxxxxx
```

Here we can see that the the file has the credentials of bandit29 `xxxxxxx` but the password is not useful for us. We know that git is a version control system. We can see if there was any previous version existed using `git` as shown below.

```bash
git log --reflog
# commit 817e303aa6c2b207ea043c7bba1bb7575dc4ea73 (HEAD -> master, origin/master, origin/HEAD)
# Author: Morla Porla <morla@overthewire.org>
# Date:   Thu Sep 19 07:08:39 2024 +0000
#     fix info leak

# commit 3621de89d8eac9d3b64302bfb2dc67e9a566decd
# Author: Morla Porla <morla@overthewire.org>
# Date:   Thu Sep 19 07:08:39 2024 +0000
#     add missing data

# commit 0622b73250502618babac3d174724bb303c32182
# Author: Ben Dover <noone@overthewire.org>
# Date:   Thu Sep 19 07:08:39 2024 +0000
#     initial commit of README.md

```
Here we can see that we have totally 3 commits. We are at the latest commit let go back to the previous commit and see the contents of the file as shown below.

```bash
git reset --hard 3621de89d8eac9d3b64302bfb2dc67e9a566decd
# HEAD is now at 3621de8 add missing data
ls -la 
README.md
cat README.md
# # Bandit Notes
# Some notes for level29 of bandit.
# ## credentials
# - username: bandit29
# - password: 4pT1t5DENaYuqnqvadYs1oE4QLCdjmJ7
```

---

## Flag

**Flag:**  4pT1t5DENaYuqnqvadYs1oE4QLCdjmJ7

---

## Lessons Learned
It was a easy challenge, as I already had some basic knowledge about the `git` it helped me identify how to get the password from the file. The main learning here was getting familiar with `git` commands.