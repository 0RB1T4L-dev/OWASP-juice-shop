# Starting Point

This file contains some basic steps, which don't relate to a specfic challenge.

# Enumeration

### Finding hidden directories with robots.txt
The *robots.txt* file is used to prevent certain directories from being indexed by search engine crawlers. It is usually a good starting point to find some interesting directories.

```
┌──(orb1t4l㉿elysium)-[~/OWASP/juice-shop]
└─$ curl http://192.168.0.1:3000/robots.txt
User-agent: *
Disallow: /ftp
```

### Web Enumeration with GoBuster
I had absolutely no success with automatic enumeration tools like GoBuster, because for some the reason the Juice Shop did not survive any attempts and always failed with *JavaScript Heap out of memory*.

### FTP Endpoint
The FTP endpoint discovered in the robots.txt file is accessible for everyone and shows some files:

![ftp endpoint](/images/ftp-endpoint.png)

When trying to access a file which is not a MD or a PDF file, we unintentionally produce our first error which is not gracefully handled.

![Error: Only MD and PDF files are allowed](/images/ftp-endpoint-file-restrictions.png)