# CTF Writeup - **OverTheWire Bandit25 - Bandit26**

**Author**: spiketyson  

---

## Challenge Details

- **Challenge Name:** Bandit25 - Bandit26
- **Category:** Linux
- **Difficulty Level:** Easy
- **Date:** 25-11-2024

---

## Summary

This challenge is the level25 for the Bandit series on OverTheWire, focusing on basic Linux command-line skills. Logging in to bandit26 from bandit25 should be fairly easy… The shell for user bandit26 is not /bin/bash, but something else. Find out what it is, how it works and how to break out of it.
---

## Approach

Since this is an easy challenge, I planned to use basic Linux commands like `ls`, `grep`, `ssh`,`more` to explore the server and retrieve the flag.

---

## Solution Walkthrough

Connect to the server using the `ssh` command as below:

```bash
ssh bandit25@bandit.labs.overthewire.org -p 2220
# Password: iCi86ttT4KSNe1armKiwbQNmB3YJP3q4
```

After logging in, Lets explore the directory using `ls` as shown below
```bash
ls -la
# Output:
# total 40
# drwxr-xr-x  2 root     root     4096 Sep 19 07:08 .
# drwxr-xr-x 70 root     root     4096 Sep 19 07:09 ..
# -rw-r-----  1 bandit25 bandit25   33 Sep 19 07:08 .bandit24.password
# -r--------  1 bandit25 bandit25 1679 Sep 19 07:08 bandit26.sshkey
# -rw-r-----  1 bandit25 bandit25  151 Sep 19 07:08 .banner
# -rw-r--r--  1 root     root      220 Mar 31  2024 .bash_logout
# -rw-r--r--  1 root     root     3771 Mar 31  2024 .bashrc
# -rw-r-----  1 bandit25 bandit25   66 Sep 19 07:08 .flag
# -rw-r-----  1 bandit25 bandit25    4 Sep 19 07:08 .pin
# -rw-r--r--  1 root     root      807 Mar 31  2024 .profile 

```

Here we have found out the sshkey for loggin in to the `bandit26` level. We also got a clue that the `bandit26` shell is not `bin\bash`. Note that each user will have a shell which we can call it the environment they want. This is different for different users or systems. There is a common place `/etc/passwd` where this informaiton is stored. Lets try to see what is the shell of the `bandit26` as shown below.

```bash
cat /etc/passwd | grep bandit26
# bandit26:x:11026:11026:bandit level 26:/home/bandit26:/usr/bin/showtext
```

Here we can see that the shell is something different from the usual. It executes the code stored int `usr/bin/showtext` before opening the connection to any user in bandit26. Lets explore the data in the file as shown below.

```bash
cat /usr/bin/showtext
# Output:
# #!/bin/sh
# export TERM=linux
# exec more ~/text.txt
# exit 0
```
The above script sets the `TERM` to `Linux` and then next line is an interesting line as it executes the `more` command on the `text.txt` This means that more is running in an interactive mode, allowing us to manipulate the file. We can take advantage of this one while connecting to the bandit26 as shown below.

```bash
# In order to trigger more with interactive mode in the terminal we can reduce the size of the terminal as small as possible the execute the below from bandit25.
ssh bandit26@bandit.labs.overthewire.org -p 2220
# Yes
# --More--(12%)  --> Interactive mode.
```

In interactive mode, we can press `v` to edit the file using the editor set. Then we know that the password is generally store in `/etc/bandit_pass/bandit26` and all we have to do is read this file in the interactive mode. Inorder to edit a particular file we can do that by using `:e` after pressing `v` which enables us to edit a particular file as shown below.

```bash
:e /etc/bandit_pass/bandit26
# FLAG{s0773xxkk0MXfdqOfPRVr9L3jJBUOgCZ}
```
We can also set the shell to use /bin/bash as discussed below, So that it would be helpful for use to connect via ssh to the next level. This can be done when we open the ssh connection and more in interactive mode where the terminal is minimized when can then press `v` and then execute `:set shell=/bin/bash` and then press `Enter`. This will give us access to `bandit26` terminal with `/bin/bash` shell.

---

## Flag

**Flag:**  s0773xxkk0MXfdqOfPRVr9L3jJBUOgCZ

---

## Lessons Learned
It was a good challenge, I was stuck in this for a very long time as i couldnt understand what the trick was but i got help from internet to know about how to solve it  The idea here is that the `bandit26` shell is left running the more command forever which has a vulnerability which we can take advantage of. 