# CTF Writeup - **OverTheWire Bandit19 - Bandit20**

**Author**: spiketyson  

---

## Challenge Details

- **Challenge Name:** Bandit19 - Bandit20
- **Category:** Linux
- **Difficulty Level:** Easy
- **Date:** 20-11-2024

---

## Summary

This challenge is the level19 for the Bandit series on OverTheWire, focusing on basic Linux command-line skills. To gain access to the next level, you should use the setuid binary in the homedirectory. Execute it without arguments to find out how to use it. The password for this level can be found in the usual place (/etc/bandit_pass), after you have used the setuid binary.
---

## Approach

Since this is an basic challenge, I planned to use basic Linux commands like `ls`, `cat`, `ssh`, `chmod` to explore the server and retrieve the flag.

---

## Solution Walkthrough

Connect to the server using the `ssh` command as below:

```bash
ssh bandit19@bandit.labs.overthewire.org -p 2220
# Password: cGWpMaKXVwDUNgPAVJbWYuGHVn9zl3j8
```

After logging in, we can explore the server by using `ls` command as shown below. Here we can find the `bandit20-do` file. Before proceeding further, we need to understand the concept of SUID, which is a short form of **Set User ID**, it means that when you execute the file, it will run with the permissions of the owner of the file. Here we can see that `bandit20-do` had the owner as `bandit20` with `rws` permission for the user and group as `bandit19` and `r-w` permissions. `rws` means that when the file is executed, it runs with the permissions of the file’s owner..

```bash
ls -ls
# Output: 
# total 36
# drwxr-xr-x  2 root     root      4096 Sep 19 07:08 .
# drwxr-xr-x 70 root     root      4096 Sep 19 07:09 ..
# -rwsr-x---  1 bandit20 bandit19 14880 Sep 19 07:08 bandit20-do
# -rw-r--r--  1 root     root       220 Mar 31  2024 .bash_logout
# -rw-r--r--  1 root     root      3771 Mar 31  2024 .bashrc
# -rw-r--r--  1 root     root       807 Mar 31  2024 .profile
```

Now we know 2 things, First the password is in `/etc/bandit_pass/bandit20` we need to access this file, but we dont have permissions for it. Second, we also know that the bandit20-do file has the permission to access anything as `bandit20` user. Lets try executing the file.

```bash
./bandit20-do
# Output: 
# Run a command as another user.
# Example: ./bandit20-do id
```

we can see that that we can run a command as another user using `./bandit20-do` we can do something like `./bandit20-do ls` or `./bandit20-do cat data.txt`. with this we can now access the file that has the password as we can execute any command as `bandit20` user using this file.   

```bash
./bandit20-do cat /etc/bandit_pass/bandit20
# Output: FLAG{0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO}
```
---

## Flag

**Flag:**  0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO

---

## Lessons Learned

It was a good challenge which made me think about SUID. I wast changing permissions using `chmod` and `chown`. But i didnt really get the idea of executing the file. It really taught me how to think like a attacker. 