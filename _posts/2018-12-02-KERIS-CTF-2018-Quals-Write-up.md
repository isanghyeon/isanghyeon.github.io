---
layout: post
title: "KERIS CTF 2018 Quals Write up"
author: "isanghyeon"
tags: CTF
---

해당 내용은 2018년 대구대학교에서 개최된 KERIS CTF 2018 Quals의 Write Up이다.

<img src="https://raw.githubusercontent.com/isanghyeon/isanghyeon.github.io/main/images/2018-12-02-KERIS-CTF-2018-Quals-Write-up/Rank-daegu.png" width="600px" height="400px" title="Rank_daegu"/>

<img src="https://raw.githubusercontent.com/isanghyeon/isanghyeon.github.io/main/images/2018-12-02-KERIS-CTF-2018-Quals-Write-up/Rank-all.png" width="600px" height="400px" title="Rank_all"/>

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
<br>

### # System(Pwnable)
- #### System 2 - exec (400P)
> nc 서버로 접속하면 "Which command do you want to execute?" 라는 문자열이 출력된다.  
> "ls" 명령어를 통해 현재 디렉토리의 파일을 확인해보니 flag 파일이 존재했고, "cat flag" 명령어를 사용해 flag를 읽을 수 있었다.  
> <pre><code>
> nc server port 
> Which command do you wnat to execute? 
> ls 
> cat flag 
> FLAG{Fak3_L0v3_i5_7h3_s3v3n733n7h_n0n-Engli5h_50ng}
> </code></pre>

<hr/>
<br>

### # Web
- #### Web 1 - login (300P)
> 웹 페이지에 접속하면 modal 형태의 로그인과 회원가입 창을 보여준다.  
> 소스코드 보기를 통해 주석 처리된 review.php 경로를 알 수 있었고, 해당 페이지에 접속하면 review.php 코드를 보여준다.  
> 코드를 통해 해당 서버는 MySQL 환경을 사용하고 있음을 알 수 있고, SQL injection 이 가능하다.  
<pre><code>query: ' or '1'='1 </code></pre>

- #### Web 2 - Puzzle (400P)
> 웹 페이지에 접속하면 어떤 이미지를 무작위로 잘라 보여주고 있다.  
> 소스 코드 보기를 통해 보면 자바스크립트가 존재하며, 난독화되어 있다.  
> 난독화된 자바스크립트를 그대로 콘솔에 입력하면 어떤 경로가 나오는데, 이를 바탕으로 자바스크립트를 실행하면 flag를 얻을 수 있다.

<hr/>
<br>

### # Network
- #### Network 1 - FindMe (300P)
> pcapng 파일을 주고 이를 분석해 풀어야하는 문제이다.  
> pcapng 파일을 wireshark로 열어 "ctrl + f"를 통해 flag 형식을 검색하면 tcp 패킷이 나온다.  
> 하지만 flag가 잘려서 보이기 때문에 flag가 들어있는 모든 패킷을 읽어야 하므로 "Analysis -> Follow -> TCP Stream" 을 통해 찾은 패킷을 모두 읽어들인다. 

<img src="https://raw.githubusercontent.com/isanghyeon/isanghyeon.github.io/main/images/2018-12-02-KERIS-CTF-2018-Quals-Write-up/FLAG-images.png" width="600px" height="400px" title="FLAG"/>

<pre><code>flag{N3tw0rk_Ch@llenge_SOlv3d!_Congr@tz!!}</code></pre>

- #### Network 2 - Router (500P)
> Cisco Router의 IOS 문제였다.  
> > 1. 라우터의 기본적인 정보를 확인하고 첫 번째 FLAG{}를 획득하시오.
> <pre><code>
> Router> show version 
> [+] FLAG1{C15c0_Pack3t_Tr4c3r_H4ve_U}
> </code></pre>
>  
> > 2. 라우터의 이름을 keris2018로 설정하고. enable secret을 keris2018secret으로. enble password를 whoisthewinnerofkeris2018로 설정하는 명령어를 차례대로 입력하고 FLAG2{}를 획득하시오.  
> <pre><code>
> Router> enable
> Router# configure terminal
> Router(config)# hostname keris2018
>   [+] Ok, you set hostname
> Router(config)# enable secret keris2018secret
>   [+] Ok, you set enable secret
> Router(config)# enable password whoisthewinnerofkeris2018
>   [+] Ok, you set enable password
>
>   [+] FLAG2{Ev3r_u5ed_itttttt?}
> </code></pre>
>  
> > 3. enable shell에 접속하기 위한 패스워드의 암호화된 값을 볼 수 있는 명령어를 이용하여 암호화된 패스워드의 값을 확인하고, 해당 값을 FLAG로 입력하시오.  
> <pre><code>
> Router(config)# exit
> Router# show running-config
>   enable secret 5 $1$mERr$ATL1hEB9UJOrNnI6iWy.R/
>   enable password 7 08364441000A111F171C050A242E3627353E27010E0551510701
> </code></pre>
> <pre><code>FLAG: FLAG1_FLAG2_encrypt1234</code></pre>
> <pre><code> flag{C15c0_Pack3t_Tr4c3r_H4ve_U_Ev3r_u5ed_itttttt?_08364441000A111F171C050A242E3627353E27010E0551510701}</code></pre>

