---
layout: post
title: "KERIS CTF 2018 Quals Write up"
author: "isanghyeon"
tags: CTF
---

해당 내용은 2018년 대구대학교에서 개최된 KERIS CTF 2018 Quals의 Write Up이다.

<img src="https://raw.githubusercontent.com/isanghyeon/isanghyeon.github.io/main/images/2018-12-02-KERIS-CTF-2018-Quals-Write-up/Rank-daegu.png" width="600px" height="300px" title="Rank_daegu"/>

<br>
<hr/>
<br>

<img src="https://raw.githubusercontent.com/isanghyeon/isanghyeon.github.io/main/images/2018-12-02-KERIS-CTF-2018-Quals-Write-up/Rank-all.png" width="600px" height="300px" title="Rank_all"/>

<br>
<hr/>
<br>

<img src="https://raw.githubusercontent.com/isanghyeon/isanghyeon.github.io/main/images/2018-12-02-KERIS-CTF-2018-Quals-Write-up/Solved.png" width="800px" height="300px" title="Solved"/>

<br>
<hr/>
<br>


## Category

### # Low, General
- #### 10문제 정도가 출제되었으며, 법률에 대한 이해도가 있고 간단한 검색을 통해 답을 얻을 수 있다.

<hr/>

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

<hr/>

### # Web
- #### Web 1 - login (300P)
> 웹 페이지에 접속하면 modal 형태의 로그인과 회원가입 창을 보여준다.  
> 소스코드 보기를 통해 주석 처리된 review.php 경로를 알 수 있었고, 해당 페이지에 접속하면 review.php 코드를 보여준다.  
> 코드를 통해 해당 서버는 MySQL 환경을 사용하고 있음을 알 수 있고, SQL injection 이 가능하다.  
```SQL 
query: ' or '1'='1 
```

- #### Web 2 - Puzzle (400P)
> 웹 페이지에 접속하면 어떤 이미지를 무작위로 잘라 보여주고 있다.  
> 소스 코드 보기를 통해 보면 자바스크립트가 존재하며, 난독화되어 있다.  
> 난독화된 자바스크립트를 그대로 콘솔에 입력하면 어떤 경로가 나오는데, 이를 바탕으로 자바스크립트를 실행하면 flag를 얻을 수 있다.

<hr/>

### # Network
- #### Network 1 - FindMe (300P)
> pcapng 파일을 주고 이를 분석해 풀어야하는 문제이다.  
> pcapng 파일을 wireshark로 열어 "ctrl + f"를 통해 flag 형식을 검색하면 tcp 패킷이 나온다.  
> 하지만 flag가 잘려서 보이기 때문에 flag가 들어있는 모든 패킷을 읽어야 하므로 "Analysis -> Follow -> TCP Stream" 을 통해 찾은 패킷을 모두 읽어들인다. 

<img src="https://raw.githubusercontent.com/isanghyeon/isanghyeon.github.io/main/images/2018-12-02-KERIS-CTF-2018-Quals-Write-up/FLAG-images.png" width="600px" height="400px" title="FLAG"/>

```bash
flag{N3tw0rk_Ch@llenge_SOlv3d!_Congr@tz!!}
````





[Pirate Ipsum](http://pirateipsum.me/)
