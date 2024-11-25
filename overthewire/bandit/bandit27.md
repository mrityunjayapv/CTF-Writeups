# CTF Writeup - **OverTheWire Bandit27 - Bandit28**

**Author**: spiketyson  

---

## Challenge Details

- **Challenge Name:** Bandit27 - Bandit28
- **Category:** Linux
- **Difficulty Level:** Easy
- **Date:** 25-11-2024

---

## Summary

This challenge is the level26 for the Bandit series on OverTheWire, focusing on basic Linux command-line skills. There is a git repository at ssh://bandit27-git@localhost/home/bandit27-git/repo via the port 2220. The password for the user bandit27-git is the same as for the user bandit27. Clone the repository and find the password for the next level.
---

## Approach

Since this is an easy challenge, I planned to use basic Linux commands like `ls`, `git`, `cat` to explore the server and retrieve the flag.

---

## Solution Walkthrough

Connect to the server using the `ssh` command as below as:

```bash
ssh bandit27@bandit.labs.overthewire.org -p 2220 -t sh
# Password: s0773xxkk0MXfdqOfPRVr9L3jJBUOgCZ
```

After loggin in, Lets create a temporary directory using `mktemp` and then git clone the directory using `git` as shown below.

```bash
mktemp -d
# /tmp/tmp.Dlwlug5odv
cd /tmp/tmp.Dlwlug5odv
git clone ssh://bandit27-git@localhost:2220/home/bandit27-git/repo
# Output:
# bandit27-git@localhost's password: 
# remote: Enumerating objects: 3, done.
# remote: Counting objects: 100% (3/3), done.
# remote: Compressing objects: 100% (2/2), done.
# remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
# Receiving objects: 100% (3/3), done.
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

Here we can see that there is a file named `README`. Lets see the contents of this file. It will give us the password for the next level

```bash
cat README
# FLAG{Yz9IpL0sBcCeuG7m9uQFt8ZNpS4HZRcN}
```

---

## Flag

**Flag:**  Yz9IpL0sBcCeuG7m9uQFt8ZNpS4HZRcN

---

## Lessons Learned
It was a good challenge, I was stuck in the begining of the challenge as to how we need to connect to the server. But once i understood that we need to exploit the bug in previous challenge to set the shell to /bin/bash and then use `:shell` to open the shell so that we can access the server, it became straightforward after that. 