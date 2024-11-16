# CTF Writeup - **OverTheWire Bandit8 - Bandit9**

**Author**: spiketyson  

---

## Challenge Details

- **Challenge Name:** Bandit8 - Bandit9
- **Category:** Linux
- **Difficulty Level:** Easy
- **Date:** 16-11-2024

---

## Summary

This challenge is the level8 for the Bandit series on OverTheWire, focusing on basic Linux command-line skills. The goal is to connect to a server via SSH and read the file that has flag with a lot of data.

---

## Approach

Since this is an basic challenge, I planned to use basic Linux commands like `ssh`, `ls`,`sort`, and `uniq` to explore the server and retrieve the flag.

---

## Solution Walkthrough

Connecting to the server using the `ssh` command as below:

```bash
ssh bandit8@bandit.labs.overthewire.org -p 2220
# Password: dfwvzFQi4mU0wfNbFOe9RoWskMLg7eEc
```

After logging in, I used the `ls` command to list the files in the directory and found a file named `data.txt`.

```bash
ls -la
# Output: data.txt
```

Now that I have found the file with the flag. We can use `cat` command to display the contents of the file. 

```bash
cat data.txt
# Output: CgUjZiluCoMEvzNAge1Nbv3g9tpLQQj2   UuNP4xguSOjcTHAzdtHBgm2eNz1Z5133 ...
```

There is a lot of data in the file. But we have the clue that the flag appears only once in the whole file. we can use the `sort` command to sort the data in the file and then pass the data to find the uniq value in files as showing below. 

```bash
sort data.txt | uniq -uc
# Output: 1 FLAG{4CKMh1JI91bUIZZPXDqGanal4xvAg0JM}
```
---

## Flag

**Flag:** 4CKMh1JI91bUIZZPXDqGanal4xvAg0JM

---

## Lessons Learned

It was a easier challange, the idea to learn how to use the `sort` and `uniq` commands and the use pipe to send the output of one command to input of other command.