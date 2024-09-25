## Server-Side XSS Protection
**Category:** XSS

**Difficulty:** 4/6

**Challenge Description:** Perform a persisted XSS attack with \<iframe src="javascript:alert('xss')"\> bypassing a server-side security mechanism.

## Approach

### Find the server-side protected Input

Since I already inspected the search, email and product review input (see [Client-Side XSS Protection](/client-side-xss-protection.md)), where XSS either was possible or wasn't excuted at all, I decided to go for the customer feedback endpoint.

After posting the following feedback:

![Feedback Comment Input](/images/feedback-comment-input.png)

the `iframe` tag gets stripped from the comment and only 'XSS' is displayed on the about endpoint:

![Feedback Stripped Comment](/images/feedback-stripped-comment.png)

### Sanitize HTML Library

Because of [Vulnerable Library](/vulnerable-library.md) we already know, that Juice Shop uses the sanitize-html library in a vulnerable version.

When googling after the exact version, Snyk pops up again with a detailed list of [vulnerabilities](https://security.snyk.io/package/npm/sanitize-html/1.4.2) in this version. Apperantly the sanitization process is not executed recursive.

So when we add a additional script tag before the iframe tag, the script tag gets stripped away and produces a valid iframe tag.

*<\<script\>Stripped\</script\>iframe src="javascript:alert('xss')"> => \<iframe src="javascript:alert('xss')"\>*

![XSS Sanitizer Bypass](/images/feedback-sanitizer-bypass.png)

When we now open the about us page, the persisted XSS gets executed:

![Sucessfull XSS](/images/feedback-successfull-xss.png)