# CTF Writeup - **OverTheWire Bandit1 - Bandit2**

**Author**: spiketyson  
**Date**: 14-11-2024

---

## Challenge Details

- **Challenge Name:** Bandit1 - Bandit2
- **Category:** Linux
- **Difficulty Level:** Easy
- **Date:** 14-11-2024

---

## Summary

This challenge is the level1 for the Bandit series on OverTheWire, focusing on basic Linux command-line skills. The goal is to connect to a server via SSH and read the file that has the flag.

---

## Approach

Since this is an introductory challenge, I planned to use basic Linux commands like `ssh`, `ls`, and `cat` to explore the server and retrieve the flag.

---

## Solution Walkthrough

Connecting to the server using the `ssh` command as below:

```bash
ssh bandit1@bandit.labs.overthewire.org -p 2220
# Password: ZjLjTmM6FvvyRnrb2rfNWOZOTa6ip5If
```

After logging in, I used the `ls` command to list the files in the directory and found a file named `-`.

```bash
ls -ltr
# Output: -
```

Using the `cat` command, I viewed the contents of `-` file to extract the flag.

```bash
cat ./-
# Output: FLAG{263JGJPfgU6LtdEvgfWU1XP5yac29mFx}
```

---

## Flag

**Flag:** 263JGJPfgU6LtdEvgfWU1XP5yac29mFx

---

## Lessons Learned

It was a tricky challenge as the file name was a special character. But the logic of accessing the file is similar to executing a program.