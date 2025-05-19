# Solution
Reading source reveals:
```php
  
<?  
  
$encodedSecret = "3d3d516343746d4d6d6c315669563362";  
  
function encodeSecret($secret) {  
    return bin2hex(strrev(base64_encode($secret)));  
}  
  
if(array_key_exists("submit", $_POST)) {  
    if(encodeSecret($_POST['secret']) == $encodedSecret) {  
    print "Access granted. The password for natas9 is <censored>";  
    } else {  
    print "Wrong secret";  
    }  
}  
?>
```

To decode:
```php
<?php
function decodeSecret($secret){
  return base64_decode(strrev(hex2bin($secret)));
  }
  
print "The secret is: "; 
print decodeSecret("3d3d516343746d4d6d6c315669563362");
print "\n";
?>
```

```
ZE1ck82lmdGIoErlhQgWND6j2Wzz6b6t
```