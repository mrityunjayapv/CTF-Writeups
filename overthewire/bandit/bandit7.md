# CTF Writeup - **OverTheWire Bandit7 - Bandit8**

**Author**: spiketyson  

---

## Challenge Details

- **Challenge Name:** Bandit7 - Bandit8
- **Category:** Linux
- **Difficulty Level:** Easy
- **Date:** 15-11-2024

---

## Summary

This challenge is the level7 for the Bandit series on OverTheWire, focusing on basic Linux command-line skills. The goal is to connect to a server via SSH and find and read the file that has flag.

---

## Approach

Since this is an basic challenge, I planned to use basic Linux commands like `ssh`, `ls`,`grep`, and `cat` to explore the server and retrieve the flag.

---

## Solution Walkthrough

Connecting to the server using the `ssh` command as below:

```bash
ssh bandit6@bandit.labs.overthewire.org -p 2220
# Password: morbNTDkSW6jIlUc0ymOdMaLnOlFVAaj
```

After logging in, I used the `ls` command to list the files in the directory and found a file named `data.txt`.

```bash
ls -la
# Output: data.txt
```

Now that I have found the file with the flag. We can use `cat` command to display the contents of the file. 

```bash
cat data.txt
# Output: seeing  hhrdfoZgoMQmINOrmmZlL5t8sVhDGDWZ renounces       H5pjlsprVRLLDbiSKtxAIG6NSBCkmzq2 ...
```

The above command listed the contents of the file. It seems like there is a lot of content in the files. But we know that the flag is next to the word `millionth`. we can use `grep` for search for the word in the file as below. 

```bash
cat data.txt | grep millionth
# Output: millionth FLAG{dfwvzFQi4mU0wfNbFOe9RoWskMLg7eEc}
```
---

## Flag

**Flag:** dfwvzFQi4mU0wfNbFOe9RoWskMLg7eEc

---

## Lessons Learned

It was a simple challange, the idea to find the files using `grep` command and pipe it with the output of `cat` command.