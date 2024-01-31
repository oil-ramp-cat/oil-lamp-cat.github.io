---
title: -wargame- overthewire bandit
date: 2024-02-01 00:25:15 +09:00
categories: [war game]
tags: [bandit, overtherwire]
pin: true
---

# 리눅스 워게임 중 하나인 overthewire의 bandit

[overthewire](https://overthewire.org/wargames/)의 워 게임은

1. Bandit
2. Leviathan or Natas or Krypton
3. Narnia
4. Behemoth
5. Utumno
6. Maze
7. Vortex
8. Manpage
9. Drifter
10. FormulaOne

이 있고 그 중 이 페이지에서는 bandit을 다룰 것이다.

[overthewire-bandit](https://overthewire.org/wargames/bandit/)

## Bandit Information (bandit 소개)

![bandit information](https://github.com/oil-lamp-cat/oil-lamp-cat.github.io/assets/103806022/431a3132-838c-497d-8797-3a0b4e466d4c)

음... 한글로 번역해서 만들어봤는데 아무래도 가독성이 좋지 않으니 영어를 직접 해석하며 읽어보길 바란다. 컴퓨터와 영어는 땔 수 없는 관계인만큼 이렇게 틈틈이 공부하면 좋지 않을까?

![bandit information - ko](https://github.com/oil-lamp-cat/oil-lamp-cat.github.io/assets/103806022/eee3a091-23e6-4376-8625-dfdb101c9d30)

## bandit 접속

bandit을 접속하기 위해서는 ssh로 접속해야하며 shell로도 접속이 가능하지만 보통 putty나 termius를 사용하는 편이다. 나도 최근까지는 putty를 사용하고 있었지만 termius의 ui가 훨씬 더 보기 좋아 [termius](https://termius.com/)로 옮겼다. 아 참고로 나는 "Dracula" 테마를 사용하고 있다.

![termius-Hosts-page](https://github.com/oil-lamp-cat/oil-lamp-cat.github.io/assets/103806022/97224693-4e5c-42be-b23e-4ea745efa433)

처음 프로그램을 키게 되면 이런 화면이 보일 텐데 Group과 Hosts는 내가 몇가지 이미 만들어놔서 그런 것이니 이 화면에서 NEW HOST를 누르면 된다.

![캡처](https://github.com/oil-lamp-cat/oil-lamp-cat.github.io/assets/103806022/3ad9ec47-4e84-45f8-b036-14bdefe0d3e6)

위에서 부터 설명하자면

- Address : 우리가 접속할 IP 혹은 Hostname
- Label : (아직 이게 무엇인지 이해하지 못해 나는 flag저장용으로 쓰는 중이다.)
- Parent Group : 하나의 Group을 만들어 SSH Port를 저장해 놓았다가 사용할 수 있게 할 수 있다.
- Tags : 테그를 지정해 줄 수 있다.
- SSH on <> port : 접속할 Adress의 접속할 수 있는 port이다.
- Username : 접속할 username
- Password : 접속할 user의 password

일단 bandit을 하는 동안 사용할 것들은 이정도라고 생각한다. 고로 우리가 적어야 할 것들은

![bandit0](https://github.com/oil-lamp-cat/oil-lamp-cat.github.io/assets/103806022/462c0d60-8907-47e2-8556-e7af4652a82b)

이정도 되시겠다

Host: bandit.labs.overthewire.org<br/>
Port: 2220

## Bandit Level 0

![bandit0](https://github.com/oil-lamp-cat/oil-lamp-cat.github.io/assets/103806022/5454cf8c-d2d9-4149-aa41-ba383a7dec2f)

### 목표

이 레벨의 목표는 SSH를 사용하여 게임에 로그인하는 것입니다. 연결해야 하는 호스트는 포트 2220의 bandit.labs.overthewire.org 입니다. 사용자 이름은 bandit0이고 비밀번호는 bandit0입니다. 로그인이 완료되면 Level 1 페이지로 이동하여 Level 1을 이기는 방법을 알아보세요.

### 풀이

접속했다면 Level 0는 해결이다! 그럼 여기서 무엇을 찾아야할지 [Level 0 -> Level 1](https://overthewire.org/wargames/bandit/bandit1.html) 페이지로 가서 확인해보자!

## Bandit Level 0 -> Level 1

![bandit0_1](https://github.com/oil-lamp-cat/oil-lamp-cat.github.io/assets/103806022/dc9ad301-252f-4b22-802a-306b20854359)

### 목표

다음 레벨의 비밀번호는 home 디렉토리에 위치한 readme 라는 파일에 저장됩니다. 이 비밀번호를 사용하여 SSH로 bandit1에 로그인합니다. 레벨의 비밀번호를 찾을 때마다 SSH(2220 포트)를 사용하여 해당 레벨에 로그인하고 게임을 계속 진행합니다.

### 풀이

- 현재 본인이 어떤 디렉토리에 있는지 확인하기 위해 "pwd"명령어를 써주자.

- 그 후 "ls"명령어를 이용하여 어떤 파일들이 있는지 확인해보자.

- 아마 readme 파일이 존재할 것이다.

- 존재하지 않는다면 현재 디렉토리가 home이 아닐테니 "pwd" 명령어를 사용하여 확인해보자.

- readme 파일은 명령어로 읽어보기 위해서는 "cat" 명령어를 사용한다.

- 여기서 잠시 명령어들의 유래를 이야기해보자 아무래도 나는 이러는 편이 기억하는데 도움이 되었다.

- "pwd" 명령어는 "print working directory" 현재 일하고 있는 디렉토리 라는 의미이다.

- "ls" 명령어는 "list directory contents" 디렉토리안에 있는 디렉토리 및 파일 리스트를 출력한다.

- "cat" 명령어는 "concatenate"는 "연결하다"라는 뜻을 가지고 있는데 본래의 기능은 여러 파일의 내용을 하나로 합쳐주는 역할을 하지만 이번에 사용한 것처럼 파일의 내용을 단순 출력하여 확인하거나, ">" 이나 ">>" 같은 기호와 함께 사용하여 파일을 생성하고, 저장하는 용도로도 사용될 수 있다고 한다.

![bandit0_1_sol](https://github.com/oil-lamp-cat/oil-lamp-cat.github.io/assets/103806022/7821e92b-0987-4c0b-9244-31e2167cbe52)

여기 아래 보이는 NH2SXQwcBdpmTEzi3bvBHMM9H66vVXjL 이 바로 다음 게임의 비밀번호가 된다!

## Bandit Level 1 -> Level 2

user_id : bandit1<br/>
password : NH2SXQwcBdpmTEzi3bvBHMM9H66vVXjL (언제든 바뀔 수 있다.)

![bandit1_2](https://github.com/oil-lamp-cat/oil-lamp-cat.github.io/assets/103806022/d76a3a34-31d1-42eb-9c35-d473593df16f)

### 목표

다음 레벨의 비밀번호는 home 디렉토리의 '-'라는 이름을 가진 파일에 있다.

찾아보면 좋을 것들 :

- 구글에 "- 이름을 가진 파일" 이라고 검색해보세요
- (Advanced Bash-scripting Guide - Chapter 3 - Special Characters)[http://tldp.org/LDP/abs/html/special-chars.html]

### 풀이

- "pwd" 명령어로 현재 위치를 보니 home 디렉토리이다.

- "ls" 명령어로 리스트를 확인해보니 "-" 라는 파일이 있다.

- "cat -" 로 열어봐도 아무것도 나오지 않는다. 이럴 때에는 ctrl+c 혹은 ^c로 빠져나올 수 있다.

- "-" 라는 문자가 리눅스에서 예약된 즉 약속된 특수문자라 file, cat에서 인자로 넘겨받지 못한다. 고로 현재 경로에 있는 파일을 뜻하는 ./를 사용하기로 하자.

- "file ./-" 명령어를 통해 어떤 형식의 파일인지 읽어보자. [ASCII](https://namu.wiki/w/%EC%95%84%EC%8A%A4%ED%82%A4%20%EC%BD%94%EB%93%9C)(아스키코드) text 파일이라고 한다.

- "cat ./-" 명령어로 파일 내용을 출력해보자.

찾았다! rRGizSaX8Mk1RTb1CNQoXTcYZWU6lgzi

![bandit1_2_sol](https://github.com/oil-lamp-cat/oil-lamp-cat.github.io/assets/103806022/74168a24-1404-4139-8e37-539d92f09f05)
