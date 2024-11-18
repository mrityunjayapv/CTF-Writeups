# CTF Writeup - **OverTheWire Bandit13 - Bandit14**

**Author**: spiketyson  

---

## Challenge Details

- **Challenge Name:** Bandit13 - Bandit14
- **Category:** Linux
- **Difficulty Level:** Easy
- **Date:** 18-11-2024

---

## Summary

This challenge is the level13 for the Bandit series on OverTheWire, focusing on basic Linux command-line skills. The goal is retrive the flag from /etc/bandit_pass/bandit14 and can only be read by user bandit14. We need to get the ssh key.
---

## Approach

Since this is an basic challenge, I planned to use basic Linux commands like `ssh`, `ls`, and `cat` to explore the server and retrieve the flag.

---

## Solution Walkthrough

Connecting to the server using the `ssh` command as below:

```bash
ssh bandit13@bandit.labs.overthewire.org -p 2220
# Password: FO5dwFsc0cbaIiH0h8J2eUks2vdTDwAn
```

After logging in, I used the `ls` command to list the files in the directory and found a file named `data.txt`.

```bash
ls -la
# Output: sshkey.private
```

Now that I have found the file with which we can login to `bandit14`. We can use `ssh` command to login to the bandit14 as shown below. 

```bash
ssh -i sshkey.private bandit14@localhost
# bandit14$ 
```

Now we have access to the password to the file. we can use `cat` command to view the password thats there at  `/etc/bandit_pass/bandit14` as shown below 

```bash
cat /etc/bandit_pass/bandit14
# Output: FLAG{MU4VWeTyJk8ROof1qqmcBPaLh7lDCPvS}
```
---

## Flag

**Flag:** MU4VWeTyJk8ROof1qqmcBPaLh7lDCPvS

---

## Lessons Learned

It was a easier challange, the good learning was the usage of `ssh` command and how to login to a system when we have the sshkey alone. 