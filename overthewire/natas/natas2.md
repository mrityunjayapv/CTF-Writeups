# CTF Writeup - **OverTheWire Natas1 - Natas2**

**Author**: spiketyson 

---

## Challenge Details

- **Challenge Name:** Natas1 - Natas2
- **Category:** Linux
- **Difficulty Level:** Easy
- **Date:** 30-11-2024

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
Username: natas2
Password: TguMNxKo1DSa1tujBLuZJnDUlCcUAPlI
URL:      http://natas2.natas.labs.overthewire.org
```

After logging in, we can see that the webpage says `There is nothing on this page `.  Since its says there is nothing on this webpage, it gives us clue that the password of the next level can be on someother files on the webserver. We can now inspect element using `F12` and look for other pages on the web server in `sources` tab.

```bash
natas2.natas.labs.overthewire.org
    > Files
        pixel.png
    (index)
```

We can see there is a directory named `Files` which has `pixel.png` when we open this image its not very useful for us. But we need to note that there can be multiple files in the Files folder that are not getting displayed for security reasons to the user. Since its a web server it has a format in which we can access the files and directory. For example, The webpage is rendered with `http://natas2.natas.labs.overthewire.org/index.html`but this index.html is hidden in order to avoid routing problems. If we want to access the files we can do it using relative path. Since the `Files` directory under the `natas2.natas.labs.overthewire.org` we can simply go to `http://natas2.natas.labs.overthewire.org/Files` and see all the files. 

```bash
# Index of /files
# [ICO]	Name	Last modified	Size	Description
# [PARENTDIR]	Parent Directory	 	-	 
# [IMG]	pixel.png	2024-09-19 07:03	303	 
# [TXT]	users.txt	2024-09-19 07:03	145	 
# Apache/2.4.58 (Ubuntu) Server at natas2.natas.labs.overthewire.org Port 80
```

Now we can see that there are totally 2 files in the system. As we have already seen the image lets skip it and view that contents of the `users.txt` as shown below. This file has the password for `natas3`

```bash
# # username:password
# alice:BYNdCesZqW
# bob:jw2ueICLvT
# charlie:G5vCxkVV3m
# natas3:3gqisGdR0pjm6tpkDKdIWO2hSvchLeYH
# eve:zo4mJWyNj2
# mallory:9urtcpzBmH
```

---

## Flag

**Flag:** 3gqisGdR0pjm6tpkDKdIWO2hSvchLeYH

---

## Lessons Learned

It was a good challenge which made me understand the working of the webserver and how we can access files in the system.
