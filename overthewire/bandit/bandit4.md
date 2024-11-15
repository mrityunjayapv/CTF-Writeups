# CTF Writeup - **OverTheWire Bandit4 - Bandit5**

**Author**: spiketyson  

---

## Challenge Details

- **Challenge Name:** Bandit4 - Bandit5
- **Category:** Linux
- **Difficulty Level:** Easy
- **Date:** 15-11-2024

---

## Summary

This challenge is the level4 for the Bandit series on OverTheWire, focusing on basic Linux command-line skills. The goal is to connect to a server via SSH and read the file that flag.

---

## Approach

Since this is an basic challenge, I planned to use basic Linux commands like `ssh`, `ls`,`find`, and `cat` to explore the server and retrieve the flag.

---

## Solution Walkthrough

Connecting to the server using the `ssh` command as below:

```bash
ssh bandit4@bandit.labs.overthewire.org -p 2220
# Password: 2WmrDFRmJIq3IPxneAaMGhap0pFhF3NJ
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
# Output: -file00  -file01  -file02  -file03  -file04  -file05  -file06  -file07  -file08  -file09
```
Now I found the files that has the flag. using `cat` command we can read the contents of the file as shown below.

```bash
cat ./-file00
# Output: �p��&�y�,�(jo�.at�:uf�^���
```
As we have 10 files. we cannot go through each file and read it, as it is time consuming process if we have 100 files. Instead we know that the flag is in Human readable format (ASCII) and it is a non executable file. with this we can use the `find` command as shown below. 

```bash
find . -type f ! -executable -exec file {} + | grep ASCII
# Output: ./-file07: ASCII text
cat ./-file07
# Output: FLAG{4oQYVPkxZOOEOO5pTW81FB8j8lxXGUQw}
```
---

## Flag

**Flag:** 4oQYVPkxZOOEOO5pTW81FB8j8lxXGUQw

---

## Lessons Learned

It was a tricky challange, the idea to find the files using `find` command and the the logic of arriving that it is non executable file and the files has ascii characters was based on the clue given in the challange.