# CTF Writeup - **OverTheWire Natas10 - Natas11**

**Author**: spiketyson 

---

## Challenge Details

- **Challenge Name:** Natas10 - Natas11
- **Category:** Linux
- **Difficulty Level:** Easy
- **Date:** 4-12-2024

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
Username: natas11
Password: UJdqkK1pTu6VLt9UHWAgRZz6sVUZ3lEk
URL:      http://natas11.natas.labs.overthewire.org
```

After logging in, we can see that the webpage asks for background color code as input.  But we dont know anything about it, So lets input some random value and check it out. 

```bash
Cookies are protected with XOR encryption
Background color: #000000
# Submit
# Background color changes to black. 
```

Now we need to find the exact color to input to get the flag. Let view the source code. 

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
# <script>var wechallinfo = { "level": "natas11", "pass": "<censored>" };</script></head>
# <?
# $defaultdata = array( "showpassword"=>"no", "bgcolor"=>"#ffffff");

# function xor_encrypt($in) {
#     $key = '<censored>';
#     $text = $in;
#     $outText = '';
#     // Iterate through each character
#     for($i=0;$i<strlen($text);$i++) {
#     $outText .= $text[$i] ^ $key[$i % strlen($key)];
#     }
#     return $outText;
# }
# function loadData($def) {
#     global $_COOKIE;
#     $mydata = $def;
#     if(array_key_exists("data", $_COOKIE)) {
#     $tempdata = json_decode(xor_encrypt(base64_decode($_COOKIE["data"])), true);
#     if(is_array($tempdata) && array_key_exists("showpassword", $tempdata) && array_key_exists("bgcolor", $tempdata)) {
#         if (preg_match('/^#(?:[a-f\d]{6})$/i', $tempdata['bgcolor'])) {
#         $mydata['showpassword'] = $tempdata['showpassword'];
#         $mydata['bgcolor'] = $tempdata['bgcolor'];
#         }
#     }
#     }
#     return $mydata;
# }

# function saveData($d) {
#     setcookie("data", base64_encode(xor_encrypt(json_encode($d))));
# }

# $data = loadData($defaultdata);

# if(array_key_exists("bgcolor",$_REQUEST)) {
#     if (preg_match('/^#(?:[a-f\d]{6})$/i', $_REQUEST['bgcolor'])) {
#         $data['bgcolor'] = $_REQUEST['bgcolor'];
#     }
# }

# saveData($data);

# ?>

# <h1>natas11</h1>
# <div id="content">
# <body style="background: <?=$data['bgcolor']?>;">
# Cookies are protected with XOR encryption<br/><br/>

# <?
# if($data["showpassword"] == "yes") {
#     print "The password for natas12 is <censored><br>";
# }

# ?>

# <form>
# Background color: <input name=bgcolor value="<?=$data['bgcolor']?>">
# <input type=submit value="Set color">
# </form>

# <div id="viewsource"><a href="index-source.html">View sourcecode</a></div>
# </div>
# </body>
# </html>
```

We can see that there is a form which takes our input, and when we submit the input there is a php code which gets executed on the input, Where there are 3 main function, `xor_encrypt` is the function that has a key which is xored with the plaintext that is passed in the parameters. `loadData` is the function which get the value of `data` in cookie as well as the default data, and then this `data` value is decoded with `base64` and then passed to the `xor_encrypt` function and then it finally it is decoded using `json` and copied to `tempdata`. This means that the cookie has the data similar to `defaultdata` but encrypted. Then it checks the `tempdata` and copies the to return data. `savedata` function then sets the cookie by coverting the data to a different format.  Before moving we need to note that xor has special property as shown below.

```bash
X Y XOR
# 0 0 0
# 1 0 1
# 0 1 1
# 1 1 0

# plaintext xor key  = ciphertext
# ciphertext xor key = plaintext
# plaitext xor ciphertext = key 
```
Now, we know that if we xor ciphertext and plaintext we will get the key. Initially when we login we can see that the there will be no cookie and no data value right so what will happen is that the default value is passed to the savedata `function` which then encrypts the defaultdata and is stored int the cookie. We know that it was base64_encoded before setting, so we need to decode it using base64 and then we can do a xor with the plaintext which is the `defaultdata` as this will give us the key with which it was encypted. Once we find the key. We can use the key to encryp the desired data which is given in the next section.

```bash
<?php
	function xor_encrypt_2($in) {
		$key = base64_decode("HmYkBwozJw4WNyAAFyB1VUcqOE1JZjUIBis7ABdmbU1GIjEJAyIxTRg=");
		$text = $in;
		$outText = '';
		for($i=0;$i<strlen($text);$i++) {
			$outText .= $text[$i] ^ $key[$i % strlen($key)];
		}
		return $outText;
	}

	$mydata = array( "showpassword"=>"no", "bgcolor"=>"#ffffff" );
	$mydata_json = json_encode($mydata);
	$mydata_enc = xor_encrypt_2($mydata_json);
	echo $mydata_enc;
?>

# eDWoeDWoeDWoeDWoeDWoeDWoeDWoeDWoeDWoeDWoe
```

After executing the above code in php we can see that we got the key which is `eDWo` which is repeated multiple times. we can now replace the key with the exact value and then encrypt the cookie value that we want.

```bash
<?php
	function xor_encrypt_2($in) {
		$key = "eDWo";
		$text = $in;
		$outText = '';
		for($i=0;$i<strlen($text);$i++) {
			$outText .= $text[$i] ^ $key[$i % strlen($key)];
		}
		return $outText;
	}

	$mydata = array( "showpassword"=>"yes", "bgcolor"=>"#ffffff" );
	$mydata_json = json_encode($mydata);
	$mydata_enc = base64_encode(xor_encrypt_2($mydata_json));
	echo $mydata_enc;
?>

# HmYkBwozJw4WNyAAFyB1VUc9MhxHaHUNAic4Awo2dVVHZzEJAyIxCUc5
```

We got the cookie value that needs to be set. Now let go set the cookie value in `inspect element -> Application - > cookies -> data -> HmYkBwozJw4WNyAAFyB1VUc9MhxHaHUNAic4Awo2dVVHZzEJAyIxCUc5`. Once we have set the cookie let try to put some color code and submit it. we can see that we got the value of the password for the next level as shown below.

```bash
# The password for natas12 is yZdkjAYZRd3R7tq7T5kXMjMJlOIkzDeB
```

## Flag

**Flag:** yZdkjAYZRd3R7tq7T5kXMjMJlOIkzDeB

---

## Lessons Learned

It was a very hard challenge where i was stuck for a very long time, I couldnt solve it without help but still i couldn't grasp everything fully from it. One key idea here to understand it is that try explaining to yourself multiple times. 