<hr/>
<br>

### # Reverse Engineering (REV)
- #### REV 1 - hand-ray (300P)
> assembly 파일과 nc 서버 주소를 함께 준다.  
> assembly 를 조금만 읽을 수 있다면 쉽게 접근할 수 있는 문제였다. 
<pre><code>
gdb-peda$ disas main

Dump of assembler code for function main:

   0x0000000000400686 <+0>: push   rbp
   0x0000000000400687 <+1>: mov    rbp,rsp
   0x000000000040068a <+4>: sub    rsp,0x10
   0x000000000040068e <+8>: mov    rax,QWORD PTR fs:0x28
   0x0000000000400697 <+17>: mov    QWORD PTR [rbp-0x8],rax
   0x000000000040069b <+21>: xor    eax,eax
   0x000000000040069d <+23>: mov    edi,0x400784
   0x00000000004006a2 <+28>: call   0x400520 <puts@plt>
   0x00000000004006a7 <+33>: lea    rax,[rbp-0xc]
   0x00000000004006ab <+37>: mov    rsi,rax
   0x00000000004006ae <+40>: mov    edi,0x400797
   0x00000000004006b3 <+45>: mov    eax,0x0
   0x00000000004006b8 <+50>: call   0x400560 <__isoc99_scanf@plt>
   0x00000000004006bd <+55>: mov    eax,DWORD PTR [rbp-0xc]
   0x00000000004006c0 <+58>: cmp    eax,0x1c0552315
   0x00000000004006c5 <+63>: jne    0x4006db <main+85>
   0x00000000004006c7 <+65>: mov    edi,0x40079a
   0x00000000004006cc <+70>: call   0x400540 <system@plt>
   0x00000000004006d1 <+75>: mov    edi,0x0
   0x00000000004006d6 <+80>: call   0x400570 <exit@plt>
   0x00000000004006db <+85>: mov    edi,0x4007a3
   0x00000000004006e0 <+90>: call   0x400520 <puts@plt>
   0x00000000004006e5 <+95>: mov    eax,0x0
   0x00000000004006ea <+100>: mov    rdx,QWORD PTR [rbp-0x8]
   0x00000000004006ee <+104>: xor    rdx,QWORD PTR fs:0x28
   0x00000000004006f7 <+113>: je     0x4006fe <main+120>
   0x00000000004006f9 <+115>: call   0x400530 <__stack_chk_fail@plt>
   0x00000000004006fe <+120>: leave  
   0x00000000004006ff <+121>: ret    

End of assembler dump.
gdb-peda$ x/s 0x400784
0x400784: "Type your password"
gdb-peda$ x/s 0x400797
0x400797: "%d"
gdb-peda$ x/s 0x40079a
0x40079a: "cat flag"
gdb-peda$ x/s 0x4007a3
0x4007a3: "Wrong..."
gdb-peda$
</code></pre>

> assembly 를 보면 main+58 에서 ```cmp eat, 0x1c0552315``` 과 같은 코드가 존재하는데, 이는 사용자가 입력한 값이 eax에 들어가고 0x1c0552315와 비교해 참이면 flag를 출력하게 한다.  
> 따라서 16진수인 0x1c0552315를 10진수로 변환하면 29381923이 되고, 이를 nc 서버에 접속해 넘기게 되면 flag를 받을 수 있다.

<hr/>
<br>

### Crypto
- #### Crypto 1 - XOR (300P)
> XOR된 문자열을 주고 Key는 한 자리이다.  
> ```$.#%96-&#;b+1b!.-7&;lb1-b/;b$''.+,%b+1b,-6b%--&b'+6*'0l? ```  
> 키는 한자리라고 했으니 브루트 포스를 통하여 flag를 구한다.