# CTF Writeup - **OverTheWire Krypton1 - Krypton2**

**Author**: spiketyson 

---

## Challenge Details

- **Challenge Name:** Krypton1 - Krypton2
- **Category:** Linux
- **Difficulty Level:** Easy
- **Date:** 27-11-2024

---

## Summary

This challenge is the introductory level1 for the Krypton series on OverTheWire, focusing on basic cryptography skills. The password for level 2 is in the file ‘krypton2’. It is ‘encrypted’ using a simple rotation. It is also in non-standard ciphertext format. When using alpha characters for cipher text it is normal to group the letters into 5 letter clusters, regardless of word boundaries. This helps obfuscate any patterns. This file has kept the plain text word boundaries and carried them to the cipher text. Enjoy!
---

## Approach

Since this is an introductory challenge, I planned to use `tr` for decoding the `ROT13` string as we have seen a similar challenge in bandit series to decode 13, we can use the same here.

---

## Solution Walkthrough

As we have been give and `tr` encoded string lets go to `ROT13` to decode it to get the password of krypton1.

```bash
cd /krypton/krypton1/
ls -la
# total 16
# drwxr-xr-x 2 root     root     4096 Sep 19 07:09 .
# drwxr-xr-x 9 root     root     4096 Sep 19 07:10 ..
# -rw-r----- 1 krypton1 krypton1   26 Sep 19 07:09 krypton2
# -rw-r----- 1 krypton1 krypton1  882 Sep 19 07:09 README
cat README
# Welcome to Krypton!
# This game is intended to give hands on experience with cryptography
# and cryptanalysis.  The levels progress from classic ciphers, to modern,
# easy to harder.
# Although there are excellent public tools, like cryptool,to perform
# the simple analysis, we strongly encourage you to try and do these
# without them for now.  We will use them in later excercises.
# ** Please try these levels without cryptool first **
# The first level is easy.  The password for level 2 is in the file
# 'krypton2'.  It is 'encrypted' using a simple rotation called ROT13.
# It is also in non-standard ciphertext format.  When using alpha characters for
# cipher text it is normal to group the letters into 5 letter clusters,
# regardless of word boundaries.  This helps obfuscate any patterns.
# This file has kept the plain text word boundaries and carried them to
# the cipher text.
# Enjoy!
cat krypton2 | tr 'a-zA-z' 'n-za-mN-ZA-M'
# LEVEL TWO PASSWORD ROTTEN
```

---

## Flag

**Flag:** ROTTEN

---

## Lessons Learned

It was a good refresher for using `tr` to decode instead of using the online tools.
