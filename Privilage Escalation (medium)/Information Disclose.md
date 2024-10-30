## Information disclosure in error messages

1) GET /product?productId=1 try to write string like GET /product?productId="example".
2) The unexpected data type causes an exception, and a full stack trace is displayed in the response. This reveals that the lab is using Apache Struts 2 2.3.31.

## Information disclosure on debug page

1) Go to the `"Target" > "Site Map"` tab. Right-click on the top-level entry for the lab and select `"Engagement tools" > "Find comments"`. Notice that the home page contains an HTML comment that contains a link called "Debug". This points to `/cgi-bin/phpinfo.php`
2) "Send to Repeater" `/cgi-bin/phpinfo.php` and send the request to retrieve the file. Notice that it reveals various debugging information, including the SECRET_KEY environment variable.

## Source code disclosure via backup files

1) Find `/backup` and `ProductTemplate.java.bak` using **robots.txt**. Or use "site map Burp" and go to "Engagement tools" > "Discover content".
2) Browse to /backup/ProductTemplate.java.bak to access the source code. Notice hard-coded password for a Postgres database.

## Authentication bypass via information disclosure (privilage escalation)

1) `GET /admin`. Notice that it is only for administrators.
2) `TRACE /admin`. Notice that the `X-Custom-IP-Authorization` header, containing your IP address.
3) Go to **Proxy > Match and replace**.
4) Under HTTP match and replace rules, click Add. The Add match/replace rule dialog opens.
5) Leave the Match field empty.
6) Under Type, make sure that Request header is selected.
7) In the Replace field, enter the following:

```X-Custom-IP-Authorization: 127.0.0.1```

8) Click Test.
9) Under Auto-modified request, notice that Burp has added the X-Custom-IP-Authorization header to the modified request.
10) Click OK. Burp Proxy now adds the X-Custom-IP-Authorization header to every request you send.
11) Browse to the home page. Notice that you now have access to the admin panel, where you can delete carlos.

## Information disclosure in version control history (privilage escalation)

1) Open the lab and browse to `/.git` to reveal the lab's Git version control data.
2) Download a copy of this entire directory: `wget -r https://YOUR-LAB-ID.web-security-academy.net/.git/`.
3) Explore the downloaded directory using your local Git installation. Notice that there is a commit with the message "Remove admin password from config".
4) Look closer at the diff for the changed `admin.conf` file. Notice that the commit replaced the hard-coded admin password with variable **ADMIN_PASSWORD** instead.
