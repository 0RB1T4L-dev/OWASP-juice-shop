## Vulnerable Library
**Category:** Vulnerable Components

**Difficulty:** 4/5

**Challenge Description:** Inform the shop about a vulnerable library it is using. (Mention the exact library name and version in your comment)

## Approach

### Libraries in use

In order to tell the shop about a vulnerable library, we first need to what libraries are used. Fortunately we already got access to a backup of the **package.json** in [Poison Nullbytes](/poison-nullbytes.md).

### Checking for vulnerable Libraries

Snyk offers a online [package.json health checker](https://snyk.io/advisor/check/npm), so I just uploaded the file and got a report. The report shows multiple libraries, but only one with a critical vulnerability: **marsdb**.

![Snyk Health Report](/images/snyk-marsdb-vulnerable.png)

### Reporting the library

After the finding the critical vulnerable library, the last step is to report the library with the customer feedback function. 

Reporting the mardb library did not solve the challenge, so I decided to search for more vulnerable libraries. When looking through the packa.json file, I noticed, that there are two depedencies with absolute versions: **express-jwt** and **sanitize-html**. Those two libraries also have vulnerabilities in the specified version.

![Vulnerable Libraries Report](/images/vulnerable-libraries.png)