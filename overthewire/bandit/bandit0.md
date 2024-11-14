# CTF Writeup - **OverTheWire - Bandit0**

**Author**: spiketyson  
**Date**: 14-11-2024

---

## Challenge Details

- **Challenge Name:** [Challenge Name]
- **Category:** Linux
- **Difficulty Level:** Easy
- **Date:** 14-11-2024

---

## Summary

This challenge is the introductory level for the Bandit series on OverTheWire, focusing on basic Linux command-line skills. The goal is to connect to a server via SSH and read the file that has the flag.

---

## Approach

Since this is an introductory challenge, I planned to use basic Linux commands like `ssh`, `ls`, and `cat` to explore the server and retrieve the flag.

---

## Solution Walkthrough

### Step 1: Connect via SSH
Connecting to the server using the `ssh` command as below:

```bash
ssh bandit0@bandit.labs.overthewire.org -p 2220
# Password: bandit0
```

After logging in, I used the `ls` command to list the files in the directory and found a file named `readme`.

```bash
ls
# Output: readme
```

Using the `cat` command, I viewed the contents of `readme` to extract the flag.

```bash
cat readme
# Output: FLAG{ZjLjTmM6FvvyRnrb2rfNWOZOTa6ip5If}
```

---

## Flag

**Flag:** ZjLjTmM6FvvyRnrb2rfNWOZOTa6ip5If

---

## Lessons Learned

It was a good refresher for using `ssh` and understanding directory structures on remote servers.
