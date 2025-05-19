# Solution
```
1JNwQM1Oi6J6j1k49Xyw7ZN6pXMQInVj
```

Source:
```php
<?php  
  
// morla / 10111  
// database gets cleared every 5 min  
  
  
/*  
CREATE TABLE `users` (  
  `username` varchar(64) DEFAULT NULL,  
  `password` varchar(64) DEFAULT NULL  
);  
*/  
  
  
function checkCredentials($link,$usr,$pass){    $user=mysqli_real_escape_string($link, $usr);    $password=mysqli_real_escape_string($link, $pass);    $query = "SELECT username from users where username='$user' and password='$password' ";    $res = mysqli_query($link, $query);  
    if(mysqli_num_rows($res) > 0){  
        return True;  
    }  
    return False;  
}  
  
  
function validUser($link,$usr){    $user=mysqli_real_escape_string($link, $usr);    $query = "SELECT * from users where username='$user'";    $res = mysqli_query($link, $query);  
    if($res) {  
        if(mysqli_num_rows($res) > 0) {  
            return True;  
        }  
    }  
    return False;  
}  
  
  
function dumpData($link,$usr){    $user=mysqli_real_escape_string($link, trim($usr));    $query = "SELECT * from users where username='$user'";    $res = mysqli_query($link, $query);  
    if($res) {  
        if(mysqli_num_rows($res) > 0) {  
            while ($row = mysqli_fetch_assoc($res)) {                // thanks to Gobo for reporting this bug!  
                //return print_r($row);                return print_r($row,true);  
            }  
        }  
    }  
    return False;  
}  
  
  
function createUser($link, $usr, $pass){  
  
    if($usr != trim($usr)) {  
        echo "Go away hacker";  
        return False;  
    }    $user=mysqli_real_escape_string($link, substr($usr, 0, 64));    $password=mysqli_real_escape_string($link, substr($pass, 0, 64));    $query = "INSERT INTO users (username,password) values ('$user','$password')";    $res = mysqli_query($link, $query);  
    if(mysqli_affected_rows($link) > 0){  
        return True;  
    }  
    return False;  
}  
  
  
if(array_key_exists("username", $_REQUEST) and array_key_exists("password", $_REQUEST)) {    $link = mysqli_connect('localhost', 'natas27', '<censored>');    mysqli_select_db($link, 'natas27');  
  
  
    if(validUser($link,$_REQUEST["username"])) {        //user exists, check creds        if(checkCredentials($link,$_REQUEST["username"],$_REQUEST["password"])){  
            echo "Welcome " . htmlentities($_REQUEST["username"]) . "!<br>";  
            echo "Here is your data:<br>";            $data=dumpData($link,$_REQUEST["username"]);  
            print htmlentities($data);  
        }  
        else{  
            echo "Wrong password for user: " . htmlentities($_REQUEST["username"]) . "<br>";  
        }  
    }  
    else {        //user doesn't exist        if(createUser($link,$_REQUEST["username"],$_REQUEST["password"])){  
            echo "User " . htmlentities($_REQUEST["username"]) . " was created!";  
        }  
    }    mysqli_close($link);  
} else {  
?>
```

The application is proned against sql injection. However, we can still exploit it.
1. The username in the database is not unique. But it is checked by `validUser`
2. The username max length is 64 characters.
3. The database does not run strict mode.
4. The username is truncated if it exists; called in `dumpData`.

From these information, we have a payload:
```
natas28                                                         x
```

The payload is exactly 65 char-long, in which 7-letter is taken up by `natas28`, 57 whitespaces and an `x`. 

- Why 57 spaces? => To match exactly 64 length limit from the database
- Why an x? => To prevent the user from not being created. If there were only whitespaces, the server-side logic prevents us from creating a user. The x is truncated anyway due to it at pos > 64.
- After creating the user, in the database, there is this record:
```
natas28<57 spaces>                                                    
```

Why this matter?
From `dumpData`, if we login with the above username, it will be interpreted as `natas28`. As the username `natas28<57 spaces>` exists, but the spaces are truncated.

