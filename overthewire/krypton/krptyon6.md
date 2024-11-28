# CTF Writeup - **OverTheWire Krypton6 - Krypton7**

**Author**: spiketyson 

---

## Challenge Details

- **Challenge Name:** Krypton6 - Krypton7
- **Category:** Linux
- **Difficulty Level:** Easy
- **Date:** 28-11-2024

---

## Summary

This challenge is the introductory level5 for the Krypton series on OverTheWire, focusing on basic cryptography skills.  In this example, the keyfile is in your directory, however it is not readable by you. The binary ‘encrypt6’ is also available. It will read the keyfile and encrypt any message you desire, using the key AND a ‘random’ number. You get to perform a ‘known ciphertext’ attack by introducing plaintext of your choice. The challenge here is not simple, but the ‘random’ number generator is weak.As stated, it is now that we suggest you begin to use public tools, like cryptool, to help in your analysis. You will most likely need a hint to get going. See ‘HINT1’ if you need a kicktstart. If you have further difficulty, there is a hint in ‘HINT2’. The password for level 7 (krypton7) is encrypted with ‘encrypt6’

---

## Approach

Since this is an introductory challenge, I planned to use `python` and `linux` cli commands for decoding the cipher string.
---

## Solution Walkthrough

Lets log into the server as `krypton6` to have the necessary permissions. 

```bash
ssh krypton6@krypton.labs.overthewire.org -p 2231
# Password: RANDOM
```

After logging in, lets move to the `/krypton/krypton6` directory and explore it.

```bash
cd /krypton/krypton6/
ls -la
# total 56
# drwxr-xr-x 3 root     root      4096 Sep 19 07:10 .
# drwxr-xr-x 9 root     root      4096 Sep 19 07:10 ..
# -rwsr-x--- 1 krypton7 krypton6 16520 Sep 19 07:10 encrypt6
# -rw-r----- 1 krypton6 krypton6   164 Sep 19 07:10 HINT1
# -rw-r----- 1 krypton6 krypton6    11 Sep 19 07:10 HINT2
# -rw-r----- 1 krypton7 krypton7    11 Sep 19 07:10 keyfile.dat
# -rw-r----- 1 krypton6 krypton6    15 Sep 19 07:10 krypton7
# drwxr-xr-x 2 root     root      4096 Sep 19 07:10 onetime
# -rw-r----- 1 krypton6 krypton6  4342 Sep 19 07:10 README
```

Here we have 5 files. `krypton7` has the password for the next level. `encrypt6` is the file which has the encryption mechanism. `HINT1`,`HINT2` files gives you a clues for the challenge. Since this challenge requires us to create file to work with lets do some operations before starting.
```bash
mktemp -d 
# /tmp/tmp.NCG7PNCmLW
cd /tmp/tmp.NCG7PNCmLW
ln -s /krypton/krypton6/encrypt6
ln -s /krypton/krypton6/keyfile.data
echo "AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA" > aaa
./encrypt aaa outa
cat outa
# EICTDGYIYZKTHNSIRFXYCPFUEOCKRNEICTDGYIYZKTHNSIRFXYCPF
echo "BBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBB" > bbb
./encrypt bbb outb
cat outb
# FJDUEHZJZALUIOTJSGYZDQGVFPDLSOFJDUEHZ
```

Now that we can see that `EICTDGYIYZKTHNSIRFXYCPFUEOCKRN` and `FJDUEHZJZALUIOTJSGYZDQGVFPDLSO` is gettting repeated, The period here is 30. Lets find the alphanumeric difference between characters 

```bash
$python
>>> [ord(b) - ord(a) for a,b in zip('BBBBBBBBBBBBBBBBBBBBBBBBBBBBBB', 'FJDUEHZJZALUIOTJSGYZDQGVFPDLSO')]
# [4, 8, 2, 19, 3, 6, 24, 8, 24, -1, 10, 19, 7, 13, 18, 8, 17, 5, 23, 24, 2, 15, 5, 20, 4, 14, 2, 10, 17, 13]
>>> ''.join([chr(ord(b) - a) for a,b in zip([4, 8, 2, 19, 3, 6, 24, 8, 24, -1, 10, 19, 7, 13, 18], 'PNUKLYLWRQKGKBE')])
# 'LFS8IS4O:RA4D53'
```

Now that we have decrypted our password its still not in human readable format right. If we look into the `HINT2` we can see that we need to use a 8 bit Linear bit feedback register. The characters in the output that look out of place were all shifted by more than 12 originally. The -1 in the shifts for B reveal there is a wraparound happening that isn't being fully seen by using just the first few characters of the alphabet as plaintext. Making the higher shifts signed by subtracting them from 26 is indeed the solution:

```bash
>>> ''.join([chr(ord(b) - a) for a,b in zip([4, 8, 2, -7, 3, 6, -2, 8, -2, -1, 10, -7, 7, -13, -8], 'PNUKLYLWRQKGKBE')])
# 'LFSRISNOTRANDOM'
ssh krypton7@krypton.labs.overthewire.org -p 2231
# Password: LFSRISNOTRANDOM
cd /krypton/krypton7/
cat README
# Congratulations on beating Krypton!
```

---

## Flag

**Flag:** LFSRISNOTRANDOM

---

## Lessons Learned

It was a good learning series for using cryptographic tools to decode the ciphers. This was the last challenge of this series.
