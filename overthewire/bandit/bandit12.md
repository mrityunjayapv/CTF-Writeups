# CTF Writeup - **OverTheWire Bandit12 - Bandit13**

**Author**: spiketyson  

---

## Challenge Details

- **Challenge Name:** Bandit12 - Bandit13
- **Category:** Linux
- **Difficulty Level:** Easy
- **Date:** 17-11-2024

---

## Summary

This challenge is the level12 for the Bandit series on OverTheWire, focusing on basic Linux command-line skills. The goal is retrive the flag from a file which is a hexdump of a file that has been repeatedly compressed.
---

## Approach

Since this is an basic challenge, I planned to use basic Linux commands like `ssh`, `ls`,`tr`, and `cat` to explore the server and retrieve the flag.

---

## Solution Walkthrough

Connecting to the server using the `ssh` command as below:

```bash
ssh bandit11@bandit.labs.overthewire.org -p 2220
# Password: dtR173fZKb0RRsDFSGsg2RWnpNVj3qRr
```

After logging in, We can use the `ls` command to list the files in the directory. We can see a file named `data.txt`. We can use the `cat` command to see the contents. 

```bash
ls -la
# Output: data.txt
cat data.txt
# 00000000: 1f8b 0808 dfcd eb66 0203 6461 7461 322e  .......f..data2.
# 00000010: 6269 6e00 013e 02c1 fd42 5a68 3931 4159  bin..>...BZh91AY
```
We need to do all file processing in temporary directory. we can use the `mktemp` to create it and then copy the file to the directory. 

```bash
mktemp -d
# /tmp/tmp.kHEU9OHGED
cd /tmp/tmp.kHEU9OHGED
cp ~/data.txt hexdump.txt
```
Now the above data is a hexdump of a file as given in the clue so we need to reverse it to binary. we can use `xxd` command to get the original file as shown below. Once we have reversed it, we can use the file to check the file type using `file` command. It shows the its a `gzip` file so we need to change the format to `.gz` and then uncompress it using `gzip` as shown below. 

```bash
xxd -r hexdump.txt hexdump
file hexdump
# hexdump: gzip compressed data
mv hexdump hexdump.gz
gzip -d hexdump.gz
ls
# hexdump
```

Now we can check the file format after the unzipping. we can use `mv` command to move the file to it respective `.bz2` as it bzip2 compressed data and then uncompress the file using `bzip2` as shown below.

```bash
file hexdump
# hexdump: bzip2 compressed data
mv hexdump hexdump.bz2
bzip2 -d hexdump.bz2
ls
# hexdump
```

Now we can check the file format after the unzipping. we can use `mv` command to move the file to it respective `.gz` as it gzip compressed data and then uncompress the file using `gzip` as shown below.

```bash
file hexdump
# hexdump: gzip compressed data
mv hexdump hexdump.gz
gzip -d hexdump.gz
ls
# hexdump
```
Now we can check the file format after the unzipping. we can use `mv` command to move the file to it respective `.tar` as it is tar archive data and then uncompress the file using `tar` as shown below.

```bash
file hexdump
# hexdump: POSIX tar archive (GNU)
mv hexdump hexdump.tar
tar -xvf hexdump.tar
# data5.bin
```

Now we can check the file format after the unzipping. we can use `mv` command to move the file to it respective `.tar` as it is tar archive data and then uncompress the file using `tar` as shown below.

```bash
file data5.bin
# data5.bin: POSIX tar archive (GNU)
mv data5.bin data5.tar
tar -xvf data5.tar
# data6.bin
```

Now we can check the file format after the unzipping. we can use `mv` command to move the file to it respective `.bz2` as it is bzip2 compressed data and then uncompress the file using `bzip2` as shown below.

```bash
file data6.bin
# data6.bin: bzip2 compressed data
mv data6.bin data6.bz2
bzip2 -d data6.bz2
ls
# data6
```

Now we can check the file format after the unzipping. we can use `mv` command to move the file to it respective `.tar` as it is tar archive data and then uncompress the file using `tar` as shown below.

```bash
file data6 
# data6: POSIX tar archive (GNU)
mv data6 data6.tar
tar -xvf data6.tar
# data8.bin
```

Now we can check the file format after the unzipping. we can use `mv` command to move the file to it respective `.gz` as it gzip compressed data and then uncompress the file using `gzip` as shown below.

```bash
file data8.bin
# data8.bin: gzip compressed data
mv data8.bin data8.gz
gzip -d data8.gz
ls
# data8
```

Finally we got a file which we can check the file type using `file` command. It shows as `ASCII` this is what we have been searching for. we can now read the data of the file by using `cat` command as shown below.

```bash
file data8
# data8: ASCII text
cat data8
# The password is FO5dwFsc0cbaIiH0h8J2eUks2vdTDwAn
```
---

## Flag

**Flag:** FO5dwFsc0cbaIiH0h8J2eUks2vdTDwAn

---

## Lessons Learned

It was a long challange, The struggle was to find a way to reverse the hexdump once i got the idea of how to use `xxd` then it was easier to figure out the other steps to find the file type `file` command and then convert to file to that particular extension and uncompress it using `tar`, `bzip2`,`gz`.