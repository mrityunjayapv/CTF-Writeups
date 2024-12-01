# CTF Writeup - **OverTheWire Natas2 - Natas3**

**Author**: spiketyson 

---

## Challenge Details

- **Challenge Name:** Natas2 - Natas3
- **Category:** Linux
- **Difficulty Level:** Easy
- **Date:** 1-12-2024

---

## Summary

This challenge is the introductory level for the Natas series on OverTheWire, focusing on web exploitation. The goal is to find the flag that is hidden web server.

---

## Approach

Since this is an basic challenge, I planned to see the page source of the webpage to see if there is any flag hidden in the webpages or css files.

---

## Solution Walkthrough

Connecting to the web server using:

```bash
Username: natas3
Password: 3gqisGdR0pjm6tpkDKdIWO2hSvchLeYH
URL:      http://natas3.natas.labs.overthewire.org
```

After logging in, we can see that the webpage says `There is nothing on this page `.  Since its says there is nothing on this webpage, it gives us clue that the password of the next level can be on someother files on the webserver. We can now inspect element using `F12` and look for other pages on the web server in `sources` tab.

```bash
natas2.natas.labs.overthewire.org
    (index)
```

We can see there is a only file named `index.html`. Lets explore this file first.

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
# <script>var wechallinfo = { "level": "natas3", "pass": "3gqisGdR0pjm6tpkDKdIWO2hSvchLeYH" };</script></head>
# <body>
# <h1>natas3</h1>
# <div id="content">
# There is nothing on this page
# <!-- No more information leaks!! Not even Google will find it this time... -->
# </div>
# </body></html>
```

Now we can see that it gives us clue that `No more information leaks!! Not even Google will find it this time`. Before proceeding with the clue we need to understand how web crawlers, search engines and web servers works. Web servers are computer systems that host and deliver web pages or applications to users over the internet. These web pages are typically accessed through a web browser, allowing users from any part of the world to interact with the server. Web crawlers, also known as spiders or bots, are automated programs that systematically browse the internet, accessing and indexing webpages, files, and directories. They help search engines build a comprehensive index of available web content, making searches faster and more efficient by reducing the time users spend finding relevant information. Search engines take input from the user in the form of a query and search through their indexed database, which is built using web crawlers, to find matching webpages. The search results are then displayed to the user in order of relevance. Now the clue says the not even google will find it, which means the the web crawlers cannot access this webpage. These rules of how certain crawlers can access which part of webpages is defined generally in `robots.txt` in the webserver, where they define rules for specific search engines to crawl. Lets access the `robots.txt` and view the contents of it.

```bash
http://natas3.natas.labs.overthewire.org/robots.txt
# User-agent: *
# Disallow: /s3cr3t/
```

Here we can see that it says for any user agent(web crawler) always disallow access to `/s3cr3t/`. Lets see if we can access the webpage.  

```bash
http://natas3.natas.labs.overthewire.org/s3cr3t/
# [TXT]	users.txt	2024-09-19 07:03	40	 
```

we can the that there is a users.txt file in the directory. Lets view the contents of the file. 

```bash
http://natas3.natas.labs.overthewire.org/s3cr3t/users.txt
# natas4:QryZXc2e0zahULdHrtHxzyYkj59kUxLQ
```

## Flag

**Flag:** QryZXc2e0zahULdHrtHxzyYkj59kUxLQ

---

## Lessons Learned

It was a nice challenge where i learned about how search engines, web crawlers and web pages work.
