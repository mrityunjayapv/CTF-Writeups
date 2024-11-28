# CTF Writeup - **OverTheWire Krypton5 - Krypton6**

**Author**: spiketyson 

---

## Challenge Details

- **Challenge Name:** Krypton5 - Krypton6
- **Category:** Linux
- **Difficulty Level:** Easy
- **Date:** 28-11-2024

---

## Summary

This challenge is the introductory level5 for the Krypton series on OverTheWire, focusing on basic cryptography skills.  This level is a Vigenère Cipher. You have intercepted two longer, english language messages (American English). You also have a key piece of information. You know the key length! For this exercise, the key length is 6. The password to level five is in the usual place, encrypted with the 6 letter key.

---

## Approach

Since this is an introductory challenge, I planned to use `dcode.fr` for decoding the cipher string.
---

## Solution Walkthrough

Lets log into the server as `krypton5` to have the necessary permissions. 

```bash
ssh krypton5@krypton.labs.overthewire.org -p 2231
# Password: CLEARTEXT
```

After logging in, lets move to the `/krypton/krypton5` directory and explore it.

```bash
cd /krypton/krypton5/
ls -la
# total 28
# drwxr-xr-x 2 root     root     4096 Sep 19 07:10 .
# drwxr-xr-x 9 root     root     4096 Sep 19 07:10 ..
# -rw-r----- 1 krypton4 krypton4 1740 Sep 19 07:10 found1
# -rw-r----- 1 krypton4 krypton4 2943 Sep 19 07:10 found2
# -rw-r----- 1 krypton4 krypton4  287 Sep 19 07:10 HINT
# -rw-r----- 1 krypton4 krypton4   10 Sep 19 07:10 krypton6
# -rw-r----- 1 krypton4 krypton4 1385 Sep 19 07:10 README
```

Here we have 5 files. `krypton5` has the password for the next level. `found1`,`found2` are the files which has the cipher text which was encrypted with the same encryption mechanism. `HINT1` files gives you a clue to do a character frequency of the `found` files. Since Its vigenere cipher, It would be tedious to do frequency analysis and then fing the key lenght using kasaki's test. But here we know the key length is 6 and there is 2 files with encrypted data. Lets go to `dcode.fr/vigenere-cipher` and then lets try decrypting the `found1` file

