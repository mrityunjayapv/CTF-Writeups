# CTF Writeup - **OverTheWire Bandit2 - Bandit3**

**Author**: spiketyson  

---

## Challenge Details

- **Challenge Name:** Bandit2 - Bandit3
- **Category:** Linux
- **Difficulty Level:** Easy
- **Date:** 14-11-2024

---

## Summary

This challenge is the level2 for the Bandit series on OverTheWire, focusing on basic Linux command-line skills. The goal is to connect to a server via SSH and read the file that has the flag.

---

## Approach

Since this is an introductory challenge, I planned to use basic Linux commands like `ssh`, `ls`, and `cat` to explore the server and retrieve the flag.

---

## Solution Walkthrough

Connecting to the server using the `ssh` command as below:

```bash
ssh bandit2@bandit.labs.overthewire.org -p 2220
# Password: 263JGJPfgU6LtdEvgfWU1XP5yac29mFx
```

After logging in, I used the `ls` command to list the files in the directory and found a file named `spaces in this filename`.

```bash
ls -ltr
# Output: spaces in this filename
```

Using the `cat` command, I viewed the contents of `spaces in this filename` file to extract the flag.

```bash
cat ./spaces\ in\ this\ filename
# Output: FLAG{MNk8KNH3Usiio41PRUEoDFPqfxLPlSmx}
```

---

## Flag

**Flag:** MNk8KNH3Usiio41PRUEoDFPqfxLPlSmx

---

## Lessons Learned

It was a simple challenge as seen previously as the file name had spaces in between, using logic of accessing the file is similar to executing a program made it easier