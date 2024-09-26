## Admin Registration
**Category:** Improper Input Validation

**Difficulty:** 3/6

**Challenge Description:** Register as a user with administrator privileges.

## Approach

### Analyzing the User Registration Process

When you analyze the registration process of a new user in BurpSuite, you can see, that in the response there is a field called *role* with the value *customer*.

![User Registration Response](/images/admin-registration-user-registration.png)

### Setting the Role in the Request

If user creation process is not properly implemented it could be possible, that function also accepts JSON properties, which are not asked in the UI form.

We already know from [User Credentials](/user-credentials.md), that another possible value for role is *admin*.

If we add the JSON property in the request, the response says, that the user was successfully created with the role admin.

![Admin Registration](/images/admin-registration-admin.png)

When we know login with the newly created account we are able to access the admin panel.

![Admin Prove](/images/admin-registration-prove.png)