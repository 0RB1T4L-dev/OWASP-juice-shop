## Client-Side XSS Protection
**Category:** XSS

**Difficulty:** 3/6

**Challenge Description:** Perform a persisted XSS attack with \<iframe src="javascript:alert('xss')"\> bypassing a client-side security mechanism.

## Approach

### Finding the right input field

I spent some time playing around with the input of the reviews of the products, but wasn't successfull. I then moved on to the second thing, which is visible in a review => the email.

### Creating a XSS User

In order to bypass the client-side restrictions, we have to use BurpSuite again to create this user.

![Persisted XSS in Email](/images/xss-in-email.png)

When the administrator opens the administration pnael, the XSS is executed.

![XSS in Admin Panel](/images/persisted-xss-admin-panel.png)

For some reason, the persisted XSS does not work in the review section of a product.

![Failed XSS in Review Section](/images/persisted-xss-in-review-not-working.png)