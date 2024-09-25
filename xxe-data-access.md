## XXE Data Access
**Category:** XXE

**Difficulty:** 3/5

**Challenge Description:** Retrieve the content of C:\Windows\system.ini or /etc/passwd from the server.

## Approach

### Finding the right File Upload

Since I could not imagine, that the profile picture file upload could allow me for XXE, I moved on to the invoice file upload in the deprecated B2B interface ([Deprecated Interface](/deprecated-interface.md)).

### Checking for XML Parsing Activities

I first tried to upload a empty XML file and got the following response:

![Empty XML File](/images/complain-empty-xml-file.png)

Altough get a 410 response code, it still looks like, the endpoint is trying to pare the XML file.

### Performing the XXE

The next step was to fill the XML file, with the standard XXE:

```xml
<?xml version="1.0"?>
<!DOCTYPE root [<!ENTITY test SYSTEM 'file:///etc/passwd'>]>
<root>&test;</root>
```

After uploading the file again, I was able to retrieve the content of the /etc/passwd file:

![Complain XXE](/images/complain-xxe.png)

