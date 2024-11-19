# CTF Writeup - **OverTheWire Bandit18 - Bandit19**

**Author**: spiketyson  

---

## Challenge Details

- **Challenge Name:** Bandit18 - Bandit19
- **Category:** Linux
- **Difficulty Level:** Easy
- **Date:** 19-11-2024

---

## Summary

This challenge is the level18 for the Bandit series on OverTheWire, focusing on basic Linux command-line skills. The password for the next level is stored in a file readme in the homedirectory. Unfortunately, someone has modified .bashrc to log you out when you log in with SSH.
---

## Approach

Since this is an basic challenge, I planned to use basic Linux commands like `ls`, `cat`, `ssh` to explore the server and retrieve the flag.

---

## Solution Walkthrough

Connect to the server using the `ssh` command as below:

```bash
ssh bandit18@bandit.labs.overthewire.org -p 2220
# Password: x2gLTTjFwMOhQ8oWNbMN362QKxfRqGlO
```

Ouch, It logs us out even though we have entered correct password. Before proceeding we need to understand the concept behind `ssh` connection. SSH connection enables secure remote access, encrypted data communications, and strong authentication. We we connect to a server using `ssh`. It creates a secure environment that can be used to access the remote server. In order to create this secure environment for every user who connects it has some predefined rules/intructions to be followed for setting up the enviroment. These rules/instruction are executed before the user has control to access the files and is stored in the file called `.bashrc`. The idea here is that the .bashrc file is messed up so it doesn't let us login. We need to login to the server using `ssh` but when loging in it should not follow the rules that are there in the .bashrc file. In order to do that we can use `-t` to Allocate a terminal, and `sh` to start an `sh` shell instead of the default `bash`. Since `sh` doesn’t load .`bashrc`, this bypasses the .bashrc file entirely.

```bash
ssh bandit18@bandit.labs.overthewire.org -p 2220 -t "sh"
# Output: x2gLTTjFwMOhQ8oWNbMN362QKxfRqGlO
```

Now that we have logged into the server. we know that the file in the home directory has the flag. we can use `ls` command to list all the files and then read the file using `cat` command as shown below.

```bash
ls 
# Output: readme
cat readme
# Output: FLAG{cGWpMaKXVwDUNgPAVJbWYuGHVn9zl3j8}

```
---

## Flag

**Flag:**  cGWpMaKXVwDUNgPAVJbWYuGHVn9zl3j8

---

## Lessons Learned

It was a good challenge which made me understand the concepts behind `ssh` connection and why we need the `.bashrc` file.