## Password Strength
**Category:** Broken Authentication

**Difficulty:** 2/5

**Challenge Description:** Log in with the administrator's user credentials without previously changing them or applying SQL Injection.

## Approach

### Prerequisites

The time I did this challenge, I already had the admin credentials from [User Credentials](/user-credentials.md). Therefor I only had to crack the hash and not bruteforce the login form.

### Hash Cracking

The hash of the admin password is **0192023a7bbd73250516f069df18b500**. Time to start HashCat.

The hash looks like a MD5 hash, which is also suggested by HashCat:

```
┌──(orb1t4l㉿elysium)-[~/OWASP/juice-shop/hashes]
└─$ hashcat -a 0 admin_password.txt /usr/share/wordlists/rockyou.txt 
hashcat (v6.2.6) starting in autodetect mode

OpenCL API (OpenCL 3.0 PoCL 6.0+debian  Linux, None+Asserts, RELOC, LLVM 17.0.6, SLEEF, DISTRO, POCL_DEBUG) - Platform #1 [The pocl project]
============================================================================================================================================
* Device #1: cpu-haswell-Intel(R) Core(TM) i5-8600 CPU @ 3.10GHz, 1425/2914 MB (512 MB allocatable), 2MCU

The following 11 hash-modes match the structure of your input hash:
```Shell
     # | Name                                                       | Category
======+============================================================+======================================
   900 | MD4                                                        | Raw Hash
     0 | MD5                                                        | Raw Hash
    70 | md5(utf16le($pass))                                        | Raw Hash
  2600 | md5(md5($pass))                                            | Raw Hash salted and/or iterated
  3500 | md5(md5(md5($pass)))                                       | Raw Hash salted and/or iterated
  4400 | md5(sha1($pass))                                           | Raw Hash salted and/or iterated
 20900 | md5(sha1($pass).md5($pass).sha1($pass))                    | Raw Hash salted and/or iterated
  4300 | md5(strtoupper(md5($pass)))                                | Raw Hash salted and/or iterated
  1000 | NTLM                                                       | Operating System
  9900 | Radmin2                                                    | Operating System
  8600 | Lotus Notes/Domino 5                                       | Enterprise Application Software (EAS)
```

After specifying the hash mode, it took HashCat only under a minute to crack the hash:

```Shell
Watchdog: Temperature abort trigger set to 90c

Host memory required for this attack: 0 MB

Dictionary cache built:
* Filename..: /usr/share/wordlists/rockyou.txt
* Passwords.: 14344391
* Bytes.....: 139921497
* Keyspace..: 14344384
* Runtime...: 2 secs

0192023a7bbd73250516f069df18b500:admin123                 
                                                          
Session..........: hashcat
Status...........: Cracked
Hash.Mode........: 0 (MD5)
Hash.Target......: 0192023a7bbd73250516f069df18b500
Time.Started.....: Fri Sep 13 11:37:44 2024 (1 sec)
Time.Estimated...: Fri Sep 13 11:37:45 2024 (0 secs)
Kernel.Feature...: Pure Kernel
Guess.Base.......: File (/usr/share/wordlists/rockyou.txt)
Guess.Queue......: 1/1 (100.00%)
Speed.#1.........:   986.9 kH/s (0.05ms) @ Accel:256 Loops:1 Thr:1 Vec:8
Recovered........: 1/1 (100.00%) Digests (total), 1/1 (100.00%) Digests (new)
Progress.........: 90112/14344384 (0.63%)
Rejected.........: 0/90112 (0.00%)
Restore.Point....: 89600/14344384 (0.62%)
Restore.Sub.#1...: Salt:0 Amplifier:0-1 Iteration:0-1
Candidate.Engine.: Device Generator
Candidates.#1....: capucino -> KAROLINA
Hardware.Mon.#1..: Util: 49%

Started: Fri Sep 13 11:37:19 2024
Stopped: Fri Sep 13 11:37:46 2024
```

So the password of the administrator is `admin123`.

### Solving another Challenge on the fly

The usage of the MD5 algorithm to store password is definitely not secure and should be reported. There is an extra challenge for this called **Weird Crypto**, where you have to report a algorithm in the customer feedback, that should not be used. 