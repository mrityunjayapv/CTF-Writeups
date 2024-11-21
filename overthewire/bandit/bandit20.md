# CTF Writeup - **OverTheWire Bandit20 - Bandit21**

**Author**: spiketyson  

---

## Challenge Details

- **Challenge Name:** Bandit20 - Bandit21
- **Category:** Linux
- **Difficulty Level:** Easy
- **Date:** 21-11-2024

---

## Summary

This challenge is the level20 for the Bandit series on OverTheWire, focusing on basic Linux command-line skills. There is a setuid binary in the homedirectory that does the following: it makes a connection to localhost on the port you specify as a commandline argument. It then reads a line of text from the connection and compares it to the password in the previous level (bandit20). If the password is correct, it will transmit the password for the next level (bandit21).
---

## Approach

Since this is an moderate challenge, I planned to use basic Linux commands like `ls`, `nc`, `ssh` to explore the server and retrieve the flag.

---

## Solution Walkthrough

Connect to the server using the `ssh` command as below:

```bash
ssh bandit20@bandit.labs.overthewire.org -p 2220
# Password: 0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO
```

After logging in, we can explore the server by using `ls` command as shown below. Here we can find the `suconnect` file. As we have seen a similar challenge before, lets try executing the program and understand it. 

```bash
ls -ls
# Output: 
# total 36
# drwxr-xr-x  2 root     root      4096 Sep 19 07:08 .
# drwxr-xr-x 70 root     root      4096 Sep 19 07:09 ..
# -rw-r--r--  1 root     root       220 Mar 31  2024 .bash_logout
# -rw-r--r--  1 root     root      3771 Mar 31  2024 .bashrc
# -rw-r--r--  1 root     root       807 Mar 31  2024 .profile
# -rwsr-x---  1 bandit21 bandit20 15604 Sep 19 07:08 suconnect
./suconnect
# Usage: ./suconnect <portnumber>
# This program will connect to the given port on localhost using TCP. If it receives the correct password from the other side, the next password is transmitted back.
```

Before proceeding, lets understand what is happening here. As the execution of `./suconnect` gives us a clue that the program will connect to a given port on our system using TCP and if it gets a input (password of the previous challenge) then it will respond with the correct password as this is given in the challenge description. Now we can to create a temporary server in our sytem using `nc` to run on a port and make a connection to that port using `./suconnect` as shown below.

```bash
nc -l 1234
# This will start running on localhost port 1234
0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO
```

Now that our server is running on port 1234. we can sned the password of bandit20 to that the server can respond it to anyone connecting to it. we can now connect to `bandit20` using `ssh` in another terminals as we have the server runnning. we want out client to connect to it as shown below. After connecting we can see that 


```bash
# Terminal 1
./suconnect 1234
# Read: 0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO
# Password matches, sending next password
```

```bash
# Terminal 2
nc -l 1234 
0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO
# FLAG{EeoULMCra2q0dSkYj561DX7s1CpBuOBt}
```


---

## Flag

**Flag:**  EeoULMCra2q0dSkYj561DX7s1CpBuOBt

---

## Lessons Learned

It was a good challenge which made me think about SUID. The idea to create a server to connect didnt strike to me as i didnt get why we need to do that. But latter i understood that the program is trying to connect to something so we need to create something. 