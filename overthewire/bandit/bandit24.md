# CTF Writeup - **OverTheWire Bandit24 - Bandit25**

**Author**: spiketyson  

---

## Challenge Details

- **Challenge Name:** Bandit24 - Bandit25
- **Category:** Linux
- **Difficulty Level:** Easy
- **Date:** 22-11-2024

---

## Summary

This challenge is the level22 for the Bandit series on OverTheWire, focusing on basic Linux command-line skills. A daemon is listening on port 30002 and will give you the password for bandit25 if given the password for bandit24 and a secret numeric 4-digit pincode. There is no way to retrieve the pincode except by going through all of the 10000 combinations, called brute-forcing. You do not need to create new connections each time.
---

## Approach

Since this is an easy challenge, I planned to use basic Linux commands like `ls`, `nano`, `cd`,`mktemp` to explore the server and retrieve the flag.

---

## Solution Walkthrough

Connect to the server using the `ssh` command as below:

```bash
ssh bandit24@bandit.labs.overthewire.org -p 2220
# Password: gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8
```

After logging in, Lets create a temporary directory using `mktemp` so that we can do work on creating the possibilities and result file. 

```bash
mktemp -d
# Output: 
# /tmp/tmp.Ot37KX9Lf8
cd /tmp/tmp.Ot37KX9Lf8
```

Now we know that there is a service running on port 30002. we need to connect to it and give the password and the 4 digit code. Lets try doing that now. 

```bash
nc localhost 30002
# I am the pincode checker for user bandit25. Please enter the password for user bandit24 and the secret pincode on a single line, separated by a space.
gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8 1235
# Wrong! Please enter the correct pincode. Try again.
```

After trying out a sample possibility for the service, We know that there are 10000 possibilities that we need to try but that is very time consuming, Instead we can create a file that has all the possibilities from {0000-9999} using a bash script. We can feed this file as an input to the service to try all possible combination and get the corresponding output for each one as shown below in the script.

```bash
# #!/bin/bash
# for i in {0000..9999}
# do
#         echo gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8 $i >> possibilities.txt
# done
# cat possibilities.txt | nc localhost 30002 > result.txt
```
The above script generates all possible combination which looks something like 

```bash
# gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8 0000
# gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8 0001
# gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8 0002
# gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8 0003
# ...
```

This file is fed to the service using the piping in `cat possibilities.txt | nc localhost 30002 > result.txt` This in turn copies the output produced by the service for each input to result.txt. We already know that if the password is wrong it would produce `Wrong!` string. We can filter it out from the file to get the password of the next level as shown below. 

```bash
sort result.txt | grep -v "Wrong!"
# FLAG{iCi86ttT4KSNe1armKiwbQNmB3YJP3q4}
```

---

## Flag

**Flag:**  iCi86ttT4KSNe1armKiwbQNmB3YJP3q4

---

## Lessons Learned
It was a good challenge that made me think how I can create a bash file to automate processes that i that are time consuming. The learning here was getting used to shell script.