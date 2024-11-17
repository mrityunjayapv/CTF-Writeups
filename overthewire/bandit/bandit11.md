# CTF Writeup - **OverTheWire Bandit11 - Bandit12**

**Author**: spiketyson  

---

## Challenge Details

- **Challenge Name:** Bandit11 - Bandit12
- **Category:** Linux
- **Difficulty Level:** Easy
- **Date:** 17-11-2024

---

## Summary

This challenge is the level11 for the Bandit series on OverTheWire, focusing on basic Linux command-line skills. The goal is to access file data.txt where all lowercase (a-z) and uppercase (A-Z) letters have been rotated by 13 positions.
---

## Approach

Since this is an basic challenge, I planned to use basic Linux commands like `ssh`, `ls`,`tr`, and `cat` to explore the server and retrieve the flag.

---

## Solution Walkthrough

Connecting to the server using the `ssh` command as below:

```bash
ssh bandit11@bandit.labs.overthewire.org -p 2220
# Password: dtR173fZKb0RRsDFSGsg2RWnpNVj3qRr
```

After logging in, I used the `ls` command to list the files in the directory and found a file named `data.txt`.

```bash
ls -la
# Output: data.txt
```

Now that I have found the file with the flag. We can use `cat` command to display the contents of the file. 

```bash
cat data.txt
# Output: Gur cnffjbeq vf 7k16JArUVv5LxVuJfsSVdbbtaHGlw9D4
```

The clue given is that all lowercase (a-z) and uppercase (A-Z) letters have been rotated by 13 positions. Now we can decode the data in the file using `tr` command which translates characters, With the clue we know that `a -> n`, `b -> o` and so on similarly `A -> N`, `Z -> M` after the rotation, so all we need to do is the we need to rotate it again 13 positions which will result the same string before encryption as there are only 26 alphabets.      

```bash
cat data.txt | tr 'a-zA-Z' 'n-za-mN-ZA-M'
# Output: The password is 7x16WNeHIi5YkIhWsfFIqoognUTyj9Q4
```
---

## Flag

**Flag:** 7x16WNeHIi5YkIhWsfFIqoognUTyj9Q4

---

## Lessons Learned

It was a easier challange, the good learning was the usage of `tr` command for translations and piping the output of `cat` command as input for `tr`.