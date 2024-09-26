## Two Factor Authentication
**Category:** Broken Authentication

**Difficulty:** 5/6

**Challenge Description:** Solve the 2FA challenge for user "wurstbrot". (Disabling, bypassing or overwriting his 2FA settings does not count as a solution)

## Approach

### Getting the Password

Since [User Credentials](/user-credentials.md) we fortunately have the complete data of users in the database.

The according entry for the user *wurstbrot* looks like the following:

```json
{
    "id": 10,
    "username": "wurstbrot",
    "email": "wurstbrot@juice-sh.op",
    "password": "9ad5b0492bbe528583e128d2a8941de4",
    "role": "admin",
    "deluxeToken": "",
    "lastLoginIp": "",
    "totpSecret": "IFTXE3SPOEYVURT2MRYGI52TKJ4HC3KH",
    "isActive": 1
}
```

So the first step is to crack the password hash (or so I thought). Neither HashCat nor any online hash databases were successfull with cracking the hash. Since I didn't want to start another (maybe impossible) attempt to crack the hash, I decided stick with another method to login => SQLi (see [Login Admin](/login-admin.md)).

But the really interesting information from user data was the `totpSecret` field, because it wasn't populated anywhere else.

### Researching TOTP Authentication and Google Authenticator

I then started to activate 2FA with my test account, to get some more information.

![2FA Initialisation](/images/2fa-initialisation.png)

It says that, TOTP (Time-based one-time password) is used as the authentication mechanism and suggests Google Authenticator as app.

After looking around in the app, I found out, that you could add an account either per QR code or per setup key. Setup key sounds for me enough like totpSecret to try it out.

### Login as wurstbrot

First I added an account to my authenticator app with the value of the setup key being the totpSecret of wurstbrot. The type of key is time based, since its TOTP.

![Login Wurstbrot](/images/login-wurstbrot.png)

After that the login form prompted me for the 2FA token, where I entered the current value of my authenticator app.

![2FA Prompt](/images/2fa-prompt.png)

With that I was able to successfully login as wurstbrot.