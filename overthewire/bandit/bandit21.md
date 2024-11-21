# CTF Writeup - **OverTheWire Bandit21 - Bandit22**

**Author**: spiketyson  

---

## Challenge Details

- **Challenge Name:** Bandit21 - Bandit22
- **Category:** Linux
- **Difficulty Level:** Easy
- **Date:** 21-11-2024

---

## Summary

This challenge is the level21 for the Bandit series on OverTheWire, focusing on basic Linux command-line skills. A program is running automatically at regular intervals from cron, the time-based job scheduler. Look in /etc/cron.d/ for the configuration and see what command is being executed.
---

## Approach

Since this is an easy challenge, I planned to use basic Linux commands like `ls`, `cat`, `cd` to explore the server and retrieve the flag.

---

## Solution Walkthrough

Connect to the server using the `ssh` command as below:

```bash
ssh bandit21@bandit.labs.overthewire.org -p 2220
# Password: EeoULMCra2q0dSkYj561DX7s1CpBuOBt
```

After logging in, Lets explore the `/etc/cron.d` directory using ls command as shown below.

```bash
ls -la /etc/cron.d
# Output: 
# total 44
# drwxr-xr-x   2 root root  4096 Sep 19 07:10 .
# drwxr-xr-x 121 root root 12288 Sep 20 18:37 ..
# -rw-r--r--   1 root root   120 Sep 19 07:08 cronjob_bandit22
# -rw-r--r--   1 root root   122 Sep 19 07:08 cronjob_bandit23
# -rw-r--r--   1 root root   120 Sep 19 07:08 cronjob_bandit24
# -rw-r--r--   1 root root   201 Apr  8  2024 e2scrub_all
# -rwx------   1 root root    52 Sep 19 07:10 otw-tmp-dir
# -rw-r--r--   1 root root   102 Mar 31  2024 .placeholder
# -rw-r--r--   1 root root   396 Jan  9  2024 sysstat
```

Here we can find a file related to bandit22. As we are looking for bandit22 `password` `cronjob_bandit22` seems interesting. lets see the contents of the file.  

```bash
cat cronjob_bandit22
# @reboot bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
# * * * * * bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
```

Cronjobs are programs run automatically at regular intervals. In Linux, there are multiple folders that can contain these cronjobs. We know that at regular interval `cronjob_bandit22` is being executed. Here we can see that `cronjob_bandit22` is executing the file `/usr/bin/cronjob_bandit22.sh`
Lets see the contents of this file. 

```bash
cat /usr/bin/cronjob_bandit22.sh
# !/bin/bash
# chmod 644 /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
# cat /etc/bandit_pass/bandit22 > /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
```

It looks like the `cronjob_bandit22.sh` is changing the permission of a temporary file located at tmp directory `/tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv` and it is copying the password of bandit22 which is stored generally in `/etc/bandit_pass/bandit22` to the temp file. We have found out that this file has the password for next level. we can display the contents of the file as shown below. 


```bash
 cat /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
# FLAG{tRae0UfB9v0UzbCdn9cY0gQnds9GF58Q}
```

---

## Flag

**Flag:**  tRae0UfB9v0UzbCdn9cY0gQnds9GF58Q

---

## Lessons Learned

It was a easy challenge which gave me the exact clues i needed. It was very obvious the we need to look for something related bandit22 in the cronjobs.