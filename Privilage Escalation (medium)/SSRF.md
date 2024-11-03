# WHERE to USE SSRF?

`Referer`, `Host`, `X-Forwarded-For`, `X-Forwarded-Host`, `X-Original-URL`, `X-Rewrite-URL`, 
`<img src="http://example.com/image.jpg">`, all inputs, parameters.

## SECRET PARAMETER:
```
GET /fetch?url=http://example.com/admin
```

## SSRF mostly with "Check stock"
In this lab they state the admin interface is at `http://192.168.0.12:8080/admin` but in the Burp exam use the `localhost:6566`.

## Basic SSRF against the local server

stockApi parameter to http://localhost/admin

## Basic SSRF against another back-end system

1) stockApi parameter to http://192.168.0.1:8080/admin.
2) Add §. Payloads , change the payload type to Numbers, and enter 1, 255. Brute ip. Receive 200.

## Blind SSRF with out-of-band detection

1) Visit a product, intercept the request in Burp Suite.
2) Referer header, right-click and select "Insert Collaborator Payload" to replace the original domain.

## SSRF with blacklist-based input filter

1) `stockApi` parameter to `http://127.0.0.1/`.
2) `http://127.1/admin`.
3) If still not working, try to encode "a" with `%2561`.

## SSRF via flawed request parsing

```
GET https://YOUR-LAB-ID.web-security-academy.net/
Host: BURP-COLLABORATOR-SUBDOMAIN
```
Notice that when you do this, modifying the Host header no longer causes your request to be blocked.
Use the Host header to scan the IP range 192.168.0.0/24.

## SSRF with filter bypass via open redirection vulnerability

1) `stockApi` parameter and observe that it isn't possible to make the server issue the request directly to a different host.
2) Click "next product" and observe that the path parameter is placed into the Location header of a redirection response, resulting in an open redirection.
3) Create URL `/product/nextProduct?path=http://192.168.0.12:8080/admin` and feed it to the `stockApi` parameter.
4) Delete user `/product/nextProduct?path=http://192.168.0.12:8080/admin/delete?username=carlos`.

# ByPasses:

## Bypassing filters by obfuscating URLs or using IP addresses in different formats (eg decimal, hexadecimal).

```
http://2130706433/admin
```
