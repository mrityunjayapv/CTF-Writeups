# CTF Writeup - **OverTheWire Bandit15 - Bandit16**

**Author**: spiketyson  

---

## Challenge Details

- **Challenge Name:** Bandit15 - Bandit16
- **Category:** Linux
- **Difficulty Level:** Easy
- **Date:** 18-11-2024

---

## Summary

This challenge is the level15 for the Bandit series on OverTheWire, focusing on basic Linux command-line skills. The password for the next level can be retrieved by submitting the password of the current level to port 30001 on localhost using SSL/TLS encryption.
---

## Approach

Since this is an basic challenge, I planned to use basic Linux commands like `ssh`, `ls`, and `cat` to explore the server and retrieve the flag.

---

## Solution Walkthrough

Connecting to the server using the `ssh` command as below:

```bash
ssh bandit15@bandit.labs.overthewire.org -p 2220
# Password: 8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo
```

After logging in, we know that we have to submit the password of the current level to port 30001 which means the there is a application running at that port and its listenning but note here that we need to use openssl to talkt to the application it it uses ssl/tls for encyption. we can use `openssl` as shown below. 

```bash
openssl s_client -connect localhost:30001
# Output:
# CONNECTED(00000003)
# Can't use SSL_get_servername
# depth=0 CN = SnakeOil
# verify error:num=18:self-signed certificate
# verify return:1
# depth=0 CN = SnakeOil
# verify return:1
# ---
# Certificate chain
#  0 s:CN = SnakeOil
#    i:CN = SnakeOil
#    a:PKEY: rsaEncryption, 4096 (bit); sigalg: RSA-SHA256
#    v:NotBefore: Jun 10 03:59:50 2024 GMT; NotAfter: Jun  8 03:59:50 2034 GMT
# ---
# Server certificate
# -----BEGIN CERTIFICATE-----
# MIIFBzCCAu+gAwIBAgIUBLz7DBxA0IfojaL/WaJzE6Sbz7cwDQYJKoZIhvcNAQEL
# BQAwEzERMA8GA1UEAwwIU25ha2VPaWwwHhcNMjQwNjEwMDM1OTUwWhcNMzQwNjA4
# MDM1OTUwWjATMREwDwYDVQQDDAhTbmFrZU9pbDCCAiIwDQYJKoZIhvcNAQEBBQAD
# ggIPADCCAgoCggIBANI+P5QXm9Bj21FIPsQqbqZRb5XmSZZJYaam7EIJ16Fxedf+
# jXAv4d/FVqiEM4BuSNsNMeBMx2Gq0lAfN33h+RMTjRoMb8yBsZsC063MLfXCk4p+
# 09gtGP7BS6Iy5XdmfY/fPHvA3JDEScdlDDmd6Lsbdwhv93Q8M6POVO9sv4HuS4t/
# jEjr+NhE+Bjr/wDbyg7GL71BP1WPZpQnRE4OzoSrt5+bZVLvODWUFwinB0fLaGRk
# GmI0r5EUOUd7HpYyoIQbiNlePGfPpHRKnmdXTTEZEoxeWWAaM1VhPGqfrB/Pnca+
# vAJX7iBOb3kHinmfVOScsG/YAUR94wSELeY+UlEWJaELVUntrJ5HeRDiTChiVQ++
# wnnjNbepaW6shopybUF3XXfhIb4NvwLWpvoKFXVtcVjlOujF0snVvpE+MRT0wacy
# tHtjZs7Ao7GYxDz6H8AdBLKJW67uQon37a4MI260ADFMS+2vEAbNSFP+f6ii5mrB
# 18cY64ZaF6oU8bjGK7BArDx56bRc3WFyuBIGWAFHEuB948BcshXY7baf5jjzPmgz
# mq1zdRthQB31MOM2ii6vuTkheAvKfFf+llH4M9SnES4NSF2hj9NnHga9V08wfhYc
# x0W6qu+S8HUdVF+V23yTvUNgz4Q+UoGs4sHSDEsIBFqNvInnpUmtNgcR2L5PAgMB
# AAGjUzBRMB0GA1UdDgQWBBTPo8kfze4P9EgxNuyk7+xDGFtAYzAfBgNVHSMEGDAW
# gBTPo8kfze4P9EgxNuyk7+xDGFtAYzAPBgNVHRMBAf8EBTADAQH/MA0GCSqGSIb3
# DQEBCwUAA4ICAQAKHomtmcGqyiLnhziLe97Mq2+Sul5QgYVwfx/KYOXxv2T8ZmcR
# Ae9XFhZT4jsAOUDK1OXx9aZgDGJHJLNEVTe9zWv1ONFfNxEBxQgP7hhmDBWdtj6d
# taqEW/Jp06X+08BtnYK9NZsvDg2YRcvOHConeMjwvEL7tQK0m+GVyQfLYg6jnrhx
# egH+abucTKxabFcWSE+Vk0uJYMqcbXvB4WNKz9vj4V5Hn7/DN4xIjFko+nREw6Oa
# /AUFjNnO/FPjap+d68H1LdzMH3PSs+yjGid+6Zx9FCnt9qZydW13Miqg3nDnODXw
# +Z682mQFjVlGPCA5ZOQbyMKY4tNazG2n8qy2famQT3+jF8Lb6a4NGbnpeWnLMkIu
# jWLWIkA9MlbdNXuajiPNVyYIK9gdoBzbfaKwoOfSsLxEqlf8rio1GGcEV5Hlz5S2
# txwI0xdW9MWeGWoiLbZSbRJH4TIBFFtoBG0LoEJi0C+UPwS8CDngJB4TyrZqEld3
# rH87W+Et1t/Nepoc/Eoaux9PFp5VPXP+qwQGmhir/hv7OsgBhrkYuhkjxZ8+1uk7
# tUWC/XM0mpLoxsq6vVl3AJaJe1ivdA9xLytsuG4iv02Juc593HXYR8yOpow0Eq2T
# U5EyeuFg5RXYwAPi7ykw1PW7zAPL4MlonEVz+QXOSx6eyhimp1VZC11SCg==
# -----END CERTIFICATE-----
# subject=CN = SnakeOil
# issuer=CN = SnakeOil
# ---
# No client certificate CA names sent
# Peer signing digest: SHA256
# Peer signature type: RSA-PSS
# Server Temp Key: X25519, 253 bits
# ---
# SSL handshake has read 2103 bytes and written 373 bytes
# Verification error: self-signed certificate
# ---
# New, TLSv1.3, Cipher is TLS_AES_256_GCM_SHA384
# Server public key is 4096 bit
# Secure Renegotiation IS NOT supported
# Compression: NONE
# Expansion: NONE
# No ALPN negotiated
# Early data was not sent
# Verify return code: 18 (self-signed certificate)
# ---
# ---
# Post-Handshake New Session Ticket arrived:
# SSL-Session:
#     Protocol  : TLSv1.3
#     Cipher    : TLS_AES_256_GCM_SHA384
#     Session-ID: 53DE0003AC2E099443C47EB51782BC6D52A9A41D4230E3E3C91A13D30E43A01A     
#     Session-ID-ctx:
#     Resumption PSK: 0BD60104CB561A1413C3EED93AFE7F2BD7828634EBA48DE09BBC08C27AE3B940D71CA255B83695497D958D8298199EE0
#     PSK identity: None
#     PSK identity hint: None
#     SRP username: None
#     TLS session ticket lifetime hint: 300 (seconds)
#     TLS session ticket:
#     0000 - ad 67 51 83 e2 e6 b7 93-8b 25 99 84 27 67 26 9e   .gQ......%..'g&.        
#     0010 - ee f1 e1 ec 61 bc 76 c4-a4 43 81 7d 71 60 6b 09   ....a.v..C.}q`k.        
#     0020 - 62 fe e9 b1 7f 8d 70 71-24 ee 86 c8 ff 1e 64 24   b.....pq$.....d$        
#     0030 - 29 98 30 8d 6e ae b8 44-37 fa ce ea 51 c1 96 e6   ).0.n..D7...Q...        
#     0040 - c6 2a ac d1 df d1 8f 4e-c1 90 22 24 78 9d 49 35   .*.....N.."$x.I5        
#     0050 - 09 54 7f 05 a0 86 1c 9e-de c1 36 65 53 4b aa bf   .T........6eSK..        
#     0060 - 87 c8 16 8a 8d 7b 5b 5a-f7 dc 2b d4 ad 8f fe 1a   .....{[Z..+.....        
#     0070 - a6 7a f7 20 85 f6 b3 bf-8c d3 ce 9d 00 85 29 71   .z. ..........)q        
#     0080 - 23 97 60 34 9a 36 1a 9f-0e 9d 68 ca 14 a6 cf 8d   #.`4.6....h.....        
#     0090 - 27 37 e9 31 af 0c a2 c9-76 ce 16 e0 ff 4b 65 7a   '7.1....v....Kez        
#     00a0 - f9 69 55 39 0e 0e ea 6a-b3 b2 9e b2 18 78 d5 3a   .iU9...j.....x.:        
#     00b0 - d4 c4 73 b2 d3 37 f8 3d-d8 4b 4b 9d 84 d6 1e d8   ..s..7.=.KK.....        
#     00c0 - e0 60 ed b0 46 e8 5e 4d-ef b6 4b 5b cb cf 9b b7   .`..F.^M..K[....        
#     00d0 - d4 b7 8f 86 80 50 9f dd-4b 91 4a d0 a0 17 75 3d   .....P..K.J...u=        

