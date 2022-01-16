---
layout: post
title: "KERIS CTF 2018 Quals Write up"
author: "isanghyeon"
tags: CTF
---

해당 내용은 2018년 대구대학교에서 개최된 KERIS CTF 2018 Quals의 Write Up이다.

<img src="https://raw.githubusercontent.com/isanghyeon/isanghyeon.github.io/main/images/2018-12-02-KERIS-CTF-2018-Quals-Write-up/Rank-daegu.png" width="800px" height="300px" title="Rank_daegu"/>

<br>
<hr>
<br>

<img src="https://raw.githubusercontent.com/isanghyeon/isanghyeon.github.io/main/images/2018-12-02-KERIS-CTF-2018-Quals-Write-up/Rank-all.png" width="800px" height="300px" title="Rank_all"/>

<br>
<hr>
<br>

<img src="https://raw.githubusercontent.com/isanghyeon/isanghyeon.github.io/main/images/2018-12-02-KERIS-CTF-2018-Quals-Write-up/Solved.png" width="800px" height="300px" title="Solved"/>

<br>
<hr>
<br>


## Category

### # Low, General
- #### 10문제 정도가 출제되었으며, 법률에 대한 이해도가 있고 간단한 검색을 통해 답을 얻을 수 있다.

### # System(Pwnable)
- #### System 2 - exec (400P)
> nc 서버로 접속하면 "Which command do you want to execute?" 라는 문자열이 출력된다.  
> "ls" 명령어를 통해 현재 디렉토리의 파일을 확인해보니 flag 파일이 존재했고, "cat flag" 명령어를 사용해 flag를 읽을 수 있었다.  
```bash 
nc server port 
Which command do you wnat to execute? 
ls 
cat flag 
FLAG{Fak3_L0v3_i5_7h3_s3v3n733n7h_n0n-Engli5h_50ng}
```









[Pirate Ipsum](http://pirateipsum.me/)
