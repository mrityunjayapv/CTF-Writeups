# CTF Writeup - **OverTheWire Krypton2 - Krypton3**

**Author**: spiketyson 

---

## Challenge Details

- **Challenge Name:** Krypton3 - Krypton3
- **Category:** Linux
- **Difficulty Level:** Easy
- **Date:** 27-11-2024

---

## Summary

This challenge is the introductory level2 for the Krypton series on OverTheWire, focusing on basic cryptography skills. The password for level 3 is in the file krypton3. It is in 5 letter group ciphertext. It is encrypted with a Caesar Cipher. Without any further information, this cipher text may be difficult to break. You do not have direct access to the key, however you do have access to a program that will encrypt anything you wish to give it using the key. If you think logically, this is completely easy.
---

## Approach

Since this is an introductory challenge, I planned to use `tr` for decoding the `CEASAR` cipher string.
---

## Solution Walkthrough

Lets log into the server as `krypton2` to have the necessary permissions. 

```bash
ssh krypton2@krypton.labs.overthewire.org -p 2231
# Password: ROTTEN
```

After logging in, lets move to the `/krypton/krypton2` directory and explore it.
```bash
cd /krypton/krypton2/
ls -la
# total 36
# drwxr-xr-x 2 root     root      4096 Sep 19 07:09 .
# drwxr-xr-x 9 root     root      4096 Sep 19 07:10 ..
# -rwsr-x--- 1 krypton3 krypton2 16328 Sep 19 07:09 encrypt
# -rw-r----- 1 krypton3 krypton3    27 Sep 19 07:09 keyfile.dat
# -rw-r----- 1 krypton2 krypton2    13 Sep 19 07:09 krypton3
# -rw-r----- 1 krypton2 krypton2  1815 Sep 19 07:09 README
```

Here we have 4 files. `krypton3` has the password for the next level. `keyfile.data` has the key with which it encrypts the data. `encyrpt` file is the one that encrypts the data when we give an input. Lets try to encrypt
`ABC` and see what happens but before that we need to create a temporary directory and do all these operations.

```bash
mktemp -d
# /tmp/tmp.fNV7uxzOfA
mv /krypton/krypton2/encrypt .
# mv: cannot move '/krypton/krypton2/encrypt' to './encrypt': Permission denied
ln -s /krypton/krypton2/keyfile.dat
ln -s /krypton/krypton2/encrypt
ls -la
# total 13588
# drwxrwxrwx 2 krypton1 krypton1     4096 Nov 27 12:11 .
# drwxrwx-wt 1 root     root     13897728 Nov 27 12:26 ..
# lrwxrwxrwx 1 krypton1 krypton1       25 Nov 27 12:07 encrypt -> /krypton/krypton2/encrypt
# lrwxrwxrwx 1 krypton1 krypton1       29 Nov 27 12:05 keyfile.dat -> /krypton/krypton2/keyfile.dat
```

Since we cannot move the files into temporary directory. Lets create a softlink for the file which points to the exact file as given in the clues and then lets try to encrypt it with the `encrypt` file.

```bash
echo "ABC" > encrypt.txt
./ecrypt encrypt.txt
ls -la
# ciphertext
cat ciphertext
# MNO
```

The above commands try to create a `encrypt.txt` with `ABC` as data and then encrypts the file. Once we have encrypted. We can see a new file called `ciphertext` in our temporary directory. If we see that contents of the file, we can see that `ABC` is encrypted as `MNO`. This is a classic caesar cipher as `A` is pushed by 12 steps to denoted `M` and `B` is pushed 12 steps to be denoted `N`. In Alphabets, we know that there is only 26 alphabets, and we have rotated by 12 to get the ciphertext. In order to get the original text back we need to rotate `26-12=14` so that we can get the original text. This is `ROT14` cipher, we can decrypt it using `tr` as shown below. 

```bash
cat /krypton/krypton2/krypton3 | tr 'a-zA-Z' 'o-za-nO-ZA-N'
# CAESARISEASY
```

---

## Flag

**Flag:** CAESARISEASY

---

## Lessons Learned

It was a good refresher for using `tr` to decode instead of using the online tools.
