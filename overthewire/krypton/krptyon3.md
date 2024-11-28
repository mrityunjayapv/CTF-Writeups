# CTF Writeup - **OverTheWire Krypton3 - Krypton4**

**Author**: spiketyson 

---

## Challenge Details

- **Challenge Name:** Krypton3 - Krypton4
- **Category:** Linux
- **Difficulty Level:** Easy
- **Date:** 28-11-2024

---

## Summary

This challenge is the introductory level3 for the Krypton series on OverTheWire, focusing on basic cryptography skills.  In this example, the cipher mechanism is not available to you, the attacker. However, you have been lucky. You have intercepted more than one message. The password to the next level is found in the file ‘krypton4’. You have also found 3 other files. (found1, found2, found3)

---

## Approach

Since this is an introductory challenge, I planned to use `tr` for decoding the cipher string.
---

## Solution Walkthrough

Lets log into the server as `krypton3` to have the necessary permissions. 

```bash
ssh krypton3@krypton.labs.overthewire.org -p 2231
# Password: CAESARISEASY
```

After logging in, lets move to the `/krypton/krypton3` directory and explore it.

```bash
cd /krypton/krypton3/
ls -la
# total 36
# drwxr-xr-x 2 root     root     4096 Sep 19 07:09 .
# drwxr-xr-x 9 root     root     4096 Sep 19 07:10 ..
# -rw-r----- 1 krypton3 krypton3 1542 Sep 19 07:09 found1
# -rw-r----- 1 krypton3 krypton3 2128 Sep 19 07:09 found2
# -rw-r----- 1 krypton3 krypton3  560 Sep 19 07:09 found3
# -rw-r----- 1 krypton3 krypton3   56 Sep 19 07:09 HINT1
# -rw-r----- 1 krypton3 krypton3   37 Sep 19 07:09 HINT2
# -rw-r----- 1 krypton3 krypton3   42 Sep 19 07:09 krypton4
# -rw-r----- 1 krypton3 krypton3  785 Sep 19 07:09 README
```

Here we have 7 files. `krypton4` has the password for the next level. `found1`,`found2`,`found3` are the files which has the cipher text which was encrypted with the same encryption mechanism. `HINT1`, `HINT2` files gives you a clue to do a character frequency of the `found` files. Lets try to do the character frequency analysis as shown below.

```bash
cat found1 found2 found3 | grep -o . | sort | uniq -c | sort -nr
#     704  
#     456 S
#     340 Q
#     301 J
#     257 U
#     246 B
#     240 N
#     227 G
#     227 C
#     210 D
#     132 Z
#     130 V
#     129 W
#      86 M
#      84 Y
#      75 T
#      71 X
#      67 K
#      64 E
#      60 L
#      55 A
#      28 F
#      19 I
#      12 O
#       4 R
#       4 H
#       2 P
```

Here we are trying to open the display the content of the files using `cat` command. Then we are trying to pass this data as an input to `grep -o` which displays each character in a new line. Then we are trying to pass this are an input to `sort` which sorts the whole data alphabetically. Then we are trying to get the unique values in the data using `uniq -c` where `-c` counts the number of occurences in the text. Finally we are sorting the whole data based on the frequency of data. 

Before moving further we need to understand the concept behind the frequency analysis. We know that english language has words defined, which has a structure and meaning etc. For any plaintext will certainly have some meaning and certain words. If we do character analysis on multiple such plaintext we can see the there are a few characters that appear higher number of times than some of them, For example, vowels are characters without which we cannot form meaningful words or sentences in most cases and Its fair to say that vowels will appear in english text than characters like `z`.  The below table is character frequency of english letter. 

```bash
# CH  Freq
# E  12.70
# T  9.06
# A  8.17
# O  7.51
# I  6.97
# N  6.75
# S  6.33
# H  6.09
# R  5.99
# D  4.25
# L  4.03
# C  2.78
# U  2.76
# M  2.41
# W  2.36
# F  2.23
# G  2.02
# Y  1.97
# P  1.93
# B  1.49
# V  0.98
# K  0.77
# J  0.15
# X  0.15
# Q  0.10
# Z  0.07
```

Since we have both the frequency analysis. We can directly map the character with the highest frequency in cipher text to the character with highest frequency in english. This is purely logical to map the characters based on frequency. Now we can use the `tr command` to decrypt the `krypton4` as we know the mapping of charactes in plaintext to ciphertext.

```bash

cat krypton4 | tr 'SQJUBNGCDZVWMYTXKELAFIORHP' 'ETAOINSHRDLCUMWFGYPBVKJXQZ'
# GELLC ISEAR ELEKE LFIUN MTOOG INCHO BNUAE
```
We can see that we are not getting the right password or any meaning of the plain text. We need to note that its not always the `E` is the highest frequency in a sentence. For example, `I AM MARTIN` Here `E` does not appear at all. So, Lets try mapping the the characters slightly different to make some sense.

```bash
cat krypton4 | tr 'SQJUBNGCDZVWMYTXKELAFIORHP' 'EATSORNIHCLDUPYFWGMBKVXQJZ'
# WELLD ONETH ELEVE LFOUR PASSW ORDIS BRUTE
```

---

## Flag

**Flag:** BRUTE

---

## Lessons Learned

It was a good learning for using `tr` to decode monoalphabetic substitution cipher.
