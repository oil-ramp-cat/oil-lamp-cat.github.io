---
title: "[wargame] overthewire bandit"
date: 2024-02-01 00:25:15 +09:00
categories: [war game, Linux]
tags: [bandit, overtherwire]
pin: true
---

![overthewire](https://github.com/oil-lamp-cat/oil-lamp-cat.github.io/assets/103806022/d20228f5-ee18-43c4-834b-464c5215f6b3)

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

**Host**: bandit.labs.overthewire.org<br/>
**Port**: 2220

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

**user_id** : bandit1<br/>
**password** : NH2SXQwcBdpmTEzi3bvBHMM9H66vVXjL (언제든 바뀔 수 있다.)

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

## Bandit Level 2 -> Level 3

**user_id** : bandit2<br/>
**password** : rRGizSaX8Mk1RTb1CNQoXTcYZWU6lgzi

![bandit 2_3](https://github.com/oil-lamp-cat/oil-lamp-cat.github.io/assets/103806022/05e19646-795b-4af7-9c5b-d7b6e94d99fc)

### 목표

다음 레벨의 비밀번호는 home 디렉토리의 'spaces in this filename'에 담겨있습니다.

### 풀이

- home 디렉토리에서 ls명령어를 쳐보면 목표에서 보이듯 'spaces in this filename'이라는 파일이 있다.

- 띄어쓰기가 되어있는 파일을 읽을 때에는 '' 로 파일을 감싸주면 된다.

![bandit2_3_sol](https://github.com/oil-lamp-cat/oil-lamp-cat.github.io/assets/103806022/22611709-3c82-4419-80d6-433e030d3407)

비밀번호는 aBZ0W5EmUfAf7kHTQeOwd8bauFJ2lAiG 이다!

## Bandit Level 3 -> Level 4

**user_id** : bandit3<br/>
**password** : aBZ0W5EmUfAf7kHTQeOwd8bauFJ2lAiG

![bandit3_4](https://github.com/oil-lamp-cat/oil-lamp-cat.github.io/assets/103806022/827b6011-cbbf-45de-b938-ad5a49fafe34)

### 목표

다음 레벨로 가는 비밀번호는 **inhere** 딕셔너리 안에 숨겨진 파일 안에 있습니다.

### 풀이

- home 디렉토리의 inhere 딕셔너리로 들어가 보았을 때 'ls'명령어를 사용해 보면 아무 파일도 보이지 않는다.
- 목표에서 말했듯 inhere 딕셔너리에 숨겨진 파일이라고 하니 'ls -al' 명령어를 사용하여 모든 파일 보기를 하자.

- "ls" 명령어의 옵션 중 하나인 'ls -a'는 모든 파일을 보는 옵션이고 'ls -l' 명령어는 파일이 만들어진 시간을 볼 수 있게 해주는 옵션이다. 그리고 그 둘을 합친 옵션이 'ls -al'이다. 이번 문제를 해결하기 위해서 'ls -a' 만 해줘도 된다.

![bandit3_4_sol](https://github.com/oil-lamp-cat/oil-lamp-cat.github.io/assets/103806022/ae26e9d9-0a0b-49cd-9eb8-3a2bdef9c39e)

숨겨져 있는 비밀번호는 2EW7BBsr6aMMoJ2HjW067dm8EgX26xNe이다.

## Bandit Level 4 -> Level 5

**user_id** : bandit4<br/>
**password** : 2EW7BBsr6aMMoJ2HjW067dm8EgX26xNe

![bandit4_5](https://github.com/oil-lamp-cat/oil-lamp-cat.github.io/assets/103806022/37f951f2-9b84-4074-8471-b35aaac5321e)

### 목표

다음 레벨로 가는 비밀번호는 inhere 딕셔너리의 **"인간이 읽을 수 있는"** 파일로 저장되어있습니다.

Tip: 터미널이 더러워졌다면, "reset" 명령어를 사용하세요!

(저는 "reset" 보다는 "clear"를 사용하는 편이랍니다!)

- "reset" 명령어는 말 그대로 모두 원래대로 되돌리는 기능이고 "clear" 명령어는 터미널의 화면을 깨끗이 지우는 차이가 있다.

### 풀이

- home 디렉토리 안에 inhere 디렉토리가 있고 그 안에는 `-file\*` 파일들이 10개가 있다.
- 물론 이 파일들을 직접 열어보며 읽을 수 있나 없나 확인 할 수도 있겠지만 `file` 명령어를 이용해서 찾아보기로 하자
- **file ./\*** 명령어에서 ./\* 부분은 파일 안에 있는 모든 파일의 정보를 볼 수 있게해준다.
- 찾아보니 ./-file06번 파일이 **"ASCII text"** 로 되어있다고 한다.
- `cat` 명령어로 비밀번호를 찾았다! lrIWWI6bB37kxfiCQZqUdOIYfr6eEeqR

![bandit4_5_sol](https://github.com/oil-lamp-cat/oil-lamp-cat.github.io/assets/103806022/6bf0529c-dd5a-4c6d-bdf7-4f5a44296ff3)

## Bandit Level 5 -> Level 6

**user_id** : bandit5<br/>
**password** : lrIWWI6bB37kxfiCQZqUdOIYfr6eEeqR

![bandit5_6](https://github.com/oil-lamp-cat/oil-lamp-cat.github.io/assets/103806022/dcdc812c-c3b9-4a42-ae9e-06c57b7a37c4)

### 목표

다음 레벨로 가는 비밀번호는 inhere 디렉토리의 다음 조건을 충족하는 사항을 가지고 있다.

- 사람이 읽을 수 있다.
- 1033 byte의 크기를 가지고 있다.
- 실행파일이 아니다

### 해결법

- inhere 딕셔너리에서 ls 명령어를 쳐보자.

```bash
bandit5@bandit:~/inhere$ ls -a
.            maybehere04  maybehere10  maybehere16
..           maybehere05  maybehere11  maybehere17
maybehere00  maybehere06  maybehere12  maybehere18
maybehere01  maybehere07  maybehere13  maybehere19
maybehere02  maybehere08  maybehere14
maybehere03  maybehere09  maybehere15
```

어우 하나하나 찾기에는 역시 조건을 준 이유가 있었다.

- 그렇다면 이번에는 **'find'** 명령어를 사용하기로 하자
- **"find . -size 1033c"** 명령어를 이용하여 1033 byte의 크기를 가진 파일을 찾아낼 수 있다.

```bash
bandit5@bandit:~/inhere$ find . -size 1033c
./maybehere07/.file2
```

- "cat" 명령어를 이용하여 ./maybehere07/.file2 파일을 읽어보면 비밀번호를 찾을 수 있다!

찾았다! : P4L4vucdmLnm8I7Vl7jG1ApGSfjYKqJU

![bandit5_6_sol](https://github.com/oil-lamp-cat/oil-lamp-cat.github.io/assets/103806022/f6977bc4-e750-4be2-89fa-5dbe4be55a3f)

## Bandit Level 6 -> Level 7

**user_id** : bandit6<br/>
**password** : P4L4vucdmLnm8I7Vl7jG1ApGSfjYKqJU

![bandit6_7](https://github.com/oil-lamp-cat/oil-lamp-cat.github.io/assets/103806022/b36e65f3-e0a5-4abb-b40f-4fa1b5a06468)

### 목표

다음 레벨로 가는 비밀번호는 다음 조건을 충족시키며 서버의 어딘가에 있다.

- bandit7 유저가 가지고 있다.
- bandit6 그룹에 있다.
- 33 byte의 크기를 가지고 있다.

- user == bandit7
- group == bandit6
- size == 33 byte

음... 지금까지랑은 다르게 서버의 어딘가에 있다고 한다.

### 해결법

힌트에서 준 대로 find 명령어를 이용하여 비밀번호를 찾아가보자.

```shell
bandit6@bandit:~$ find / -user bandit7 -group bandit6 -size 33c
```

결과를 보기 전 잠시 옵션들에 관해 알아보자.

- `/`는 서버에서 최상위 경로를 의미한다. 시스템 전부를 토대로 찾겠다는 의미이다.
- `-user`는 파일의 소유자(user)를 찾는다.
- `-group`은 파일이 속한 그룹을 찾습니다.
- `-size`는 Level 5-6에서 사용했던 것처럼 파일의 사이즈를 찾습니다. 그 중 `c`는 `bytes`를 의미합니다. 

어..음.. 결과가 너무 많이 나왔다. 그것도 그중 "Permission denied" 즉 접근 권한 없음 에러가 매우 많이 나왔다.

```shell
find: ‘/etc/ssl/private’: Permission denied
find: ‘/etc/polkit-1/localauthority’: Permission denied
find: ‘/etc/sudoers.d’: Permission denied
find: ‘/etc/multipath’: Permission denied
find: ‘/root’: Permission denied
find: ‘/var/lib/update-notifier/package-data-downloads/partial’: Permission denied
find: ‘/var/lib/amazon’: Permission denied
/var/lib/dpkg/info/bandit7.password
find: ‘/var/log’: Permission denied
find: ‘/var/cache/private’: Permission denied

--중략--

find: ‘/run/systemd/propagate’: Permission denied
find: ‘/run/systemd/unit-root’: Permission denied
find: ‘/run/systemd/inaccessible/dir’: Permission denied
find: ‘/run/lock/lvm’: Permission denied
```

이 긴 결과를 읽다보면 중간에 `/var/lib/dpkg/info/bandit7.password` 라는 파일에 접근이 가능하다는 결과를 볼 수 있다. 

하지만 우리는 좀 더 명령어를 이용하여 깔끔하고 빠른 방법으로 찾아보도록 하자.

---

위 문제를 해결하기 위해 우리는 `File Descriptor`라는 것에 대해 잠시 알아보자.

파일 디스크립터(File Descriptor)란 리눅스 혹은 유닉스 계열의 시스템 프로세스가 파일을 다룰 때 사용하는 개념으로 파일에 접근할 때 할당되는 값이다. 그 값들의 뜻은 다음과 같다.

- 0 : 표준 입력 , `Standard Input`
- 1 : 표준 출력 , `Standard Output`
- 2 : 표준 에러 , `Standard Error`

---

이제 우리는 오류 즉 `Permission denied`가 어떤 값을 가지게 되는지 알았고(`2`), 그럼 이 오류가 출력되지 않게 하려면 어떻게 할 지에 관해 생각해보자.

---

### I/O Redirection

 직역하자면 Input/Output의 재방향화이다. 입력과 출력의 방향을 바꿔 모니터 출력이 아닌 파일에 저장하거나 파일을 출력하는 뱡향을 바꾸는 식으로 사용할 수 있다. 나중에 이것에 관해 좀 더 깊게 다뤄봐야 할 듯 하지만 일단 간단히 적어보자면.

 - A > B : A의 결과를 B로 `보낸(저장)`다.
 - A >> B : A의 결과를 B의 데이터에 `추가`한다.
 - A < B : B의 데이터를 A에 `입력`한다.

그렇다면 우리는 `File Descriptor`를 통해 `2`번인 것을 알았고, `Redirection`을 통해 그 출력을 다른 어딘가로 보낼 수 있다는 것을 알았다.

그렇다면 이번엔 `리눅스의 블랙홀`이라고 불리는 곳으로 오류들을 보내버리면! 우리가 원하는 값들을 얻을 수 있을 것이다.

> `/dev/null` : 리눅스의 블랙홀 이라고도 불리는 이곳은 경로에 나와 있듯 `null`값을 가져 아무것도 남지 않는 공간이다.

> `2 > /dev/null` : 표준에러를 `/dev/null`로! 

---

그럼 이제야말로 정말 비밀번호를 찾아내보자.

```shell
bandit6@bandit:~$ find / -user bandit7 -group bandit6 size 33c 2>/dev/null
/var/lib/dpkg/info/bandit7.password

bandit6@bandit:~$ cat /var/lib/dpkg/info/bandit7.password z7WtoNQU2XfjmMtWA8u5rN4vzqu4v99S
```

비밀번호 : z7WtoNQU2XfjmMtWA8u5rN4vzqu4v99S

![bandit6_7_sol](https://github.com/oil-lamp-cat/oil-lamp-cat.github.io/assets/103806022/aa431231-ee86-4059-8846-35483ffba8a9)

사실 난 리눅스에 관한 지식, 명령어에 관한 지식 없이 시작하였기에 여기저기 수많은 블로그들을 참고하여 작성하고 있는 중이다. 아마 이미 이러한 지식을 알고있는 사람이라면 여기까지 매우 쉽게 도달했을 것이라 생각한다! 차근차근 나아가자! :D

## Bandit Level 7 -> Level 8

**user_id** : bandit7<br/>
**password** : z7WtoNQU2XfjmMtWA8u5rN4vzqu4v99S

![bandit7_8](https://github.com/oil-lamp-cat/oil-lamp-cat.github.io/assets/103806022/2b5b9153-6a88-4cb1-a0ff-3a78cadecde9)

### 목표

다음 레벨로 가는 비밀번호는 `data.txt`라는 파일 안에 `millionth`라는 단어 옆에 있다.

### 해결법

일단 어떤 식으로 파일이 이루어져 있나 보기 위해 파일을 읽어봤다.

```shell
bandit7@bandit:~$ cat data.txt

--중략--

summation       u50rQF4SGWx3gtytGjZL9oaF5Pqcf3yi
Israelites      d5RJ2L7p0yKa9RpCr7dU9dhuKq46dFVC
volatility's    wqQvrK0UVKfx2BtyFogXhTyGI0V4BOS5
Albany's        GopEzFC069enA4p224dn1i0Ire3HP9jx
indecisively    tOcTUgSwpbShx5J5x6xmmIQKIlG1GTJz
choosey 3u6Ir3tT1Vq7q17aWIvdT14IxT0f1y5X
velveteen's     46soI2oXIiX1HuXtDXeRLlFrctbGJTYO
```

어우.. 좀.. 너무 많다 이걸 하나하나 읽어가면서 찾을 수도 있고 혹은 notepad에 옮긴 뒤 ctrl+F로 찾을 수도 있지만 우리는 명령어를 사용하기로 하자.

그 중 우리는 `grep`이라는 명령어를 사용할 것이다.

> grep [옵션] [표현식] [파일명] : 이렇게 파일 속 특정 문자열(혹은 표현식)을 찾습니다.

```shell
bandit7@bandit:~$ grep millionth data.txt
millionth       TESKZC0XvTetK0S9xNwm25STk5iWrBvP
```
찾았다!

---

혹은 `cat`명령어와 `grep`명령어를 섞어 만들 수도 있다.

`cat data.txt | grep millionth` 이런식으로!

```shell
bandit7@bandit:~$ cat data.txt | grep millionth
millionth       TESKZC0XvTetK0S9xNwm25STk5iWrBvP
```

같은 결과가 나온다.

---

비밀번호는 `TESKZC0XvTetK0S9xNwm25STk5iWrBvP`이다.

## Bandit Level 8 -> Level 9

**user_id** : bandit8<br/>
**password** : TESKZC0XvTetK0S9xNwm25STk5iWrBvP

![bandit8_9](https://github.com/oil-lamp-cat/oil-lamp-cat.github.io/assets/103806022/c12b9a02-2f91-4be3-92bb-52697a83a962)


### 목표

다음 레벨로 가는 비밀번호는 `data.txt`에 있고 딱 `한 줄만 나타난다`고 한다.

중복되는 것을 제거하거나 딱 한번만 나오는 것을 찾으면 될 것 같다.

### 해결법

이번에는 `uniq`라는 명령어를 이용해보자.

#### uniq

`**uniq**`명령어는 **연속적으로 반복되는 행을 한 행으로 줄여주는** 명령어이다. 특히나 `uniq`명령어는 `sort`명령어와 대부분 같이 쓰이게 되는데, sort 명령어는 정렬을 해주기 때문에 한 행이 몇번 반복되는지 한눈에 알 수 있게 해주기 떄문이다. 그리고 그 두 명령어를 같이 쓰려변 저번에 `cat`명령어와 `grep`명령어를 같이 쓸 때 사용한 `|`(파이프)를 써야한다.

> uniq [옵션] [파일 이름]

옵션을 사용하지 않으면 

```shell

bandit8@bandit:~$ cat data.txt | sort | uniq
18DyjwhN856SsMx8bNrFSvr6rJxNQKhE
1iyGemEgn3qUOOFcAJyGPHOiewqZyp1y
2CQ5DQRdtoe9Ft8YpMHqCwQcN1Bk9lCI
365RauAVsFlxktPMpoLtIf1uxijU1TfV
4K2MoVHd1gXfoOdDjvlaRxFNZwmi4A4C
52p0CnGhAvm4m3fPKqz9mTxVDeVYCvnG
5Y76FifuxKStZi4CVovF2uPhgLrZnLzG
7A4l2BI3lPJgNdWAmyXAGlfB8uvCQLX0
8cxarYi5VoKRj3lzo2baLOJaMgUtzoRH
97Qwmy18JE8aGIud1stpTsOrOtUMHeGI
9d8exmGtSsGcU1gz6HmqTfSxmnmI4FBo
A16BW831T94qcsYcGDSkgzYhxnX2xUdK
aAd8RbcAAGVRifo0gE2x1nPIGH2fjgZi

--중략--

```
이런식으로 문자열 단위로 정렬되어 보이게 된다. 하지만 우리는 한번만 나오는 문자열이 필요하기에 옵션을 추가해줘 비밀번호를 찾아낼 것이다.

#### uniq 옵션들 - options

> 내가 자주 쓰게 될 것들만 일단 적어본다.

##### -c

연속적으로 반복된 수 만큼 행 앞에 표시된다.

```shell

bandit8@bandit:~$ cat data.txt | sort | uniq -c
     10 18DyjwhN856SsMx8bNrFSvr6rJxNQKhE
     10 1iyGemEgn3qUOOFcAJyGPHOiewqZyp1y
     10 2CQ5DQRdtoe9Ft8YpMHqCwQcN1Bk9lCI
     10 365RauAVsFlxktPMpoLtIf1uxijU1TfV
     10 4K2MoVHd1gXfoOdDjvlaRxFNZwmi4A4C
     10 52p0CnGhAvm4m3fPKqz9mTxVDeVYCvnG
     10 5Y76FifuxKStZi4CVovF2uPhgLrZnLzG
     10 7A4l2BI3lPJgNdWAmyXAGlfB8uvCQLX0
     10 8cxarYi5VoKRj3lzo2baLOJaMgUtzoRH
     10 97Qwmy18JE8aGIud1stpTsOrOtUMHeGI
     10 9d8exmGtSsGcU1gz6HmqTfSxmnmI4FBo

    --중략--

```

이렇게 숫자로 표기된 곳에서 혼자 1로 표시된 문자열을 찾을 수도 있고 다음 옵션을 이용하여 찾을 수도 있다.

##### -u

연속적으로 반복되지 않는 행만 출력한다.

```shell

bandit8@bandit:~$ cat data.txt | sort | uniq -u
EN632PlfYiZbn3PhVK3XOGSlNInNE00t

```

오호 이 녀석이 우리가 찾던 비밀번호다!

비밀번호 : `EN632PlfYiZbn3PhVK3XOGSlNInNE00t`

## Bandit Level 9 -> Level 10

**user_id** : bandit9<br/>
**password** : EN632PlfYiZbn3PhVK3XOGSlNInNE00t

![bandit9_10](https://github.com/oil-lamp-cat/oil-lamp-cat.github.io/assets/103806022/4744e711-77f3-466b-bb3f-0dff4f3cf79f)



### 목표

다음 레벨로 가는 비밀번호는 `data.txt` 파일 안에 `human-readable` 즉 사람이 읽을 수 있는 문자열로 몇개의 `=` 문자 뒤에 나온다.

### 해결법

한번 `grep`명령어로 `=`문자열을 찾아볼까요?

![bandit9_10_grep_result](https://github.com/oil-lamp-cat/oil-lamp-cat.github.io/assets/103806022/4a5bc229-1e2e-4a10-8755-a7da146f5695)

이런 사람이 읽을 수 있는 텍스트(text)파일이 아니라 바이너리(binary: 데이터의 저장과 처리를 목적으로 0과 1의 이진 형태로 인코딩된 파일)이다보니 grep 명령어를 사용할 수 없다고 한다.

`grep`명령어의 옵션 중에는 `-a`옵션이 있는데 이 옵션이 바로 바이너리를 텍스트 파일처럼 처리해주는 옵션이라고 합니다.

```shell

bandit9@bandit:~$ cat data.txt | grep -a "=="
"x����$�WI��G���gq��B�ș�q6�p�|r9����K�|��[�l������#0jb�7u��uf`e�Y�o�    ?>�?���F��){�x%�1�c3    ��l�XQz�U0��
                                                                                                            �O-@����E�8�jr��i��$�]�:[������x]T========== theG)"�)<�!G��{��X8�?��5�&���O:�@N��s~�i\6�Xz     ����==��S�igy����>�m���P�I�ݰ���C+x�[ڋ�e22�a�
�{��\nF���<��D� �M�f*I�`�@oݗn�ײ�p�n��Y�����x�n�����ۓ�4j\9̷�n:�J�Va���K��+        ���f��Z9�)��Q�Ѻ?��r��.���O[��6�^FS�========== passwordk^�ټ����1>���jg�S�>�����5~�I�-�"��G|�����6L�"�� �T�'����j�T=������2_�q+T6(�p
                                                                                               '
�T����T�p#fz:�LJ�r*;�o#�E�hϻ��/]��&x��:!�Oη��[%ug�Z��hݹkj��D����ZH���Ed����ZE������4K�?�p������z�iw?������J������^`t d�;T��ڪ`��M�T�/�j�M�Ł��$����O�Msh��������8��========== is�w
.�Ze�x��J���i��_qhlbw���X�yϪ�.�-3������7�T�����             Ke@�&,]D���%�v�
:��g����yV�M]��Vf��̤��i�L�u#�&ZR�7٬.׳?L6>�D�GC�:jЂ��}��=��T�k�@I]j5�}0Ļ�_�zW5E��f3c�ᦩ�a��Ozݏ������:�Dr%[+��*2lk���X�齲g/$�%��g�Z"g��x�
A�tL[�#��Ƴ���wv..ک!�5��nH�mwQ!2��Fu�cp��B�;ݍ�UH�NEz+�w�'�w$F��6�L�l��ܤ
�(���k�}�N l�E����*�9�<u2����_0�Y���4�`�w�j:6�ek�{OXr���/��?d�mH�m�3�EM`�om�x��۽�>���Ihe�X��N�tz�xb�(X�1��oPZ�ɱ�ݿQ��{�%�xV��۵G�{TbJ;=l��J�Q�/��,��??r)SՒ98UU��jߗc-�?4���6�[�$�J��S��ۿv��P��u$��4+�2ʾ�$�oI�[=lI���a6�ȁ
                                                                                                  ×/%Â���w�����l��eu-)E��f�n���%����Ƙ�p��~��L�]wPP�J��O1#�{"@Ip�I�2��Di�����T�m�Gf�rSa
���LN�|�r1�7�6��8������<D�_o��kV�Jx�#���[�����딭��E�����O.x�s�薲�Ҋ����I��t�.�RF�L�V¡G�D�5c
                                                                                          %�0d������^b�C�EX��I��,S߫��)o��8/�-��ďRj$�e0G�rx70yJ{��|Ƞ��ÒX     5��a���C�2����{�5���8+�dm)��Z�R��
                                                                         |�q���E�C��[�qX2R^{X���҉�WH     fU�����B�|�������j��n�c����`����yq��       ����������E�0�Mhr��Xw�<Ͱ�[      �N�iޑ2KYl�A���)�#pJ_�)�]�PJoZP�EW��-"�ְ��D��Yq5� �G^xu�,-ҏ���i�o}2��j��Ֆ�����[:ˇ52��)"t     jh�y��========== G7w8LIi6J3kTb8A7j9LgrywtEUlyyp6s

```

음 딱 보아하니 맨 아래 있는 `G7w8LIi6J3kTb8A7j9LgrywtEUlyyp6s`가 비밀번호 인듯 하지만 우리는 이것보다 더 깔끔한 결과를 찾아보도록 하죠.

위 문제를 해결하기 위한 방법은 우리가 보통 파일을 읽을 때에는 `cat`명령어를 사용하였지만 이번에는

> strings : 파일의 문자열을 출력

`strings` 명령어를 사용해 보도록 하겠습니다. 이렇게 함으로써 문제의 힌트였던 `human-readable` 텍스트만 출력되게 될 것이다.

![bandit9_10_sol](https://github.com/oil-lamp-cat/oil-lamp-cat.github.io/assets/103806022/71337889-8e96-4163-af94-f2466eda7b22)

이제야 모든 조건을 사용하면서 비밀번호를 찾아냈다! 

`the password is G7w8LIi6J3kTb8A7j9LgrywtEUlyyp6s`

비밀번호 : `G7w8LIi6J3kTb8A7j9LgrywtEUlyyp6s`

## Bandit Level 10 -> Level 11

**user_id** : bandit10<br/>
**password** : G7w8LIi6J3kTb8A7j9LgrywtEUlyyp6s

### 목표

![bandit10_11](https://github.com/oil-lamp-cat/oil-lamp-cat.github.io/assets/103806022/261b6ab8-2fa7-4422-b3b1-71dac8ab6d0e)

비밀번호는 `data.txt`파일에 `base64`형태로 인코딩되어있다.

### 해결법

> `base64` : 바이너리 데이터를 문자 코드에 영향을 받지 않는 공통 ASCII 문자표로 표현하기 위해 만들어진 인코딩이다. ASCII 문자 하나가 64진법의 숫자 하나를 의미하기 때문에 Base64라는 이름을 가졌다고 한다.

인코딩의 반대말은? 디코딩!

`base64` 명령어를 사용하여 디코딩해 봅시다.

![bandit10_11_sol](https://github.com/oil-lamp-cat/oil-lamp-cat.github.io/assets/103806022/942c774b-8f40-4ec7-8b46-57cd8c2ca000)

비밀번호 : `6zPeziLdR2RKNdNYFNb6nVCKzphlXHBM`

## Bandit Level 11 -> Level 12

**user_id** : bandit11<br/>
**password** : 6zPeziLdR2RKNdNYFNb6nVCKzphlXHBM

### 목표

![bandit11_12](https://github.com/oil-lamp-cat/oil-lamp-cat.github.io/assets/103806022/d1b5ee20-b32d-47eb-bcd5-16a43e7f6b3c)

비밀번호는 `data.txt`파일에 대소문자가 13자리씩 회전되어있다고 한다. 

- 카이사르 암호
> ROT13 : 문자를 알파벳 뒤에 13 번째 문자로 대채하는 대체 암호.

### 해결법

```shell
bandit11@bandit:~$ ls
data.txt
bandit11@bandit:~$ cat data.txt
Gur cnffjbeq vf WIAOOSFzMjXXBC0KoSKBbJ8puQm5lIEi
```

여기 나온 결과를 카이사르 암호 풀이 사이트를 이용해서 풀어도 된다. 하지만 이번에는 `tr`명령어를 사용해 보기로 한다.

---

#### tr(translate) 명령어

특정 문자를 치환하거나 삭제할 수 있다.

`tr [option] [문자열1] [문자열2]` 형태로 사용할 수 있다.

- 소문자 -> 대문자 : `tr a-z A-Z`, `tr '[:lower:]' '[:upper:]'`, `tr '[a-z]' '[A-Z]'등의 형태로 사용할 수 있다.

---

우리는 `ROT13`된 문자열이 필요하다. 또한 대,소문자 모두 이동해야 하기에 범위 지정을 할 때에 대소문자 모두 기입해야 한다.

```shell
bandit11@bandit:~$ cat data.txt | tr 'A-Za-z' 'N-ZA-Mn-za-m'
The password is JVNBBFSmZwKKOP0XbFXOoW8chDz5yVRv
```

비밀번호 : `JVNBBFSmZwKKOP0XbFXOoW8chDz5yVRv`

## Bandit Level 12 -> Level 13

**user_id** : bandit12<br/>
**password** : JVNBBFSmZwKKOP0XbFXOoW8chDz5yVRv

### 목표

![bandit12_13](https://github.com/oil-lamp-cat/oil-lamp-cat.github.io/assets/103806022/bd856e0b-0860-41b0-84d5-a5b08b9c6387)

다음 레벨로 가는 비밀번호는 **여러번 압축된** `hexdump` 파일인 `data.txt`에 있다. 이번 레벨을 위해서는 `mkdir`을 이용해서 `/tmp` 디렉토리 아래 파일을 만들어서 문제를 푸는 것이 좋다. 예를들어: `mkdir /tmp/myname123`. `cp`명령어를 사용해서 datafile을 옮기고 `mv`명령어를 사용해서 이름을 다시 지어라.(manpage를 읽어봐!)

### 해결법

모르는 명령어가 몇개 더 추가되었군요. 한번 정리해 봅시다.

---
#### mkdir (make directory)

디렉토리(폴더)를 생성할 때 사용하는 명령어입니다.

> `mkdir [옵션] [생성할 디렉토리]`

예를 들어 `호롱고양이의 폴더`를 만든다고 해봅시다.

```mkdir lampcat_folder``` 이렇게 명령어를 이용하면 현 위치를 Default(기본)으로 하여 지금 위치에 폴더를 만들게 됩니다.

그렇다면 이번에는 **/home/** 위치에 폴더를 생성한다고 해봅니다.

```mkdir /home/lampcat_folder``` 이런식으로 폴더를 생성할 수 있다.

##### options (옵션)

- -m : 디렉토리를 생성할 때 권한을 설정합니다. (아직 이해가..)
- -p : 상위 경로도 함께 생성합니다. (위 예시에서 만약 `/home`경로가 없다면 추가적으로 생성하게 됩니다)
- -v : 디렉토리를 생성하고 생성된 디렉토리에 대한 메시지를 출력합니다.

---
#### cp (copy)
 
 리눅스 혹은 유닉스 환경에서 파일 혹은 디렉토리를 복사할 때 사용합니다.

 ```shell
 cp [옵션] [복사 대상 디렉터리or파일] [복사될 디렉터리or파일]
 ```

 ##### options (옵션)

 - -r : 하위 디렉토리까지 모두 복사
 - -i : 복사될 파일의 이름이 이미 존재한다면
 - -v : cp 명령어를 수행하면서 복사 진행 상태를 출력한다.
 - -f : 복사 파일이 이미 그 위치에 있다면 파일을 지우고, 강제로 복사한다.
 - -p : 파일 혹은 디렉토리를 복사할 때 복사 대상의 소유자(계정), 그룹, 권한등의 정보까지 복사한다.

마지막 옵션 `-p`를 사용하게 되면 권한까지 복사하게 되므로 원래 `permission denied` 되던 문제를 해결하기 위해 우리가 mkdir로 폴더를 옮겨 작업하던 것이 의미 없게 된다! 

---

#### mv (move)

> `mv [옵션] [원본 파일] [변경하려고 하는 대상, 대상의 위치]`

원래는 디렉토리를 이동시킬 때 사용하는 명령어이다.

##### options (옵션)

- -b : 이동시킬 파일이 이미 존재하면 백업 파일을 만든다.
- -i : 이동시킬 파일이 이미 존재하면 덮어쓰기 여부를 묻는다.
- -f : 이동시킬 파일이 이미 존재하면 강제로 덮어쓰기 한다.
- -n : 이동시킬 파일이 이미 존재하면 덮어쓰기 하지 않는다.
- -r : 하위 디렉토리까지 모두 이동한다.
- -v : 이동 진행상황을 출력한다.

그런데 우리는 이것을 이용하여 대상의 확장자를 변경하는 작업을 할 것이다.

---

이것만 있으면 될 줄 알았는데 압축을 해결하기 위해 추가적인 명령어가 필요했다.

#### xxd

파일 혹은 표준 입력으로부터 `hexdump`를 만들거나 복원해 주는 명령어로, 바이너리 형식에서 `hexdump`를 만들 수 있다.

> Hex dump?

hex dump는 램 또는 파일이나 저장장치에 있는 컴퓨터 데이터의 십육진법적인 보임새이다. 데이터의 hex dump를 보는 것은 주로 디버깅이나 리버스 엔지니어링의 한 부분이다. hex dump에서, 각 바이트는 2 숫자의 16진법 수로 표현된다. 

라고 위키백과에서 말했다. 글로만 봐서는 무슨 말인지 이해가 안가 직접 `cat`명령어로 열어보기로 했다.

![bandit12_13_hexdump](https://github.com/oil-lamp-cat/oil-lamp-cat.github.io/assets/103806022/a02f98f8-c948-4065-9a05-65871a64eb9b)

어우.. 이게 그 hex dump라는 것인 듯 하다. 나중에 `Hex dump`에 관해 좀 더 찾아봐야겠다.

그럼 이제 다시 xxd로 돌아와서 (일단은 쉬운 것만 적어놓겠다.)

- -i : C언어에서 사용가능한 형식으로 출력
- -E : 오른쪽 문자열을 ASCII에서 *EBCDIC로 변경
- -r : 16진수 데이터를 바이너리 데이터로 변환
- -u : hex 를 소문자 대신 대문자로 출력

#### bzip2

`gzip`보다 높은 압축률을 갖는 압축방식을 사용한다.

- bzip2 압축 해제
```shell
bzip2 -d data.bz2
```

- bzip2 압축
```shell
bzip2 data.tar
```

- bzip2 압축 파일 정보 출력
```shell
bzip2 data.bz2
```

- bzip2 압축 파일 내용 출력
```shell
bzcat data.bz2
```

#### gzip

`gzip`은 60~70%의 압축률을 갖습니다. `.gz`, `.tar`, `.bz2`에 관한 내용은 좀 더 찾아봐야겠다.

- gzip 압축 해제
```shell
gzip -d data.gz
```

- gzip 압축
```shell
gzip data.tar
```

- gzip 압축 파일 정보 출력
```shell
gzip -l data.gz
```

- gzip 압축 파일 내용 출력
```shell
zcat data.gz
```

---

#### tar (Tape Archive)

여러 파일을 하나의 파일로 묶어주는 명령어이다.

압축을 해주는 명령어인 `gzip`, `bzip2`와 다르게 `tar`명령어는 단순히 파일을 하나로 묶는 명령어이다. 하지만 여러 옵션을 추가하여 압축까지 가능하게 해준다.

- -c (create) : 파일을 하나로 합친다.
- -v (verbose) : `tar`의 과정을 보여준다.
- -f (file) : 압축파일의 이름은 지정해준다.
- -x (extract) : 압축을 해제한다.
- -z (gzip) : `gzip`형태로 압축한다. (.gz)
- -j (bzip2) : `bzip2`형태로 압축한다. (.bz2)
- -t (list) : 파일의 리스트를 확인할 수 있다.

---

그럼 이제 정말로 비밀번호를 찾아보자.

![bandit12_13_1](https://github.com/oil-lamp-cat/oil-lamp-cat.github.io/assets/103806022/de13c208-2fa4-4875-930f-018eb64cd919)

아까 했던대로 `data.txt`파일을 열어보자 hex dump파일이 나온다.

그리고 그 파일을 `xxd` 명령어를 풀어보면?

![bandit12_13_2](https://github.com/oil-lamp-cat/oil-lamp-cat.github.io/assets/103806022/6c997608-9d17-489b-95d6-62f7a43f64f2)

아.. permission denied 접근 권한 없음 오류가 나온다. 그리고 우리는 사실 이 오류를 예상했었다. 바로? 목표에서!

 `mkdir`명령어와 `cp`명령어를 이용해서 디렉토리를 만들고 파일을 복사해서 문제를 풀어보라고 한다. `cp` 명령어를 사용 할 때에 `-p` 명령어가 권한까지 복사하는 것을 보아 그냥 `cp` 명령어를 사용하였을 때에 권한이 복사되지 않기에 변경할 수 있는 것으로 보인다.

`mkdir`명령어를 사용해서 `lampcat`이라는 디렉토리를 만들면?

![bandit12_13_3](https://github.com/oil-lamp-cat/oil-lamp-cat.github.io/assets/103806022/023b9952-54af-47fb-8127-66841620428c)

아까 테스트 해 본다고 디렉토리를 만들었었더니 아직 서버에 남아있는 듯 하다. 그럼 다른 이름으로 시도해 보면 된다.

![bandit12_13_4](https://github.com/oil-lamp-cat/oil-lamp-cat.github.io/assets/103806022/f1a65e2d-b527-4530-af2c-03ad849e2224)

이번에는 디렉토리를 만드는데 성공했다. `cp`명령어로 만들어둔 디렉토리에 data.txt를 복사한 뒤 `xxd`명령어로 바이너리 데이터로 되돌려보자.

![bandit12_13_5](https://github.com/oil-lamp-cat/oil-lamp-cat.github.io/assets/103806022/16634f27-9001-42f0-9c11-409bc8cb3bf5)

보아하니 `data`라는 파일이 복원된 파일이다. file 명령어로 `data`의 상태를 읽어보니 `gzip`의 형태로 압축되어있다고 한다.

```shell
bandit12@bandit:/tmp/oillampcat$ file data
data: gzip compressed data, was "data2.bin", last modified: Thu Oct  5 06:19:20 2023, max compression, from Unix, original size modulo 2^32 573
```

그렇기에 우리는 `data`파일을 `mv`명령어를 사용하여 `.gz`형태의 확장자로 바꿔준 뒤 압축을 해제할 것이다.

![bandit12_13_6](https://github.com/oil-lamp-cat/oil-lamp-cat.github.io/assets/103806022/552a4b64-e82b-4dad-9f19-148315d56913)

그리고 돌고돌아 다시 `data`파일을 마주하였다. 그리고 이번에는 bzip의 형태로 되어있다고 한다. 그럼 또 풀어야겠지?

![bandit12_13_7](https://github.com/oil-lamp-cat/oil-lamp-cat.github.io/assets/103806022/e30dc8bb-1c1b-492f-93ad-7566431b022c)

`mv`명령어로 bz2확장자로 변경하고, `bzip2`명령어로 압축 해제하면 또또 `data`파일은 만나게 된다.

![bandit12_13_8](https://github.com/oil-lamp-cat/oil-lamp-cat.github.io/assets/103806022/ed6a1341-c251-46f0-9d02-d58be2557ef9)

그렇게 한번 더 `gzip`형태를 풀게되면, 이번에는 `tar`형식으로 압축되어있다고 한다.

![bandit12_13_9](https://github.com/oil-lamp-cat/oil-lamp-cat.github.io/assets/103806022/addece31-c3d5-4665-90f6-da78920e8771)

`tar`로 압축된 `data`파일은 이번에는 `tar`명령어의 `-x (압축해제)`, `-v(압축 과정 출력)`, `-f(압축파일 이름 지정)` 옵션을 이용해 풀어주었다.

![bandit12_13_10](https://github.com/oil-lamp-cat/oil-lamp-cat.github.io/assets/103806022/029256c9-3d9c-4f18-85fe-65e722a770c8)

이번에는 웬일로 확장자를 붙인 채로 `data5.bin`이라는 이름을 갖고 압축이 해제되었다.