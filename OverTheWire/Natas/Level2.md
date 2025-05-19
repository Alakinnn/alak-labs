# Solution
## Enumeration
Reading source, we see a subdirectory calls `files`
```
|   |
|---|
|<div id="content">|
||There is nothing on this page|
||<img src="[files/pixel.png](http://natas2.natas.labs.overthewire.org/files/pixel.png)">|
||</div>|
```

Manually visit the subdir, we see a users.txt which contains the password:
```
# username:password
alice:BYNdCesZqW
bob:jw2ueICLvT
charlie:G5vCxkVV3m
natas3:3gqisGdR0pjm6tpkDKdIWO2hSvchLeYH
eve:zo4mJWyNj2
mallory:9urtcpzBmH
```
