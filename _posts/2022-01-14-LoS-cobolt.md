---
layout: post
title: "LoS cobolt"
author: "isanghyeon"
tags: wargame
---

|문제 명|풀이 여부|URL|
|:------:|:---:|:-----:|
| **cobolt** | <span style="color:red">O</span> | https://los.rubiya.kr/chall/cobolt_b876ab5595253427d3bc34f1cd8f30db.php |

<hr/><br>

### Challenge Source Code
```php
<?php
  include "./config.php"; 
  login_chk();
  $db = dbconnect();
  if(preg_match('/prob|_|\.|\(\)/i', $_GET[id])) exit("No Hack ~_~"); 
  if(preg_match('/prob|_|\.|\(\)/i', $_GET[pw])) exit("No Hack ~_~"); 
  $query = "select id from prob_cobolt where id='{$_GET[id]}' and pw=md5('{$_GET[pw]}')"; 
  echo "<hr>query : <strong>{$query}</strong><hr><br>"; 
  $result = @mysqli_fetch_array(mysqli_query($db,$query)); 
  if($result['id'] == 'admin') solve("cobolt");
  elseif($result['id']) echo "<h2>Hello {$result['id']}<br>You are not admin :(</h2>"; 
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
> if($result['id'] == 'admin');
> ```

> - ``` $_GET[id] ``` ``` $_GET[pw] ``` 에 대해 각각 ```'/prob|_|\.|\(\)/i'``` 을 대소문자 구문 없이 핕터링 하고 있다.  
> - ``` if($result['id'] == 'admin'); ``` 와 같이 특정 query를 데이터베이스에 전송했을 때 반환되는 array의 'id' 인덱스의 값이 'admin'이면 solve 된다.

|query|parms1|params2|
|:--:|:--:|:--:|
|select id from prob_cobolt where id='{$_GET[id]}' and pw=md5('{$_GET[pw]}')|id='{$_GET[id]}'|pw=md5('{$_GET[pw]}'|


<hr/>
<br>

### Exploit Source Code
|작성 언어|Exploit Link|How to Exploit|
|:------:|:---:|:--:|
| **Python3.8** | https://github.com/isanghyeon/LordofSQLi-Solved-Exploit/tree/master/cobolt | SQL injection |