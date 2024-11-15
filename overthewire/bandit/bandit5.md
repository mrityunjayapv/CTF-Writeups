# CTF Writeup - **OverTheWire Bandit5 - Bandit6**

**Author**: spiketyson  

---

## Challenge Details

- **Challenge Name:** Bandit5 - Bandit6
- **Category:** Linux
- **Difficulty Level:** Easy
- **Date:** 15-11-2024

---

## Summary

This challenge is the level5 for the Bandit series on OverTheWire, focusing on basic Linux command-line skills. The goal is to connect to a server via SSH and read the file that flag.

---

## Approach

Since this is an basic challenge, I planned to use basic Linux commands like `ssh`, `ls`,`find`, and `cat` to explore the server and retrieve the flag.

---

## Solution Walkthrough

Connecting to the server using the `ssh` command as below:

```bash
ssh bandit5@bandit.labs.overthewire.org -p 2220
# Password: 4oQYVPkxZOOEOO5pTW81FB8j8lxXGUQw
```

After logging in, I used the `ls` command to list the files in the directory and found a directory named `inhere`.

```bash
ls -la
# Output: inhere
```

Using the `cd` command, I went inside the of `inhere` directory to explore. Then listed all the files/directories using `ls`.

```bash
cd inhere
ls
# Output: maybehere00  maybehere03  maybehere06  maybehere09  maybehere12  maybehere15  maybehere18 maybehere01  maybehere04  maybehere07  maybehere10  maybehere13  maybehere16  maybehere19 maybehere02  maybehere05  maybehere08  maybehere11  maybehere14  maybehere17

```
Now I found the directories that has the flag. As we have lot of files and directories. we cannot go through each directory/files and read it, as it is time consuming process. Instead we know that the flag is in Human readable format (ASCII) and it is a non executable file and it size is 1033 bytes. with this we can use the `find` command as shown below. 


```bash
find . -type f -size 1033c ! -executable -exec file {} + | grep ASCII
# Output: ./maybehere07/.file2: ASCII text, with very long lines (1000)
cat ./maybehere07/.file2
# Output: FLAG{HWasnPhtq9AVKe0dmk45nxy20cvUa6EG}
```
---

## Flag

**Flag:** HWasnPhtq9AVKe0dmk45nxy20cvUa6EG

---

## Lessons Learned

It was a simple challange, the idea to find the files using `find` command and then combining it the the files that has weird names. 