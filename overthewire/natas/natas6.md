# CTF Writeup - **OverTheWire Natas5 - Natas6**

**Author**: spiketyson 

---

## Challenge Details

- **Challenge Name:** Natas5 - Natas6
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
Username: natas6
Password: 0RoJwHdSKWFTYR5WuiAewauSuNaBXned
URL:      http://natas6.natas.labs.overthewire.org
```

After logging in, we can see that the webpage asks for Input secret.  But we dont know anything about it, So lets input some random value and check it out. 

```bash
Input secret: ABC
# Submit
# Wrong secret
```

It says wrong secret, but we can see that there is a source code button available. Lets view the source code of the webpage.

```bash
# <html>
# <head>
# <!-- This stuff in the header has nothing to do with the level -->
# <link rel="stylesheet" type="text/css" href="http://natas.labs.overthewire.org/css/level.css">
# <link rel="stylesheet" href="http://natas.labs.overthewire.org/css/jquery-ui.css" />
# <link rel="stylesheet" href="http://natas.labs.overthewire.org/css/wechall.css" />
# <script src="http://natas.labs.overthewire.org/js/jquery-1.9.1.js"></script>
# <script src="http://natas.labs.overthewire.org/js/jquery-ui.js"></script>
# <script src=http://natas.labs.overthewire.org/js/wechall-data.js></script><script src="http://natas.labs.overthewire.org/js/wechall.js"></script>
# <script>var wechallinfo = { "level": "natas6", "pass": "<censored>" };</script></head>
# <body>
# <h1>natas6</h1>
# <div id="content">
# <?
# include "includes/secret.inc";
#     if(array_key_exists("submit", $_POST)) {
#         if($secret == $_POST['secret']) {
#         print "Access granted. The password for natas7 is <censored>";
#     } else {
#         print "Wrong secret";
#     }
#     }
# ?>
# <form method=post>
# Input secret: <input name=secret><br>
# <input type=submit name=submit>
# </form>
# <div id="viewsource"><a href="index-source.html">View sourcecode</a></div>
# </div>
# </body>
# </html>
```

We can see that there is a form which take our input, and when we submit the input there is a javascript code which gets executed on the input, Where the code gets the secret from `includes` directory which has a file called `secret.ini` and then compares our input with the secret in the file and then outputs the password if the secrets match. Lets see if we can access `secret.ini` through url.

```bash
http://natas6.natas.labs.overthewire.org/includes/secret.inc
# <?
# $secret = "FOEIUWGHFEEUHOFUOIU";
# ?>
```

Here we go, we have got the secret which is expected from us to be inputted. Lets try inputting the `FOEIUWGHFEEUHOFUOIU` in the input field. We got the password for the next level

```bash
# Access granted. The password for natas7 is bmg8SvU1LizuWjx3y7xkNERkHxGre0GS
```

## Flag

**Flag:** bmg8SvU1LizuWjx3y7xkNERkHxGre0GS

---

## Lessons Learned

It was a good challenge where i learned about javascript.
