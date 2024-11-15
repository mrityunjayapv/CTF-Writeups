# CTF Writeup - **OverTheWire Bandit6 - Bandit7**

**Author**: spiketyson  

---

## Challenge Details

- **Challenge Name:** Bandit6 - Bandit7
- **Category:** Linux
- **Difficulty Level:** Easy
- **Date:** 15-11-2024

---

## Summary

This challenge is the level6 for the Bandit series on OverTheWire, focusing on basic Linux command-line skills. The goal is to connect to a server via SSH and find and read the file that has flag.

---

## Approach

Since this is an basic challenge, I planned to use basic Linux commands like `ssh`, `ls`,`find`, and `cat` to explore the server and retrieve the flag.

---

## Solution Walkthrough

Connecting to the server using the `ssh` command as below:

```bash
ssh bandit6@bandit.labs.overthewire.org -p 2220
# Password: HWasnPhtq9AVKe0dmk45nxy20cvUa6EG
```

After logging in, I used the `ls` command to list the files in the directory and found a nothing.

```bash
ls -la
# Output: <BLANK>
```

As we have lot of files and directories in the system. we cannot go through each directory/files and read it, as it is time consuming process. Instead we know that the flag is in Human readable format (ASCII) and it is a non executable file and it size is 33 bytes and its owner is bandit7 and group is bandit6. with this we can use the `find` command to go through the entire file system by using `/` as shown below. 


```bash
find / -type f -size 1033c ! -executable -exec file {} + | grep ASCII
# Output: /var/lib/dpkg/info/bandit7.password: ASCII text
```

The above command listed all the files and finally listed the file with the flag. using `cat` to display the file as shown below. 

```bash
cat /var/lib/dpkg/info/bandit7.password
# Output: FLAG{morbNTDkSW6jIlUc0ymOdMaLnOlFVAaj}
```
---

## Flag

**Flag:** morbNTDkSW6jIlUc0ymOdMaLnOlFVAaj

---

## Lessons Learned

It was a simple challange, the idea to find the files using `find` command in the whole file system by using `/` in the find command to search entirely was the trick here.