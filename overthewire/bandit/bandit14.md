# CTF Writeup - **OverTheWire Bandit14 - Bandit15**

**Author**: spiketyson  

---

## Challenge Details

- **Challenge Name:** Bandit14 - Bandit15
- **Category:** Linux
- **Difficulty Level:** Easy
- **Date:** 18-11-2024

---

## Summary

This challenge is the level13 for the Bandit series on OverTheWire, focusing on basic Linux command-line skills. The password for the next level can be retrieved by submitting the password of the current level to port 30000 on localhost.
---

## Approach

Since this is an basic challenge, I planned to use basic Linux commands like `ssh`, `ls`, and `cat` to explore the server and retrieve the flag.

---

## Solution Walkthrough

Connecting to the server using the `ssh` command as below:

```bash
ssh bandit14@bandit.labs.overthewire.org -p 2220
# Password: MU4VWeTyJk8ROof1qqmcBPaLh7lDCPvS
```

After logging in, we know that we have to submit the password of the current level to port 30000 which means the there is a application running at that port and its listenning. we can test all the commands as shown below to find out what we can do with it.

```bash
whatis nc 
# Output: nc (1) - arbitrary TCP and UDP connections and listens
```

Now that we found which command to use for submitting the password. we can then use the `nc` as shown below. 

```bash
nc localhost 30000
MU4VWeTyJk8ROof1qqmcBPaLh7lDCPvS
# Output: FLAG{8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo}
```
---

## Flag

**Flag:** 8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo

---

## Lessons Learned

It was a easier challange, the good learning was the usage of `nc` command and how to submit the data. 