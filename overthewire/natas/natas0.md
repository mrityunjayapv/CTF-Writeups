# CTF Writeup - **OverTheWire Natas0**

**Author**: spiketyson 

---

## Challenge Details

- **Challenge Name:** Natas0
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
Username: natas0
Password: natas0
URL:      http://natas0.natas.labs.overthewire.org
```

After logging in, we can see that the webpage says `You can find the password for the next level on this page.`. Lets see the page source of this page using right click on the webpage we can see a bunch of options to choose from but we need to pick `view page source`. When inspected the webpage, we can see that there is a comment on the html page which has the password as shown below. 


```bash
<!--The password for natas1 is 0nzCigAq7t2iALyvU9xcHlYN4MlkIwlq -->
```

---

## Flag

**Flag:** 0nzCigAq7t2iALyvU9xcHlYN4MlkIwlq

---

## Lessons Learned

It was a good refresher for using inspect element and view page source.
