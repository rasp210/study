# Cookie&Compress

[TOC]

### Cookie 设置与获取

##### 1. 种 Cookie

```php
bool setcookie ( string $name [, string $value = "" [, int $expire = 0 [, string $path = "" [, string $domain = "" [, bool $secure = false [, bool $httponly = false ]]]]]] )
```

*注： \$value 的最大值在 4KB 左右； $expire 是 Unix 时间戳；*

##### 2. 获取 Cookie

```php
$_COOKIE: $_COOKIE['cookiename']
$_REQUEST: $_REQUEST['Cookies']['cookiename']
```



### 加解密方式介绍

##### 1. 不可逆加密方法

[md5]:http://php.net/manual/zh/function.md5.php	"PHP 4, PHP 5, PHP 7"
[crypt]:http://php.net/manual/zh/function.crypt.php	"PHP 4, PHP 5, PHP 7"
[password_hash]:http://php.net/manual/zh/function.password-hash.php	"PHP 5 >= 5.5.0, PHP 7"

##### 2. 加解密方法 

[openssl_encrypt]:http://php.net/manual/zh/function.openssl-encrypt.php	"PHP 5 >= 5.3.0, PHP 7"
[openssl_decrypt]:http://php.net/manual/zh/function.openssl-decrypt.php	"PHP 5 >= 5.3.0, PHP 7"



### 压缩方式介绍

#### 三种压缩方式

#####  gzdeflate

```php
string gzdeflate ( string $data [, int $level = -1 [, int $encoding = ZLIB_ENCODING_RAW ]] )
```

使用默认的数据格式压缩字符串

* $data: 被压缩的字符串
* $level: 压缩等级，可选参数：0-9，默认为 -1，表示第 6 等级
* $encoding:  压缩编码

  [gzdeflate]:http://php.net/manual/zh/function.gzdeflate.php"PHP 4 >= 4.0.4, PHP 5, PHP 7"



##### gzinflate

```php
string gzinflate ( string $data [, int $length = 0 ] )
```

解压一个 deflate 过的字符串

* $data: 被解压的字符串
* \$length: 要解压的 \$data 长度。默认为 0，表示全部解压；$length 的值应不小于 \$data 的长度，否则函数将返回 false

  [gzinflate]:http://php.net/manual/zh/function.gzinflate.php"PHP 4 &gt;= 4.0.4, PHP 5, PHP 7"



##### gzcompress

```php
string gzcompress ( string $data [, int $level = -1 [, int $encoding = ZLIB_ENCODING_DEFLATE ]] )
```

使用 ZLIB 数据格式压缩字符串

* $data: 被压缩的字符串
* $level: 压缩等级，可选参数：0-9，默认为 -1，表示第 6 等级
* $encoding: 压缩编码

  [gzcompress]:http://php.net/manual/zh/function.gzcompress.php"PHP 4 >= 4.0.1, PHP 5, PHP 7"



##### gzuncompress

```php
string gzuncompress ( string $data [, int $length = 0 ] )
```

解压一个被 gzcompress 过的字符串

* $data: 被解压的字符串
* \$length: 要解压的 \$data 长度。默认为 0，表示全部解压；$length 的值应不小于 \$data 的长度，否则函数将返回 false

  [gzuncompress]:http://php.net/manual/zh/function.gzuncompress.php"PHP 4 >= 4.0.1, PHP 5, PHP 7"



##### gzencode

```php
string gzencode ( string $data [, int $level = -1 [, int $encoding_mode = FORCE_GZIP ]] )
```

返回与 gzip 程序压缩一样的结果，结果为字符串

* $data: 被压缩的字符串
* $level: 压缩等级，可选参数：0-9，默认为 -1，表示第 6 等级
* $encoding_mode: 可选值 FORCE_GZIP or FORCE_DEFLATE

  [gzencode]:http://php.net/manual/zh/function.gzencode.php"PHP 4 >= 4.0.4, PHP 5, PHP 7"



##### gzencode:

```
string gzdecode ( string $data [, int $length ] )
```

解压 gzencode 压缩过的字符串

* $data: 被解压的字符串

* \$length: 要解压的 \$data 长度。默认为 0，表示全部解压；$length 的值应不小于 \$data 的长度，否则函数将返回 false

  [gzencode]:http://php.net/manual/zh/function.gzdecode.php	"PHP 5 >= 5.4.0, PHP 7"



#### 三种压缩方式比较

