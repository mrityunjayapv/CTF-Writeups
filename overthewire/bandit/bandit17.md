# CTF Writeup - **OverTheWire Bandit17 - Bandit18**

**Author**: spiketyson  

---

## Challenge Details

- **Challenge Name:** Bandit17 - Bandit18
- **Category:** Linux
- **Difficulty Level:** Easy
- **Date:** 19-11-2024

---

## Summary

This challenge is the level17 for the Bandit series on OverTheWire, focusing on basic Linux command-line skills. There are 2 files in the homedirectory: passwords.old and passwords.new. The password for the next level is in passwords.new and is the only line that has been changed between passwords.old and passwords.new
---

## Approach

Since this is an basic challenge, I planned to use basic Linux commands like `ls`, `cat`, `grep` and `diff` to explore the server and retrieve the flag.

---

## Solution Walkthrough

Before connecting to server we need to modify the permission of the key file so that i has restriced access in order to avoid permission issues. We can do this by using `chmod` to change the permission of the key file. Now Connect to the server using the `ssh` command as below:

```bash
chmod 400 private.key
ssh -i private.key bandit17@bandit.labs.overthewire.org -p 2220
```

After logging in, We can use the `ls` command to list the files in the directory and found 2 files named `passwords.old` and `passwords.new`.

```bash
ls -la 
# Output: passwords.new  passwords.old
```

Now we know that the flag is in the files. We use `cat` command to see the file data.

```bash
cat passwords.old
# Output: 
# BTrrmkKVXra01wWzxqzFmAMY9CbPXRlc
# zxYJ8Detw68SBZluYcJzAkEwrwPB1uCh
# J1mJLnxroUUJC5YEtfHVPq6LXxkWTbsF
# oSRyw7ANImF3fjwvjAnKqcJgL37Eltmf
# ...
cat passwords.new
# Output: 
# BTrrmkKVXra01wWzxqzFmAMY9CbPXRlc
# zxYJ8Detw68SBZluYcJzAkEwrwPB1uCh
# J1mJLnxroUUJC5YEtfHVPq6LXxkWTbsF
# oSRyw7ANImF3fjwvjAnKqcJgL37Eltmf
# ...
```

Both files has a huge amout of data. But one thing we know is that both files have only one line difference. So we can compare the files using `diff` command to compare the files and to show the difference only as shown below.

```bash
diff passwords.old passwords.new --normal
# output 
# 42c42
# < ktfgBvpMzWKR5ENj26IbLGSblgUG9CzB
# ---
# > FLAG{x2gLTTjFwMOhQ8oWNbMN362QKxfRqGlO}

```
---

## Flag

**Flag:**  x2gLTTjFwMOhQ8oWNbMN362QKxfRqGlO

---

## Lessons Learned

It was a easier challange, the learning in the challenge was to know how to use `diff` command.