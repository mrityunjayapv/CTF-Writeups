# CTF Writeup - **OverTheWire Bandit31 - Bandit32**

**Author**: spiketyson  

---

## Challenge Details

- **Challenge Name:** Bandit31 - Bandit32
- **Category:** Linux
- **Difficulty Level:** Easy
- **Date:** 26-11-2024

---

## Summary

This challenge is the level31 for the Bandit series on OverTheWire, focusing on basic Linux command-line skills. There is a git repository at ssh://bandit31-git@localhost/home/bandit31-git/repo via the port 2220. The password for the user bandit31-git is the same as for the user bandit31.
---

## Approach

Since this is an easy challenge, I planned to use basic Linux commands like `ls`, `git`, `cat` to explore the server and retrieve the flag.

---

## Solution Walkthrough

Connect to the server using the `ssh` command as below as:

```bash
ssh bandit31@bandit.labs.overthewire.org -p 2220 
# Password: fb5S2xb7bRyFmAvQYQGEqsbhVyJqhnDy
```

After loggin in, Lets create a temporary directory using `mktemp` and then git clone the directory using `git` as shown below.

```bash
mktemp -d
# /tmp/tmp.MIKdVZRdXH
cd /tmp/tmp.MIKdVZRdXH
git clone ssh://bandit31-git@localhost:2220/home/bandit31-git/repo
# Output:
# bandit31-git@localhost's password: 
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
# This time your task is to push a file to the remote repository.

# Details:
#     File name: key.txt
#     Content: 'May I come in?'
#     Branch: master
```

Here we get the clue that we need to push a file to the remote repository, where the file should be a text file and it should have `key.txt` as it name and it should contain `May I come in?` in it. We can create a file using `nano` as shown below

```bash
nano key.txt
# May I come in?
# Ctrl + x, Press y, Press Enter. This will save the file.
```

Now lets try changing the repository to staging aread as we now have a new change in the repository. we can do it using `git` as shown below. 

```bash
git add key.txt
# The following paths are ignored by one of your .gitignore files:
# key.txt
# hint: Use -f if you really want to add them.
# hint: Turn this message off by running
# hint: "git config advice.addIgnoredFile false"
git status
# On branch master
# Your branch is up to date with 'origin/master'.
# nothing to commit, working tree clean
```

Oops we are seeing anyting. Lets understand what these functions do, `git add` adds `key.txt` that has been modified in the current repository to the staging area. `git status` shows what are those changes which are tracked and which are not. In our case we are are not able to see the any changes. But we got a clue from the output of git add to use `-f` to forcifully add the file to staging area. Let try to do that as shown below. 

```bash
git add -f key.txt
git commit -m "sample"
# [master 6620c9a] sample
#  1 file changed, 1 insertion(+), 1 deletion(-)
git push
# bandit31-git@localhost's password:
# Enumerating objects: 7, done.
# Counting objects: 100% (7/7), done.
# Delta compression using up to 2 threads
# Compressing objects: 100% (4/4), done.
# Writing objects: 100% (6/6), 534 bytes | 534.00 KiB/s, done.
# Total 6 (delta 1), reused 0 (delta 0), pack-reused 0
# remote: ### Attempting to validate files... ####
# remote:
# remote: .oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.
# remote:
# remote: Well done! Here is the password for the next level:
# remote: 3O9RfhqyAlVBEZpVb6LYStshZoqoSx5K
# remote:
# remote: .oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.
```

---

## Flag

**Flag:**  3O9RfhqyAlVBEZpVb6LYStshZoqoSx5K

---

## Lessons Learned
It was a easy challenge, as I already had some basic knowledge about the `git` it helped me identify how to get the password from the file. The main learning here was getting familiar with `git` commands.