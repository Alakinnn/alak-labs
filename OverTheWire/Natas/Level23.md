# Solution
ChatGPT said this is a type juggling vulnerability:
```php
<?php    if(array_key_exists("passwd",$_REQUEST)){  
        if(strstr($_REQUEST["passwd"],"iloveyou") && ($_REQUEST["passwd"] > 10 )){  
            echo "<br>The credentials for the next level are:<br>";  
            echo "<pre>Username: natas24 Password: <censored></pre>";  
        }  
        else{  
            echo "<br>Wrong!<br>";  
        }  
    }    // morla / 10111  
?>
```

Meaning, the password we enter, as long as it contains `iloveyou`, it's valid. To pass the length check, we just need to add a number larger than 10 in front of `iloveyou`.

```
11iloveyou
```

```
MeuqmfJ8DDKuTr5pcvzFKSwlxedZYEWd
```