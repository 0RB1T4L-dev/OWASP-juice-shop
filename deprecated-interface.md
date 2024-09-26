## Deprecated Interface
**Category:** Security Misconfiguration

**Difficulty:** 2/6

**Challenge Description:** Use a deprecated B2B interface that was not properly shut down.

## Approach

### Pure Luck

While enumerating juice-shop (more precisely file uploads), i stumbled upon the `/complain` endpoint, where you are able to upload your invoice. I have honestly no idea why this should be identified as a deprecated interface (mayber because there is already customer feed back endpoint?).

![Complain Endpoint](/images/complain-endpoint.png)