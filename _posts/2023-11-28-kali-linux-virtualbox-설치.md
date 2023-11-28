---
title: kali linux virtualbox 가상환경 다운로드
date: 2023-11-28 14:52:15 +09:00
categories: [kalilinux, virtualbox]
tags: [download, 설치]
pin: true
---

>보통은 usb에 설치하는 방법을 이용해서 사용하고 있었으나, 책을 읽으며 공부하다 [virtualbox](https://www.virtualbox.org/)를 이용하기로 했다.

# 1.virtual box 설치
[Oracle VirtualBox](https://www.virtualbox.org/) 사이트에 들어가서 [다운로드](https://www.virtualbox.org/wiki/Downloads)에 들어가 본인의 컴퓨터 사양에 맞는 링크를 클릭해 설치하기 바란다.
![스크린샷](https://github.com/oil-lamp-cat/oil-lamp-cat.github.io/assets/103806022/0c72712d-755e-4841-80d2-39cfd1b7811e)
내 컴퓨터는 windows에 설치할 것이기에 windows hosts를 눌러 설치 파일을 다운받았다.

 실행하기 전에 맞는(변조되지 않은) 설치 파일인지 확인하기 위해 [checksum](https://oil-lamp-cat.github.io/posts/window-checksum/)을 이용해 변조되었는지 확인을 하기 바란다.
 ![스크린샷](https://github.com/oil-lamp-cat/oil-lamp-cat.github.io/assets/103806022/86cf8b56-24f5-4d16-958a-a1fe678b388f)

이제는 그저 next만 누르면 virtualbox를 설치할 수 있기에 굳이 필요 없지만 그래도 모르겠다 하는 사람을 위해 아래 사진을 첨부해 놓았다.
 <details><summary>설치 과정</summary>
 <div markdown = "1">

![스크린샷](https://github.com/oil-lamp-cat/oil-lamp-cat.github.io/assets/103806022/bc915ca7-8b42-4df6-8d29-f872acbbb8a6)
Next

![스크린샷](https://github.com/oil-lamp-cat/oil-lamp-cat.github.io/assets/103806022/4c2ee854-905e-4a21-ab03-4573df675c3d)
Next

![스크린샷](https://github.com/oil-lamp-cat/oil-lamp-cat.github.io/assets/103806022/cfbd9f73-0480-48ac-90a4-c3a069d8ab8c)
Yes

![스크린샷](https://github.com/oil-lamp-cat/oil-lamp-cat.github.io/assets/103806022/7e678b0b-fbc2-4cb8-a312-5a5ffed67f3a)
Yes

![스크린샷](https://github.com/oil-lamp-cat/oil-lamp-cat.github.io/assets/103806022/af62caa8-917d-4803-95b6-57bbe69dc8a0)
Install

![스크린샷](https://github.com/oil-lamp-cat/oil-lamp-cat.github.io/assets/103806022/ddf1b4a1-f7a9-45e1-8662-96b16265f2cc)
설치 대기

![스크린샷](https://github.com/oil-lamp-cat/oil-lamp-cat.github.io/assets/103806022/0064669d-3595-4a43-9571-ca899a0877d3)
Finish 클릭

</div>
</details>

<br/>

# 2. VirtualBox 설정 및 머신 추가

>시작하기에 앞서 나는 **핸즈 온 해킹** 책을 기준으로 설정하였고 책을 따라하다 보니 이미 설정이 된 것들, 혹은 다른 점들이 있기에 기록한다.

내가 사용한 버전은 7.0 버전이다.
![스크린샷 2023-11-28 151018](https://github.com/oil-lamp-cat/oil-lamp-cat.github.io/assets/103806022/333a7a4b-780d-45ff-80a6-c3342c7d389c)

가장 처음 virtualbox를 키게 되면 아래와 같은 화면을 보게 될 것이다.
![스크린샷](https://github.com/oil-lamp-cat/oil-lamp-cat.github.io/assets/103806022/01981b20-d14e-4a26-812e-80b730a1b7a3)

여기서 우리는 파일 - 도구 - 네트워크 관리자 를 들어가 네트워크 설정을 보자.
![스크린샷](https://github.com/oil-lamp-cat/oil-lamp-cat.github.io/assets/103806022/590a6685-ba3c-4ac3-9268-1a063700e225)

책에서는 이것을 직접 설정해 주어야 했지만 다행히! 우리는 이미 설정이 끝나있다. 그래도 한번 살펴보자.
![스크린샷](https://github.com/oil-lamp-cat/oil-lamp-cat.github.io/assets/103806022/47b4da68-11ad-41f6-8235-278c4ef46ee2) 
호스트 전용 네트워크란 내가 만든 가상환경들 끼리만 통신이 가능하게 될 인터넷을 의미한다. 처음에는 왜 사용하는지 몰랐으나, 예를들어 우리가 취약한 서버를 설치해서 해킹 실습을 하게 된다면(실제로 하게 될 것이다.) 그 서버가 연결된 외부인터넷에서도 접속 할 수 있으면 내 컴퓨터가 위험하기에 가상환경으로 접속할 인터넷을 한정시켜 주는 것이다. 그리고 ipv4가 192.168.56.1/24이다.

속성을 누르면 다음과 같은 화면을 볼 수 있다. 이것 또한 우리는 자동으로 맞춰저 있기에 건드릴 것이 없다.
![스크린샷](https://github.com/oil-lamp-cat/oil-lamp-cat.github.io/assets/103806022/4ef1428f-5dcd-466f-87c9-c49557954c35)

하지만 **DHCP 서버**를 눌러보면 아래 화면과 다르게 서버 활성화가 꺼져있을 것이다. 우리는 이것을 활성화 해주면 된다.
![스크린샷](https://github.com/oil-lamp-cat/oil-lamp-cat.github.io/assets/103806022/aa930ec6-9ff7-41ff-ac45-d38fd5698daf)

# kali linux ios 다운로드

[kalilinux]()다운로드 사이트에 들어가서 live 버전을 다운로드 해주기로 하자

![스크린샷](https://github.com/oil-lamp-cat/oil-lamp-cat.github.io/assets/103806022/f899a108-6e57-44fa-aae1-154f05135169)
