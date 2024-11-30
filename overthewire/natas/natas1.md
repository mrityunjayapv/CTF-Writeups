# CTF Writeup - **OverTheWire Natas0 - Natas1**

**Author**: spiketyson 

---

## Challenge Details

- **Challenge Name:** Natas0 - Natas1
- **Category:** Linux
- **Difficulty Level:** Easy
- **Date:** 30-11-2024

---

## Summary

This challenge is the introductory level for the Natas series on OverTheWire, focusing on web exploitation. The goal is to find the flag that is hidden web server.

---

## Approach

Since this is an basic challenge, I planned to see the page source of the webpage to see if there is any flag hidden in the webpages or css files.

---

## Solution Walkthrough

Connecting to the web server using:

```bash
Username: natas1
Password: 0nzCigAq7t2iALyvU9xcHlYN4MlkIwlq
URL:      http://natas1.natas.labs.overthewire.org
```

After logging in, we can see that the webpage says `You can find the password for the next level on this page, but rightclicking has been blocked!`. Since the right click is blocked, we can still inspect element the whole webpage by clicking of `F12` which opens the inspect element of the webpage. Now that we have got access to the inspect element, We can see that the inspected webpage has a comment on the html page which has the password as shown below. 


```bash
<!--The password for natas2 is TguMNxKo1DSa1tujBLuZJnDUlCcUAPlI -->
```

---

## Flag

**Flag:** TguMNxKo1DSa1tujBLuZJnDUlCcUAPlI

---

## Lessons Learned

It was a good refresher for using inspect element and view page source.
