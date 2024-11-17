# CTF Writeup - **OverTheWire Bandit10 - Bandit11**

**Author**: spiketyson  

---

## Challenge Details

- **Challenge Name:** Bandit10 - Bandit11
- **Category:** Linux
- **Difficulty Level:** Easy
- **Date:** 17-11-2024

---

## Summary

This challenge is the level10 for the Bandit series on OverTheWire, focusing on basic Linux command-line skills. The goal is to access file data.txt which is base64 encoded.
---

## Approach

Since this is an basic challenge, I planned to use basic Linux commands like `ssh`, `ls`,`base64`, and `cat` to explore the server and retrieve the flag.

---

## Solution Walkthrough

Connecting to the server using the `ssh` command as below:

```bash
ssh bandit10@bandit.labs.overthewire.org -p 2220
# Password: FGUW5ilLVJrxX9kMYMmlN4MgbpfMiqey
```

After logging in, I used the `ls` command to list the files in the directory and found a file named `data.txt`.

```bash
ls -la
# Output: data.txt
```

Now that I have found the file with the flag. We can use `cat` command to display the contents of the file. 

```bash
cat data.txt
# Output: VGhlIHBhc3N3b3JkIGlzIGR0UjE3M2ZaS2IwUlJzREZTR3NnMlJXbnBOVmozcVJyCg==
```

The clue given is that data is base64 encoded as it ends with `==`. Now we can decode the data in the file using `base64`  

```bash
cat data.txt | base64 -d
# Output: The password is dtR173fZKb0RRsDFSGsg2RWnpNVj3qRr
```
---

## Flag

**Flag:** dtR173fZKb0RRsDFSGsg2RWnpNVj3qRr

---

## Lessons Learned

It was a easier challange, the idea is to decode data in the file using `base64`.