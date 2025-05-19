# Solution
Cookie logged in with account called `ad`:
```
3537312d6164
```

It is a hex-encoded of:
```
571-ad
```

Since it says it is similar to the previous problem, we only need to deal from 1-640.

We simply use burp to fuzz number 1 from 1 to 640 and hex-encode it
```
$1$d61646d696e
```

```
p5mCvP7GS2K6Bmt3gqhM2Fc1A5T8MVyw
```