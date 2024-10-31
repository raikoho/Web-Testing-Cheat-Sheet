## JWT authentication bypass via unverified signature

In the Inspector panel, change the value of the `sub` claim from `wiener` to `administrator`. And try to /admin.

## JWT authentication bypass via flawed signature verification

Inspector panel, change the value of the `sub` claim to `administrator`. Value of the `alg` parameter to `none`.

## JWT authentication bypass via weak signing key

1) Copy the JWT and brute-force the secret. You can do this using hashcat as follows:

```
hashcat -a 0 -m 16500 <YOUR-JWT> /path/to/jwt.secrets.list
```
https://github.com/wallarm/jwt-secrets/blob/master/jwt.secrets.list

2) Base64 encode the secret.
3) JWT Editor Keys tab and click New Symmetric Key. In the dialog, click Generate to generate a new key in JWK format.
4) Replace the generated value for the `k` property with the Base64-encoded secret.
5) Go back to the `GET /admin` and switch to the extension-generated JSON Web Token message editor tab.
6) change the value of the `sub` claim to `administrator`.
7) At the bottom of the tab, click Sign, then select the key that you generated. Make sure that the Don't modify header option is selected, then click OK.
8) Send and now you can enter /admin-panel.


## JWT authentication bypass via jwk header injection

1) New RSA Key -> Generate, OK.
2) Go back to the GET /admin and switch to the JSON Web Token tab.
3) change the value of the sub claim to administrator.
4) At the bottom of the JSON Web Token tab, click Attack, then select Embedded JWK. When prompted, select your newly generated RSA key and click OK.

## JWT authentication bypass via jku header injection

1) `JWT Editor Keys` tab in Burp's main tab bar. Click `New RSA Key`. In the dialog, click `Generate`.
2) Exploit Server Body:

```
{
    "keys": [

    ]
}
```

3) Back on the `JWT Editor Keys` tab, right-click on the entry for the key that you just generated, then select `Copy Public Key as JWK`.
4) Paste the JWK into the keys array on the exploit server:
```
{
    "keys": [
        {
            "kty": "RSA",
            "e": "AQAB",
            "kid": "893d8f0b-061f-42c2-a4aa-5056e12b8ae7",
            "n": "yy1wpYmffgXBxhAUJzHHocCuJolwDqql75ZWuCQ_cb33K2vh9mk6GPM9gNN4Y_qTVX67WhsN3JvaFYw"
        }
    ]
}
```

5) Go back to the `GET /admin` and switch to the extension-generated `JSON Web Token` tab.
6) Replace the `kid` parameter with the `kid` of the JWK that you uploaded to the exploit server.
7) Add a new `jku` parameter to the header of the JWT. Set its value to the URL of your JWK Set on the exploit server.
8) Change the value of the `sub` claim to `administrator`.
9) Click "Sign", then select the RSA key that you generated. Don't modify header option is selected, then click OK.

## JWT authentication bypass via kid header path traversal

1) JWT Editor Keys tab -> New Symmetric Key.
2) Click Generate to generate a new key in JWK format.
3) Replace the generated value for the k property with a Base64-encoded null byte (AA==).
4) Go back to `GET /admin` and switch to the `JSON Web Token` message editor tab.
5)  In the header of the JWT, change the value of the kid parameter to a path traversal sequence pointing to the /dev/null file:

```
../../../../../../../dev/null
```

6) Change the value of the `sub` claim to `administrator`.
7) Click Sign, then select the symmetric key that you generated. Don't modify header option is selected, then click OK.
