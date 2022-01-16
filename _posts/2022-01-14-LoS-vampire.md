---
layout: post
title: "LoS vampire"
author: "isanghyeon"
tags: LordofSQLi
---

|문제 명|풀이 여부|URL|
|:------:|:---:|:-----:|
| **vampire** | <span style="color:red">O</span> | https://los.rubiya.kr/chall/vampire_e3f1ef853da067db37f342f3a1881156.php |

<hr/><br>

### Challenge Source Code
```php
<?php 
  include "./config.php"; 
  login_chk(); 
  $db = dbconnect(); 
  if(preg_match('/\'/i', $_GET[id])) exit("No Hack ~_~");
  $_GET[id] = strtolower($_GET[id]);
  $_GET[id] = str_replace("admin","",$_GET[id]); 
  $query = "select id from prob_vampire where id='{$_GET[id]}'"; 
  echo "<hr>query : <strong>{$query}</strong><hr><br>"; 
  $result = @mysqli_fetch_array(mysqli_query($db,$query)); 
  if($result['id'] == 'admin') solve("vampire"); 
  highlight_file(__FILE__); 
?>
```

<br>

### Analysis Source Code
- #### preg_match function for $_GET[id]
> ```php 
> preg_match('/\'/i', $_GET[id])
> ```
- #### strtolower function for $_GET[id]
> ```php 
> $_GET[id] = strtolower($_GET[id]);
> ```
- #### str_replace function for $_GET[id]
> ```php 
> $_GET[id] = str_replace("admin","",$_GET[id]);
> ```
- #### Solve Conditions
> ```php
> if($result['id'] == 'admin') solve("vampire");
> ```

|query|parms1|
|:--:|:--:|
|select id from prob_vampire where id='{$_GET[id]}'|id='{$_GET[id]}'|

``` $_GET[id] ``` 의 값을 strtolower를 통해 소문자로 변경한 뒤 str_replace 를 통해 'admin' 문자열은 ''로 치환한다.  
str_replace 함수의 잘못된 구현으로 인해 특정 문자열을 치환하는 상황에서 이를 bypass할 수 있다.  
```php
$_GET[id] = str_replace("admin","",$_GET[id]);
와 같은 코드에서 $_GET[id]의 값이 'admin'일 경우 치환되지만, 'adadminin' 등일 경우 치환되더라도 다시 'admin'이 만들어져 bypass가 가능하다.
```

<hr/>
<br>

### Exploit Source Code
|작성 언어|Exploit Link|How to Exploit|
|:------:|:---:|:--:|
| **Python3.8** | https://github.com/isanghyeon/LordofSQLi-Solved-Exploit/tree/master/vampire | str_replace bypass |