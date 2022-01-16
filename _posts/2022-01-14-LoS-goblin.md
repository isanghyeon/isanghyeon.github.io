---
layout: post
title: "LoS goblin"
author: "isanghyeon"
tags: wargame
---

|문제 명|풀이 여부|URL|
|:------:|:---:|:-----:|
| **goblin** | <span style="color:red">O</span> | https://los.rubiya.kr/chall/goblin_e5afb87a6716708e3af46a849517afdc.php |

<hr/><br>

### Challenge Source Code
```php
<?php 
  include "./config.php"; 
  login_chk(); 
  $db = dbconnect(); 
  if(preg_match('/prob|_|\.|\(\)/i', $_GET[no])) exit("No Hack ~_~"); 
  if(preg_match('/\'|\"|\`/i', $_GET[no])) exit("No Quotes ~_~"); 
  $query = "select id from prob_goblin where id='guest' and no={$_GET[no]}"; 
  echo "<hr>query : <strong>{$query}</strong><hr><br>"; 
  $result = @mysqli_fetch_array(mysqli_query($db,$query)); 
  if($result['id']) echo "<h2>Hello {$result[id]}</h2>"; 
  if($result['id'] == 'admin') solve("goblin");
  highlight_file(__FILE__); 
?>
```

<br>

### Analysis Source Code
- #### preg_match function for $_GET[no]
> ```php 
> preg_match('/prob|_|\.|\(\)/i', $_GET[no]))
> ```
> ```php 
> preg_match('/\'|\"|\`/i', $_GET[no])
> ```
- #### Solve Conditions
> ```php
> if($result['id'] == 'admin') solve("goblin");
> ```

> - ``` $_GET[no] ``` 에 대해 ``` '/prob|_|\.|\(\)/i' ``` 와 ``` '/\'|\"|\`/i' ``` 에 대해 각각 필터링 한다. <span style="color:red">[single and double Quotes]  </span>  
> - ``` if($result['id'] == 'admin') solve("goblin"); ``` 다음 코드와 같이 특정 query 를 전송했을 때 데이터베이스에서 반환되는 array의 "id" 인덱스가 'admin'일 경우 solve 된다.  

|query|parms1|parmas2|
|:--:|:--:|:--:|
|select id from prob_goblin where id='guest' and no={$_GET[no]}|id='guest'|no={$_GET[no]}|

<hr/>
<br>

### Exploit Source Code
|작성 언어|Exploit Link|How to Exploit|
|:------:|:---:|:--:|
| **Python3.8** | https://github.com/isanghyeon/LordofSQLi-Solved-Exploit/tree/master/goblin | SQL injection |