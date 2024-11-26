# CTF Writeup - **OverTheWire Bandit32 - Bandit33**

**Author**: spiketyson  

---

## Challenge Details

- **Challenge Name:** Bandit32 - Bandit33
- **Category:** Linux
- **Difficulty Level:** Easy
- **Date:** 26-11-2024

---

## Summary

This challenge is the level32 for the Bandit series on OverTheWire, focusing on basic Linux command-line skills. After all this git stuff, it’s time for another escape. Good luck!
---

## Approach

Since this is an easy challenge, I planned to use basic Linux commands like `ls`, `sh`, `man` to explore the server and retrieve the flag.

---

## Solution Walkthrough

Connect to the server using the `ssh` command as below as:

```bash
ssh bandit32@bandit.labs.overthewire.org -p 2220 
# Password: 3O9RfhqyAlVBEZpVb6LYStshZoqoSx5K
```

After loggin in, Lets explore the server to see what is there is it as shown below.

```bash
>> ls
# sh: 1: LS: Permission denied
>> ls -ld .
# sh: 1: LS: Permission denied
>> chmod +rx .
# sh: 1: CHMOD: Permission denied
```

It seems like it is converting every command that we are giving in the shell to UPPERCASE. Before proceeding we need to understand how shell works and how it is setup for any user. Linux has Variables called local variables (valid in current shell), shell variables (set up by shell) and environment variables (valid systemwide). The variable name are written in capital letter for example $HOME or $TERM or $USER. Lets try understanding what its trying to do. 

```bash
# whille(true) {
#   print(">> ");
#   cmd = makeUppercase(readInput());
#   print(execute("sh", "-c", cmd));
# }
```

The above code isn't valid in any language, just an example. The shell is trying to execute the above command for every command that we are giving as input. But from the terminal perspective it is trying the to execute the command and print it as shown below. Note here that the first argument for this command is sh so if we want to access this we can do it by using $0 as it is our first argument. Which will execute our first argument and print it. so if our shell is started by executing something similar we can also do the same by inputing `$0` to our current shell.

```bash
$ sh -c 'echo hi'
# hi
$ sh -c 'echo $0'
sh
>> $0
$ ls -la
# total 36
# drwxr-xr-x  2 root     root      4096 Sep 19 07:08 .
# drwxr-xr-x 70 root     root      4096 Sep 19 07:09 ..
# -rw-r--r--  1 root     root       220 Mar 31  2024 .bash_logout
# -rw-r--r--  1 root     root      3771 Mar 31  2024 .bashrc
# -rw-r--r--  1 root     root       807 Mar 31  2024 .profile
# -rwsr-x---  1 bandit33 bandit32 15136 Sep 19 07:08 uppershell
```

Now that we got access to the shell. We can notice the the upper shell was running as `bandit33`. This is interesting for us as it can only be run by `bandit33` user or a program which uses `SUID`. Lets check who is the current user by using `whoami`.

```bash
whoami
# bandit33
```
As we are already `bandit33` we can just get the password from the default location as we have the access to it as shown below.

```bash
cat /etc/bandit_pass/bandit33
# FLAG{tQdtbs5D5i2vJwkO8mEyYEyTL8izoeJ0}
```


---

## Flag

**Flag:**  tQdtbs5D5i2vJwkO8mEyYEyTL8izoeJ0

---

## Lessons Learned
It was a good learning challenge where i understood how the shell works and how the variables are set in the shell and how we can execute commands using system arguments for a program. 