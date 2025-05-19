# Solution
Source code gives:
```php
function xor_encrypt($in) {
    $key = '<censored>';
    $text = $in;
    $outText = '';

    // Iterate through each character
    for($i=0;$i<strlen($text);$i++) {
    $outText .= $text[$i] ^ $key[$i % strlen($key)];
    }

    return $outText;
}
```

```php
function loadData($def) {
    global $_COOKIE;
    $mydata = $def;
    if(array_key_exists("data", $_COOKIE)) {
        $tempdata = json_decode(xor_encrypt(base64_decode($_COOKIE["data"])), true);
         if(is_array($tempdata) && array_key_exists("showpassword", $tempdata) && array_key_exists("bgcolor", $tempdata)) {
            if (preg_match('/^#(?:[a-f\d]{6})$/i', $tempdata['bgcolor'])) {
            $mydata['showpassword'] = $tempdata['showpassword'];
            $mydata['bgcolor'] = $tempdata['bgcolor'];
         }
      }
    }
    return $mydata;
}
```
If there is a “data” array from the browser cookie, the value is base64 decoded, then XOR encrypted (since XOR is reversible with the same mathematical function, this is equivalent to XOR decoding), then JSON decoding.

Then the bgcolor is read out of the array and checked against a hex color regex. If the “showpassword” key exists, it is also copied over to the `$mydata` value.

There’s also a `saveData()` function, which encodes and encrypts the data array and saves it as a cookie in the user’s browser:

```php
function saveData($d) {
    setcookie("data", base64_encode(xor_encrypt(json_encode($d))));
}
```

There’s a function to get the `bgcolor` request from the user:

```php
if(array_key_exists("bgcolor",$_REQUEST)) {
    if (preg_match('/^#(?:[a-f\d]{6})$/i', $_REQUEST['bgcolor'])) {
        $data['bgcolor'] = $_REQUEST['bgcolor'];
    }
}
```

And lastly, `saveData()` is called with the default values loaded in:

```php
$data = loadData($defaultdata);
saveData($data);
```

Again, those default values are `array( "showpassword"=>"no", "bgcolor"=>"#ffffff");`

## Exploitation
The cookie i got is, which is already URL decoded:
```
HmYkBwozJw4WNyAAFyB1VUcqOE1JZjUIBis7ABdmbU1GIjEJAyIxTRg=
```

use a [PHP compiler](https://www.w3schools.com/php/phptryit.asp?filename=tryphp_compiler&ref=learnhacking.io) to make a cookie without XOR encryption, using just `array( "showpassword"=>"no", "bgcolor"=>"#ffffff");`

This gives us a cookie value of `eyJzaG93cGFzc3dvcmQiOiJubyIsImJnY29sb3IiOiIjZmZmZmZmIn0=`.

### Get the XOR key
Use [CyberChef](https://gchq.github.io/CyberChef/?ref=learnhacking.io#recipe=From_Base64\('A-Za-z0-9%2B/%3D',true\)XOR\(%7B'option':'Base64','string':'eyJzaG93cGFzc3dvcmQiOiJubyIsImJnY29sb3IiOiIjZmZmZmZmIn0%3D'%7D,'Standard',false\)&input=Q2xWTEloNEFTQ3NDQkU4bEF4TWFjRk1aVjJoZFZWb3RFaGhVSlFOVkFtaFNFVjRzRnhGZWFBdz0) or a similar tool to XOR `HmYkBwozJw4WNyAAFyB1VUcqOE1JZjUIBis7ABdmbU1GIjEJAyIxTRg=` with `eyJzaG93cGFzc3dvcmQiOiJubyIsImJnY29sb3IiOiIjZmZmZmZmIn0=`.

We get:
```
eDWoeDWoeDWoeDWoeDWoeDWoeDWoeDWoeDWoeDWoe
```

We then need to use only one repeating part which is `eDWo` to perform XOR and base64 encode on the `{"showpassword"=>"yes", "bgcolor"=>"#ffffff"}`

Gives us the exploitable cookie:
```
HmYkBwozJw4WNyAAFyB1VUc9MhxHaHUNAic4Awo2dVVHZzEJAyIxCUc5
```

```
The password for natas12 is yZdkjAYZRd3R7tq7T5kXMjMJlOIkzDeB
```
