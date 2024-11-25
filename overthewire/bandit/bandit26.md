# CTF Writeup - **OverTheWire Bandit26 - Bandit27**

**Author**: spiketyson  

---

## Challenge Details

- **Challenge Name:** Bandit26 - Bandit27
- **Category:** Linux
- **Difficulty Level:** Easy
- **Date:** 25-11-2024

---

## Summary

This challenge is the level26 for the Bandit series on OverTheWire, focusing on basic Linux command-line skills. Explore the system and grab the password for bandit27.
---

## Approach

Since this is an easy challenge, I planned to use basic Linux commands like `ls`, `SUID`, `cat` to explore the server and retrieve the flag.

---

## Solution Walkthrough

Connect cannot connect the server using the `ssh` command as below as there is not proper shell for us to connect to terminal. we can only do it use the `bandit25` solution. We will have access to the terminal after following the last part of the walkthrough in `bandit25`. Now that we have a shell. Lets explore the server and find the flag.

```bash
ssh bandit26@bandit.labs.overthewire.org -p 2220 -t sh
# Password: s0773xxkk0MXfdqOfPRVr9L3jJBUOgCZ
```

After getting the shell, Lets explore the directory using `ls` as shown below
```bash
ls -la
# Output:
# total 44
# drwxr-xr-x  3 root     root      4096 Sep 19 07:08 .
# drwxr-xr-x 70 root     root      4096 Sep 19 07:09 ..
# -rwsr-x---  1 bandit27 bandit26 14880 Sep 19 07:08 bandit27-do
# -rw-r--r--  1 root     root       220 Mar 31  2024 .bash_logout
# -rw-r--r--  1 root     root      3771 Mar 31  2024 .bashrc
# -rw-r--r--  1 root     root       807 Mar 31  2024 .profile
# drwxr-xr-x  2 root     root      4096 Sep 19 07:08 .ssh
# -rw-r-----  1 bandit26 bandit26   258 Sep 19 07:08 text.txt
```

Here we have a interesting file `bandit27-do` which is similar to one of the previous challenges where the an application which executes commands for us as the owner. We can try to execute the `bandit27-do` and find out what is does as shown below. 

```bash
./bandit27-do
# Run a command as another user.
#   Example: ./bandit27-do id
```

Here can see that the executable file needs a command to run. We also know that the password of `bandit27` is stored in `/etc/bandit_pass/bandit27` which can be accessed only by `bandit27` user but we have an application which executes as `bandit27` we can take advantage of this as shown below. 

```bash
./bandit27-do cat /etc/bandit_pass/bandit27
# FLAG{upsNCc7vzaRDx6oZC6GiR6ERwe1MowGB}
```

---

## Flag

**Flag:**  upsNCc7vzaRDx6oZC6GiR6ERwe1MowGB

---

## Lessons Learned
It was a good challenge, I was stuck in the begining of the challenge as to how we need to connect to the server. But once i understood that we need to exploit the bug in previous challenge to set the shell to /bin/bash and then use `:shell` to open the shell so that we can access the server, it became straightforward after that. 