---
layout: post
title: "LoS gremlin"
author: "isanghyeon"
tags: LordofSQLi
---

|문제 명|풀이 여부|URL|
|:------:|:---:|:-----:|
| **gremlin** | <span style="color:red">O</span> | https://los.rubiya.kr/chall/gremlin_280c5552de8b681110e9287421b834fd.php |

<hr/><br>

### Challenge Source Code
```php
<?php
  include "./config.php";
  login_chk();
  $db = dbconnect();
  if(preg_match('/prob|_|\.|\(\)/i', $_GET[id])) exit("No Hack ~_~"); // do not try to attack another table, database!
  if(preg_match('/prob|_|\.|\(\)/i', $_GET[pw])) exit("No Hack ~_~");
  $query = "select id from prob_gremlin where id='{$_GET[id]}' and pw='{$_GET[pw]}'";
  echo "<hr>query : <strong>{$query}</strong><hr><br>";
  $result = @mysqli_fetch_array(mysqli_query($db,$query));
  if($result['id']) solve("gremlin");
  highlight_file(__FILE__);
?>
```

<br>

### Analysis Source Code
- #### preg_match function for $_GET[id]
> ```php 
> preg_match('/prob|_|\.|\(\)/i', $_GET[id])
> ```
- #### preg_match function for $_GET[pw]
> ```php 
> preg_match('/prob|_|\.|\(\)/i', $_GET[pw])
> ```
- #### Solve Conditions
> ```php
> if($result['id']) solve("gremlin");
> ```

> - ```$_GET[id]``` ```$_GET[pw]```  에 대해 각각 ```'/prob|_|\.|\(\)/i'``` 을 대소문자 구문 없이 핕터링 하고 있다.  
> - ``` if($result['id']) solve("gremlin"); ``` 다음 코드와 같이 특정 query 를 전송했을 때 데이터베이스에서 반환되는 array의 "id" 인덱스가 존재할 경우 solve 된다.

|query|parms1|params2|
|:--:|:--:|:--:|
|select id from prob_gremlin where id='{$_GET[id]}' and pw='{$_GET[pw]}'|id='{$_GET[id]}'|pw='{$_GET[pw]}'|

<hr/>
<br>

### Exploit Source Code
|작성 언어|Exploit Link|How to Exploit|
|:------:|:---:|:---:|
| **Python3.8** | https://github.com/isanghyeon/LordofSQLi-Solved-Exploit/tree/master/gremlin | SQL injection |