```php
$str = '{"result":{"status":{"code":0,"msg":"succ"},"timestamp":"Thu May 11 14:34:41 +0800 2017","data":{"_id":{},"season_id":"57281dc8c8f97c17b12e9f5f","team_id":"571e4661599b241e4fd41c43","digest":"78bda72875e8ba740e18ce59dd968262","data":{"logo_uri":"https:\/\/cdn.openplay.com\/Fp4z7PoBjjog1pR2v85qqhZmkCqH","digest":"78bda72875e8ba740e18ce59dd968262","area":{"country":"\u4e2d\u56fd","province":"\u5e7f\u4e1c","district":"\u767d\u4e91","city":"\u5e7f\u5dde"},"name":"\u5efa\u6ed4\u5357\u4fa8","short_name":"\u5efa\u6ed4\u5357\u4fa8","id":"571e4661599b241e4fd41c43","players":{"0":{"roles":{"0":"player"},"gender":1,"height":null,"player_id":"590ae487d6e4a95fce451c49","name":"\u9648\u975e","avatar_uri":"https:\/\/cdn.openplay.com\/FqCXkX3UoEG7M7h8uRfvOnQbn81H","weight":null,"id":"590ae487d6e4a95fce451c4f","shirt_number":29},"1":{"roles":{"0":"player"},"gender":1,"height":null,"player_id":"571f4a277e44b7748e1c2428","name":"\u675c\u5fd7\u5f3a","avatar_uri":"https:\/\/cdn.openplay.com\/Fq4fp6aNf-Sx0e3xj1T3FPP919sq","weight":null,"id":"590ae3dad6e4a95fd2816938","shirt_number":21},"2":{"roles":{"0":"player"},"gender":1,"height":null,"player_id":"57ad776f599b246d09f7ce94","name":"\u8303\u667a\u777f","avatar_uri":"http:\/\/opimage.b0.upaiyun.com\/op\/sports\/6f57adca861d45eebdef3a5d217d8a33.png","weight":null,"id":"590ae3b2d6e4a95fcac3abeb","shirt_number":4},"3":{"roles":{"0":"player"},"gender":1,"height":null,"player_id":"571f4a2b7e44b7748e1c243c","name":"\u5c48\u5065\u806a","weight":null,"avatar_uri":"https:\/\/cdn.openplay.com\/FqJBPgHDrvI-gz95r4GZ9gOQpPK9","id":"590ad991d6e4a95fbf05c42c","shirt_number":86},"4":{"roles":{"0":"player"},"gender":1,"height":null,"player_id":"5629ea3cc8f97c3bac1b6176","name":"\u97e9\u6bc5","weight":null,"avatar_uri":"https:\/\/cdn.openplay.com\/Fh-QKLaIkU_KhoXmGTokZAnW5FnH","id":"590ad97ad6e4a95fce451c3d","shirt_number":25},"5":{"roles":{"0":"player"},"gender":1,"height":null,"player_id":"571f4a127e44b7748e1c23ac","name":"\u6c88\u61ff\u7fc0","avatar_uri":"https:\/\/cdn.openplay.com\/FtGCvk4zljFUo3Iv_3E8UzAgS1Nm","weight":null,"id":"590ad94ad6e4a95803d3feb4","shirt_number":34},"6":{"roles":{"0":"player"},"gender":1,"height":null,"player_id":"57208e48af813226d3710314","name":"\u5ed6\u5fb7\u4f26","weight":null,"avatar_uri":"https:\/\/cdn.openplay.com\/FrR6-P7MYnw0FDYN0A-Xc_uh1bfI","id":"590ad90bd6e4a957f8f0d92f","shirt_number":14},"7":{"roles":{"0":"player"},"gender":1,"height":null,"player_id":"57285db4c8f97c77b47a87e6","name":"\u9093\u8363\u8fbe","weight":null,"avatar_uri":"https:\/\/cdn.openplay.com\/Fj6PYCXl2NKKefsNwy2dUIBg_mYB","id":"590ad8bed6e4a95fce451c37","shirt_number":11},"8":{"roles":{"0":"player"},"gender":1,"height":null,"player_id":"571f4a147e44b7748e1c23b8","name":"\u82cf\u4e39","avatar_uri":"https:\/\/cdn.openplay.com\/FvlQ24myLSiBSxbwRtcc_2oxBuDs","weight":null,"id":"590ad8abd6e4a957f47d8704","shirt_number":2},"9":{"roles":{"0":"player"},"gender":1,"height":null,"player_id":"5657d4cfc8f97c3817b8adc8","name":"\u674e\u884c\u6052","weight":null,"avatar_uri":"https:\/\/cdn.openplay.com\/FujbxA3TIptUOBU2d9uTFXB3MvCx","id":"590ad8a1d6e4a957f9af03ed","shirt_number":1},"10":{"roles":{"0":"player"},"gender":1,"height":null,"player_id":"571f4a227e44b7748e1c2408","name":"\u9648\u667a\u4f1f","weight":null,"avatar_uri":"https:\/\/cdn.openplay.com\/FgnvjMVEqRPoFHVEItT8du2ypnpg","id":"5721c57b599b244973585cf1","shirt_number":89},"11":{"roles":{"0":"player"},"gender":1,"height":null,"player_id":"571f4a2c7e44b7748e1c2444","name":"\u83ab\u5b50\u626c","weight":null,"avatar_uri":"https:\/\/cdn.openplay.com\/FvJwvgQACVJN8Diz6-DHxUOTHfV_","id":"5721c540599b2449770e4f1e","shirt_number":13},"12":{"roles":{"0":"team_manager"},"gender":1,"height":null,"player_id":"571e465d599b241e1f481f0d","name":"\u8521\u6c49\u7a57","weight":null,"avatar_uri":"https:\/\/cdn.openplay.com\/FtONVqiJN-2gp1rflqigxFOF6Yj_","id":"5721c539599b24496f5bb1a8"},"13":{"roles":{"0":"player"},"gender":1,"height":null,"player_id":"571f4a2e7e44b7748e1c244c","name":"\u5218\u6587\u9521","weight":null,"avatar_uri":"https:\/\/cdn.openplay.com\/FtKNswHigZxLig5HfF1Z7wa0zqhG","id":"5720cd06af8132260adfea0d","shirt_number":9},"14":{"roles":{"0":"player"},"gender":1,"height":null,"player_id":"57208e43af813226d37102f8","name":"\u8c2d\u56fd\u8f89","avatar_uri":"https:\/\/cdn.openplay.com\/Fs5BLuy9IrByhe0wX60O9T_8Nci7","weight":null,"id":"5720cd06af8132260adfea09","shirt_number":10},"15":{"roles":{"0":"player"},"gender":1,"height":null,"player_id":"571f4a257e44b7748e1c2418","name":"\u5468\u4fca\u5e73","weight":null,"avatar_uri":"https:\/\/cdn.openplay.com\/Fgf59hb7AVkSUMSt3i_PY8YQ6TFn","id":"5720cd05af8132260adfea01","shirt_number":22},"16":{"roles":{"0":"player"},"gender":1,"height":null,"player_id":"571f4a2e7e44b7748e1c2450","name":"\u9648\u4f1f\u4fca","avatar_uri":"https:\/\/cdn.openplay.com\/Fhie6-wiGHSAqF8qxYtUDMLYbALA","weight":null,"id":"5720cd04af8132260adfe9fd","shirt_number":69},"17":{"roles":{"0":"player"},"gender":1,"height":null,"player_id":"571f4a257e44b7748e1c241c","name":"\u8881\u632f\u7ef4","avatar_uri":"https:\/\/cdn.openplay.com\/FncHG6JsK3Y3ceAGuyGKOk9ANqy8","weight":null,"id":"5720cd01af8132260adfe9dd","shirt_number":33},"18":{"roles":{"0":"player"},"gender":1,"height":null,"player_id":"571f4a2a7e44b7748e1c2434","name":"\u674e\u5fd7\u6210","weight":null,"avatar_uri":"https:\/\/cdn.openplay.com\/FrIF_FhTbyCfh-CrVIGDqKGR1Oid","id":"5720cd01af8132260adfe9d9","shirt_number":83},"19":{"roles":{"0":"player"},"gender":1,"height":null,"player_id":"571f4a277e44b7748e1c2424","name":"\u9648\u6b63\u6839","avatar_uri":"https:\/\/cdn.openplay.com\/FjD45ajteRCOPLt6qZ74U247q5ep","weight":null,"id":"5720cd00af8132260adfe9d5","shirt_number":8},"20":{"roles":{"0":"player"},"gender":1,"height":null,"player_id":"571f4a267e44b7748e1c2420","name":"\u9648\u56fd\u6d77","weight":null,"avatar_uri":"https:\/\/cdn.openplay.com\/FgHFI2WsklXJmmQK6r8jW4pcJcfD","id":"5720ccffaf8132260adfe9c5","shirt_number":17}}},"microtime":1494396813.8055,"date":"2017-05-10","time":"14:13:33"}}}';

echo strlen($str), "\n";

$gd9 = gzdeflate($str, 9); 
echo strlen($gd9), "\n";

$gc9 = gzcompress($str, 9); 
echo strlen($gc9), "\n";

$ge9 = gzencode($str, 9); 
echo strlen($ge9), "\n";
```

*结果：*

str: 6034 

gzdeflate: 1756 

gzcompress: 1762 

gzencode: 1774



### 压缩方式介绍

##### base64_encode

编码后的数据要比原始数据多占用 33% 左右的空间

##### convert_uuencode

编码后的数据将会比源数据大 35% 左右



\$cookie = base64_encode(gzdeflate($str, 9));
\$str = gzinflate(base64_decode($cookie));