```bash
# CIPHERTEXT = SXULW GNXIO WRZJG OFLCM RHEFZ ALGSP DXBLM PWIQT XJGLA RIYRI BLPPC HMXMG CTZDL CLKRU YMYSJ TWUTX ZCMRH EFZAL OTMNL BLULV MCQMG CTZDL CPTBI AVPML NVRJN SSXWT XJGLA RIQPE FUGVP PGRLG OMDKW RSIFK TZYRM QHNXD UOWQT XJGLA RIQAV VTZVP LMAIV ZPHCX FPAVT MLBSD OIFVT PBACS EQKOL BCRSM AMULP SPPYF CXOKH LZXUO GNLID ZVRAL DOACC INREN YMLRH VXXJD XMSIN BXUGI UPVRG ESQSG YKQOK LMXRS IBZAL BAYJM AYAVB XRSIC KKPYH ULWFU YHBPG VIGNX WBIQP RGVXY SSBEL NZLVW IMQMG YGVSW GPWGG NARSP TXVKL PXWGD XRJHU SXQMI VTZYO GCTZR JYVBK MZHBX YVBIT TPVTM OOWSA IERTA SZCOI TXXLY JAZQC GKPCS LZRYE MOOVC HIEKT RSREH MGNTS KVEPN NCTUN EOFIR TPPDL YAPNO GMKGC ZRGNX ARVMY IBLXU QPYYH GNXYO ACCIN QBUQA GELNR TYQIH LANTW HAYCP RJOMO KJYTV SGVLY RRSIG NKVXI MQJEG GJOML MSGNV VERRC MRYBA GEQNP RGKLB XFLRP XRZDE JESGN XSYVB DSSZA LCXYE ICXXZ OVTPW BLEVK ZCDEA JYPCL CDXUG MARML RWVTZ LXIPL PJKKL CIREP RJYVB ITPVV ZPHCX FPCRG KVPSS CPBXW VXIRS SHYTU NWCGI ANNUN VCOEA JLLFI LECSO OLCTG CMGAT SBITP PNZBV XWUPV RIHUM IBPHG UXUQP YYHNZ MOKXD LZBAK LNTCC MBJTZ KXRSM FSKZC SSELP UMARE BCIPK GAVCY EXNOG LNLCC JVBXH XHRHI AZBLD LZWIF YXKLM PELQG RVPAF ZQNVK VZLCE MPVKP FERPM AZALV MDPKH GKKCL YOLRX TSNIB ELRYN IVMKP ECVXH BELNI OETUX SSYGV TZARE RLVEG GNOQC YXFCX YOQYO ISUKA RIQHE YRHDS REFTB LEVXH MYEAJ PLCXK TRFZX YOZCY XUKVV MOJLR RMAVC XFLHO KXUVE GOSAR RHBSS YHQUS LXSDJ INXLH PXCCV NVIPX KMFXV ZLTOW QLKRY TZDLC DTVXB ACSDE LVYOL BCWPE ERTZD TYDXF AILBR YEYEG ESIHC QMPOX UDMLZ VVMBU KPGEC EGIWO HMFXG NXPBW KPVRS XZCEE PWVTM OOIYC XURRV BHCCS SKOLX XQSEQ RTAOP WNSZK MVDLC PRTRB ZRGPZ AAGGK ZIMAP RLKVW EAZRT XXZCS DMVVZ BZRWS MNRIM ZSRYX IEOVH GLGNL FZKHX KCESE KEHDI FLZRV KVFIB XSEKB TZSPE EAZMV DLCSY ZGGYK GCELN TTUIG MXQHT BJKXG ZRFEX ABIAP MIKWA RVMFK UGGFY JRSIP NBJUI LDSSZ ALMSA VPNTX IBSMO
# Key length = Unknown
# --> Decode
# Key = KEYLENGTH
# PLAINTEXT = ITWASTHEBESTOFTIMESITWASTHEWORSTOFTIMESITWASTHEAGEOFWISDOMITWASTHEAGEOFFOOLISHNESSITWASTHEEPOCHOFBELIEFITWASTHEEPOCHOFINCREDULITYITWASTHESEASONOFLIGHTITWASTHESEASONOFDARKNESSITWASTHESPRINGOFHOPEITWASTHEWINTEROFDESPAIRWEHADEVERYTHINGBEFOREUSWEHADNOTHINGBEFOREUSWEWEREALLGOINGDIRECTTOHEAVENWEWEREALLGOINGDIRECTTHEOTHERWAYINSHORTTHEPERIODWASSOFARLIKETHEPRESENTPERIODTHATSOMEOFITSNOISIESTAUTHORITIESINSISTEDONITSBEINGRECEIVEDFORGOODORFOREVILINTHESUPERLATIVEDEGREEOFCOMPARISONONLYTHEREWEREAKINGWITHALARGEJAWANDAQUEENWITHAPLAINFACEONTHETHRONEOFENGLANDTHEREWEREAKINGWITHALARGEJAWANDAQUEENWITHAFAIRFACEONTHETHRONEOFFRANCEINBOTHCOUNTRIESITWASCLEARERTHANCRYSTALTOTHELORDSOFTHESTATEPRESERVESOFLOAVESANDFISHESTHATTHINGSINGENERALWERESETTLEDFOREVERITWASTHEYEAROFOURLORDONETHOUSANDSEVENHUNDREDANDSEVENTYFIVESPIRITUALREVELATIONSWERECONCEDEDTOENGLANDATTHATFAVOUREDPERIODASATTHISMRSSOUTHCOTTHADRECENTLYATTAINEDHERFIVEANDTWENTIETHBLESSEDBIRTHDAYOFWHOMAPROPHETICPRIVATEINTHELIFEGUARDSHADHERALDEDTHESUBLIMEAPPEARANCEBYANN
```

Now that we have found the key `KEYLENGHT` we can use it to decrypt the `krypton6` password.

```bash
# CIPHERTEXT = KEYLENGTH
# Key Length = 9
# Key = KEYLENGHT
# PLAINTEXT = RANDOM
```

---

## Flag

**Flag:** RANDOM

---

## Lessons Learned

It was a good learning for using `dcode.fr` to decode monoalphabetic substitution cipher.