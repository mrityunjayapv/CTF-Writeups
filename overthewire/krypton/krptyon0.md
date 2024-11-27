# CTF Writeup - **OverTheWire Krypton0 - Krypton1**

**Author**: spiketyson 

---

## Challenge Details

- **Challenge Name:** Krypton0 - Krypton1
- **Category:** Linux
- **Difficulty Level:** Easy
- **Date:** 27-11-2024

---

## Summary

This challenge is the introductory level0 for the Krypton series on OverTheWire, focusing on basic cryptography skills. The goal is to connect to a server via SSH after decoding a base64 string.

---

## Approach

Since this is an introductory challenge, I planned to use `base64` for decoding the `base64` string and then connect to server using `ssh` and explore the server using `ls`, and `cat` to retrieve the flag.

---

## Solution Walkthrough

As we have been give and `base64` encoded string lets use `base64 -d` to decode it to get the password of krypton0 to log into the server.

```bash
echo "S1JZUFRPTklTR1JFQVQ=" | base64 -d
# KRYPTONISGREAT
```
Now that we got the password, we can login to the server using `ssh` ash shown below. 

```bash
ssh krypton1@krypton.labs.overthewire.org -p 2231
# Password: KRYPTONISGREAT
```

After logging in, We can move to `/kryton/` for getting the passwords for other challenge. 

```bash
cd /krypton/
ls -la
# total 36
# drwxr-xr-x  9 root root 4096 Sep 19 07:10 .
# drwxr-xr-x 25 root root 4096 Nov 23 18:05 ..
# drwxr-xr-x  2 root root 4096 Sep 19 07:09 krypton1
# drwxr-xr-x  2 root root 4096 Sep 19 07:09 krypton2
# drwxr-xr-x  2 root root 4096 Sep 19 07:09 krypton3
# drwxr-xr-x  2 root root 4096 Sep 19 07:10 krypton4
# drwxr-xr-x  2 root root 4096 Sep 19 07:10 krypton5
# drwxr-xr-x  3 root root 4096 Sep 19 07:10 krypton6
# drwxr-xr-x  2 root root 4096 Sep 19 07:10 krypton7
```
Now that we are able to see all other challenges. Lets move to the other challenge.

---

## Flag

**Flag:** KRYPTONISGREAT

---

## Lessons Learned

It was a good refresher for using `base64` decoding and understanding what is `base64`.
