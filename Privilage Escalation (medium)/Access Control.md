## Unprotected admin functionality

Just try to find something like unprotected /administrator-panel. With FUZZ or bruteforce or in robots.txt

## Unprotected admin functionality with unpredictable URL

Try to find admin panel from the source code (HTML, JavaScript etc):
```
<script>
	var isAdmin = false;
	if (isAdmin) {
		...
		var adminPanelTag = document.createElement('a');
		adminPanelTag.setAttribute('https://insecure-website.com/administrator-panel-yb556');
		adminPanelTag.innerText = 'Admin panel';
		...
	}
</script>
```
## Play with id parameters to find something:

https://insecure-website.com/myaccount?**id=456**

## User role controlled by request parameter

See **Admin=false**. Change it to **Admin=true** and just load /admin-panel.

## User role can be modified in user profile

add "roleid":2 into the JSON in the request body

## URL-based access control can be circumvented

1) Change the URL in the request line from /admin-panel to /
2) Add header X-Original-URL: /admin-panel

## URL-based access control can be circumvented

Experimental with POST, GET, POSTX, PUT when you try to do things that needs for privilage escalation

## Try Encode it a little or add some symbols:

``/ADMIN/DELETEUSER, /admin/deleteUser, /admin/deleteUser/, /admin/deleteUser.anything``

## There is a lot of others Access Control Problems with Promoting Systems, but its sounds easy and can be found at the PortSwigger
