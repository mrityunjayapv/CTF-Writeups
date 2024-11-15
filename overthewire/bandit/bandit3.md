# CTF Writeup - **OverTheWire Bandit3 - Bandit4**

**Author**: spiketyson  
**Date**: 14-11-2024

---

## Challenge Details

- **Challenge Name:** Bandit3 - Bandit4
- **Category:** Linux
- **Difficulty Level:** Easy
- **Date:** 15-11-2024

---

## Summary

This challenge is the level3 for the Bandit series on OverTheWire, focusing on basic Linux command-line skills. The goal is to connect to a server via SSH and read the file that is stored in a hidden directory.

---

## Approach

Since this is an basic challenge, I planned to use basic Linux commands like `ssh`, `ls`, and `cat` to explore the server and retrieve the flag.

---

## Solution Walkthrough

Connecting to the server using the `ssh` command as below:

```bash
ssh bandit3@bandit.labs.overthewire.org -p 2220
# Password: MNk8KNH3Usiio41PRUEoDFPqfxLPlSmx
```

After logging in, I used the `ls` command to list the files in the directory and found a directory named `inhere`.

```bash
ls -ltr
# Output: inhere
```

Using the `cd` command, I went inside the of `inhere` directory to explore. Then listed all the files/directories using `ls` but there was nothing in this directory. But the clue said the file is hidden then i used `ls -la`

```bash
cd inhere
la -la
# Output: ...Hiding-From-You
```
Now I found the file that has the flag. using `cat` command we can read the contents of the file as shown below.

```bash
cat ...Hiding-From-You
# Output: FLAG{2WmrDFRmJIq3IPxneAaMGhap0pFhF3NJ}
```

---

## Flag

**Flag:** 2WmrDFRmJIq3IPxneAaMGhap0pFhF3NJ

---

## Lessons Learned

It was a simple challenge as the trick was to find the file that is hidden. good learning was  ls -la should be used to find the hidden files instead of using ls -ltr for list files.