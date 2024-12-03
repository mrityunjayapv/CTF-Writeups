# CTF Writeup - **OverTheWire Natas7 - Natas8**

**Author**: spiketyson 

---

## Challenge Details

- **Challenge Name:** Natas7 - Natas8
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
Username: natas8
Password: xcoXLmzMkoIP9D7hlgPlh9XD7OgLAe5Q
URL:      http://natas8.natas.labs.overthewire.org
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
# <script>var wechallinfo = { "level": "natas8", "pass": "<censored>" };</script></head>
# <body>
# <h1>natas8</h1>
# <div id="content">
# <?
# $encodedSecret = "3d3d516343746d4d6d6c315669563362";
# function encodeSecret($secret) {
#     return bin2hex(strrev(base64_encode($secret)));
# }
# if(array_key_exists("submit", $_POST)) {
#     if(encodeSecret($_POST['secret']) == $encodedSecret) {
#     print "Access granted. The password for natas9 is <censored>";
#     } else {
#     print "Wrong secret";
#     }
# }
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

We can see that there is a form which takes our input, and when we submit the input there is a php code which gets executed on the input, Where the code takes our input and passes it to a function called `encodedSecret()`. This function then converts out secret message to `base64` format then the reverses the string using `strrev` and then finally converts the whole string from binary to hexadecimal using `bin2hex`. Finally this string is compared to the encoded string. Since we dont know what input string will produce the encoded secret. Lets go the opposite way of trying to reverse the operation. Lets take the encoded string and convert it to `binary` and the reverse it and the decrypt is using `base64` decoder.
 
```bash
3d3d516343746d4d6d6c315669563362
# hex2bin in php: ==QcCtmMml1ViV3b
==QcCtmMml1ViV3b
# Reversed String: b3ViV1lmMmtCcQ==
b3ViV1lmMmtCcQ==
# base64 decoded: oubWYf2kBq --> This is the secret.
```

Now that we go the secret, Lets try inputting this in the input field and submit th form. This will give us the password for the next level.

```bash
# Access granted. The password for natas9 is ZE1ck82lmdGIoErlhQgWND6j2Wzz6b6t
```

## Flag

**Flag:** ZE1ck82lmdGIoErlhQgWND6j2Wzz6b6t

---

## Lessons Learned

It was a good challenge where i learned about url injection.
