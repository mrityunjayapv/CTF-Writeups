# CTF Writeup - **OverTheWire Natas3 - Natas4**

**Author**: spiketyson 

---

## Challenge Details

- **Challenge Name:** Natas3 - Natas4
- **Category:** Linux
- **Difficulty Level:** Easy
- **Date:** 1-12-2024

---

## Summary

This challenge is the introductory level for the Natas series on OverTheWire, focusing on web exploitation. The goal is to find the flag that is hidden web server using burpsuite.

---

## Approach

Since this is an basic challenge, I planned to use burp suite for this challenge in order to manipulate the request sent to the server.

---

## Solution Walkthrough

Connecting to the web server using:

```bash
Username: natas4
Password: QryZXc2e0zahULdHrtHxzyYkj59kUxLQ
URL:      http://natas4.natas.labs.overthewire.org
```

After logging in, we can see that the webpage says `Access disallowed. You are visiting from "" while authorized users should come only from "http://natas5.natas.labs.overthewire.org/"`.  It is saying that the request for the currect webpage should come from `http://natas5.natas.labs.overthewire.org/` but its comming from "". Before proceeding we need to understand how web pages work. When requesting a webpage from a server, the server responds by sending the webpage's data over the network in the form of packets. A single webpage may be broken into multiple packets for transmission. Each packet contains a basic structure with fields such as the **source** (the origin of the request, typically your IP address), **destination** (the server's IP address), **length** (the size of the packet), **port** (e.g., port 80 for HTTP or 443 for HTTPS), and **request type** (e.g., `GET` or `POST`). These details are added to every request or packet sent over the network. Now, to address the problem of modifying the **source** of a request before it reaches the server, we can use a tool like **Burp Suite**. Start by opening Burp Suite and creating a **Temporary Project**. Navigate to the **Proxy** tab and select the **Intercept** subtab. Use the built-in **Burp Browser**, which allows interception and modification of requests and responses before they reach the server. Turn on the **Intercept** feature, and as you visit a webpage, Burp Suite will capture the HTTP request, we can now edit the `Referer: http://natas5.natas.labs.overthewire.org/` field in the intercepter and then forward the request. We can now see that the server has sent back the response as shown below, 

```bash
# Access granted. The password for natas5 is 0n35PkggAPm2zbEpOU802c0x0Msn1ToK
```

## Flag

**Flag:** 0n35PkggAPm2zbEpOU802c0x0Msn1ToK

---

## Lessons Learned

It was a good challenge where i learned about what burp suite is and how to work with them to capture request and modify it.
