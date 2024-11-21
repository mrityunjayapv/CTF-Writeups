# CTF Writeup - **OverTheWire Bandit21 - Bandit22**

**Author**: spiketyson  

---

## Challenge Details

- **Challenge Name:** Bandit22 - Bandit23
- **Category:** Linux
- **Difficulty Level:** Easy
- **Date:** 21-11-2024

---

## Summary

This challenge is the level22 for the Bandit series on OverTheWire, focusing on basic Linux command-line skills. A program is running automatically at regular intervals from cron, the time-based job scheduler. Look in /etc/cron.d/ for the configuration and see what command is being executed.
---

## Approach

Since this is an easy challenge, I planned to use basic Linux commands like `ls`, `cat`, `cd` to explore the server and retrieve the flag.

---

## Solution Walkthrough

Connect to the server using the `ssh` command as below:

```bash
ssh bandit22@bandit.labs.overthewire.org -p 2220
# Password: tRae0UfB9v0UzbCdn9cY0gQnds9GF58Q
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

Here we can find a file related to bandit23. As we are looking for bandit23 password `cronjob_bandit23` seems interesting. lets see the contents of the file.  

```bash
cat cronjob_bandit23
# @reboot bandit23 /usr/bin/cronjob_bandit23.sh &> /dev/null
# * * * * * bandit23 /usr/bin/cronjob_bandit23.sh &> /dev/null
```

Cronjobs are programs run automatically at regular intervals. In Linux, there are multiple folders that can contain these cronjobs. We know that at regular interval `cronjob_bandit23` is being executed. Here we can see that `cronjob_bandit23` is executing the file `/usr/bin/cronjob_bandit23.sh`
Lets see the contents of this file. 

```bash
cat /usr/bin/cronjob_bandit23.sh
# #!/bin/bash
# myname=$(whoami)
# mytarget=$(echo I am user $myname | md5sum | cut -d ' ' -f 1)
# echo "Copying passwordfile /etc/bandit_pass/$myname to /tmp/$mytarget"
# cat /etc/bandit_pass/$myname > /tmp/$mytarget
```

It looks like the `cronjob_bandit23.sh` is getting the current user information by execution `whoami` which in our case will be `bandit22`, with this name it creates a command where it echos the `I am user bandit22` string and then calculates the `md5sum` of this string after removing the spaces in the sentences by using the `cut` command. After calculating the `md5sum`, The password of the current user is transferred to the file which is the md5sum `8169b67bd894ddbb4412f91573b38db3` named in the tmp directory.Here we need to note one thing clearly, we need to be bandit23 to get the password if we are executing this file or We need to replicate what ever is done in the file manually and the access the md5sum named file in the `tmp` directory. The steps followed are shown below.


```bash
./usr/bin/cronjob_bandit23.sh
# Copying passwordfile /etc/bandit_pass/bandit22 to /tmp/8169b67bd894ddbb4412f91573b38db3
cat /tmp/8169b67bd894ddbb4412f91573b38db3
# tRae0UfB9v0UzbCdn9cY0gQnds9GF58Q --> current password
```

We can see that when we execute the file it gives us the password for current system. But we need to password of `bandit23`. We also know that the cronjobs are executed periodically. which means the there is a file which has the password copied to it already, We just need to find the `md5sum` of the `bandit23` inorder to access the file shown below.

```bash
echo I am user bandit23 | md5sum | cut -d ' ' -f 1
# 8ca319486bfbbc3663ea0fbe81326349
cat /tmp/8ca319486bfbbc3663ea0fbe81326349
# FLAG{0Zf11ioIjMVN551jX3CmStKLYqjk54Ga}
```

---

## Flag

**Flag:**  0Zf11ioIjMVN551jX3CmStKLYqjk54Ga

---

## Lessons Learned

It was a easy challenge which gave me the exact clues i needed. It was very obvious the we need to look for something related bandit23 in the cronjobs.