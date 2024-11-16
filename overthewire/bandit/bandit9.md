# CTF Writeup - **OverTheWire Bandit9 - Bandit10**

**Author**: spiketyson  

---

## Challenge Details

- **Challenge Name:** Bandit9 - Bandit10
- **Category:** Linux
- **Difficulty Level:** Easy
- **Date:** 16-11-2024

---

## Summary

This challenge is the level9 for the Bandit series on OverTheWire, focusing on basic Linux command-line skills. The goal is to access file data.txt in one of the few human-readable strings, preceded by several ‘=’ characters.

---

## Approach

Since this is an basic challenge, I planned to use basic Linux commands like `ssh`, `ls`,`grep`, and `cat` to explore the server and retrieve the flag.

---

## Solution Walkthrough

Connecting to the server using the `ssh` command as below:

```bash
ssh bandit9@bandit.labs.overthewire.org -p 2220
# Password: 4CKMh1JI91bUIZZPXDqGanal4xvAg0JM
```

After logging in, I used the `ls` command to list the files in the directory and found a file named `data.txt`.

```bash
ls -la
# Output: data.txt
```

Now that I have found the file with the flag. We can use `cat` command to display the contents of the file. 

```bash
cat data.txt
# Output: k�7iL�����62h�O�O�����錶5� ...
```

There is a lot of data in the file it not in human readable format. But we have the clue that the flag is next to `=` symbol and it human readable. we can use the `grep` command to get the === symbols location as shown below. We also need to use `strings` command to get all the human readable content from the file as it is a binary file with mixed data. 

```bash
cat data.txt | strings | grep ===
# Output: 
# }========== the
# 3JprD========== passwordi
# ~fDV3========== is
# D9========== Flag{FGUW5ilLVJrxX9kMYMmlN4MgbpfMiqey}
```
---

## Flag

**Flag:** FGUW5ilLVJrxX9kMYMmlN4MgbpfMiqey

---

## Lessons Learned

It was a easier challange, the idea is to find the `===` symbol in the binary file and to learn how to use `strings` command.