#     Start Time: 1731943011
#     Timeout   : 7200 (sec)
#     Verify return code: 18 (self-signed certificate)
#     Extended master secret: no
#     Max Early Data: 0
# ---
# read R BLOCK
# ---
# Post-Handshake New Session Ticket arrived:
# SSL-Session:
#     Protocol  : TLSv1.3
#     Cipher    : TLS_AES_256_GCM_SHA384
#     Session-ID: AC8A38E7A0D3C88D63DBB1FCC21CF2750E9F241717AD1661654865D685F4BDCC     
#     Session-ID-ctx:
#     Resumption PSK: CBF394A4B4C067F48504745C8F961253EC61E39D5BA858740B0B886E0885E536D9C8E9CCC5FBCEEEA4F67B3A1E07C6A7
#     PSK identity: None
#     PSK identity hint: None
#     SRP username: None
#     TLS session ticket lifetime hint: 300 (seconds)
#     TLS session ticket:
#     0000 - ad 67 51 83 e2 e6 b7 93-8b 25 99 84 27 67 26 9e   .gQ......%..'g&.        
#     0010 - ab 4d 69 30 28 fa 3a 6a-08 09 77 d4 4d 9c 04 23   .Mi0(.:j..w.M..#        
#     0020 - ea b3 4e 44 6d e8 27 53-6e c8 19 8d eb d7 aa 6e   ..NDm.'Sn......n        
#     0030 - de 06 99 e6 df 09 02 fb-aa dc 72 98 ed 10 64 84   ..........r...d.        
#     0040 - c2 2d 0b 1b fe b2 2d 62-f5 f1 ae 24 7b 96 a2 fe   .-....-b...${...        
#     0050 - 89 1b 1c 00 a2 24 79 93-85 77 94 d1 07 be a9 b4   .....$y..w......        
#     0060 - 3f ee f2 70 f8 d4 69 2d-65 73 ea 87 aa 3a ed 7b   ?..p..i-es...:.{        
#     0070 - f9 7e 60 a0 e6 8c 7d 5a-f3 e7 5b d2 e4 18 4a c0   .~`...}Z..[...J.        
#     0080 - 33 f4 11 0d 61 b6 36 70-6c 44 a9 8e 9a 0a 78 93   3...a.6plD....x.        
#     0090 - 49 73 41 09 31 9d 9a a3-ba e6 d6 2f 27 50 12 5a   IsA.1....../'P.Z        
#     00a0 - cf 5b 53 70 07 3c 8b 6b-d2 b2 d2 b5 5c 8d e4 10   .[Sp.<.k....\...        
#     00b0 - d0 be a7 86 0b 19 73 36-71 d1 a8 79 56 ee eb 7f   ......s6q..yV...        
#     00c0 - 33 75 a5 d1 a6 23 db 22-8e 52 e3 b2 f0 b1 db 1b   3u...#.".R......        
#     00d0 - b1 76 45 5b 21 8a d1 2d-11 bf 07 d5 88 5e 7f 6f   .vE[!..-.....^.o        

#     Start Time: 1731943011
#     Timeout   : 7200 (sec)
#     Verify return code: 18 (self-signed certificate)
#     Extended master secret: no
#     Max Early Data: 0
# ---
# read R BLOCK

```

Now It clearly shows a lot of data. But if we remember correctly we need to feed the password of the current level as an input to get the flag. so all the data here is garbage. we need to scroll down and we can see that the command has not completed the execution completely and its waiting for a password. Let just paste the password of the current level as shown below.

```bash
8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo
# Output: FLAG{kSkvUpMQ7lBYyCM4GBPvCvT1BfWRy0Dx}
```
---

## Flag

**Flag:** kSkvUpMQ7lBYyCM4GBPvCvT1BfWRy0Dx

---

## Lessons Learned

It was a easier challange, the first trick was to find the command to use here. But with the clue we can easily figure out that we need to use openssl. the second challange was to learning to use openssl as it has a huge number of options. But the main idead is to send data using tls/ssl in openssl to a server which is the localhost and a specific port.