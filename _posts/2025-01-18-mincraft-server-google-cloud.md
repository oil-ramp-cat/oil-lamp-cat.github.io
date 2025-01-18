---
title: "[Google Cloud]마인크래프트 서버 만들기"
date: 2025-1-18 18:50:15 +09:00
categories: [마인크래프트, 서버]
tags: [google cloud]
pin: true
---

## 시작

2일 전 1월 16일 드디어 훈련소를 마치고 사회에 나왔다

물론 a형 독감과 함께라는 문제가 있었지만 일단 나오고 나니 너무 기분이 좋다

훈련소에서 만난 사람들과 마크 24시간 마크 서버를 간단하게 열어서 놀아볼까 하는 생각에 시작한다

## 선택

마인크래프트 서버를 여는 방법에는 여러가지가 있다

- 개인 컴퓨터
- 미니 컴퓨터
- 외부 서비스

일단 내가 고려해볼 것은 24시간 돌릴 생각이기에 개인 컴퓨터를 켜놓고 있기가 좀 그러니 외부 서비스와 미니 컴퓨터 중 하나일 것이다

외부 서비스에는 `Microsoft Azure` 과 `Google Cloud`가 있다

미니 컴퓨터로는 따로 구매하거나 내가 전에 작업했던 `Rpi 4B`가 있다

처음에는 `라즈베리파이`를 이용하려고 했었다. 그런데 생각해보니 

![Image](https://github.com/user-attachments/assets/453ee0c3-2609-40eb-abab-fe7e3344adbd)

우리 집이 좀 많이 오래된 곳인지라 랜선을 끌어오려면 라우터에서 직접 가져와야하는데 하필이면 출입문이 막고 있어 살짝 성가신 작업이 될 것이라고 생각한다

그렇다면 이제 `외부 서비스`를 이용하는 것인데 난 이미 `Microsoft Azure`을 재미사마 안드로이드 구동을 위한 VM 서비스를 한달 동안 썼기에 이제 무료 토큰은 남아있지 않았다

그럼 `Google Cloud`를 써야겠지?

## Google Cloud 무료 서비스 등록하기

![Image](https://github.com/user-attachments/assets/e0c6ce19-ce5c-4538-bf7f-4dce01f8e76e)

만약 처음 구글 계정으로 서비스를 이용하고자 한다면 `무료로 시작하기`가 있을 것이다. 그럼 구글 계정만 계속 만들면 무한으로 쓸 수 있나? 싶은 생각이 들기도 하지만 아니겠지

![Image](https://github.com/user-attachments/assets/635be07a-af8a-4f34-9011-7cec72d50c72)

300달러의 크레딧을 무료로 90일 동안 쓸 수 있다고 한다

일단 써보고 90일 안에 따로 서버를 구성한다면 옮기면 되고 아니면 다른 방법을 찾아보던가 하자, 일단은 간단히 가지고 놀 생각이니까

![Image](https://github.com/user-attachments/assets/d5d038a2-64fd-410d-9a07-87c221412dfd)

간단한 본인 정보를 넣고 나면 바로 사용할 수 있다

![Image](https://github.com/user-attachments/assets/aaa9f903-fc74-4db1-b85a-c0c7f8f2d58d)

당연지사 결제는 자동 이체로 전환되지 않는다고 한다

![Image](https://github.com/user-attachments/assets/7b6302f7-8efa-4079-9d26-65e3efbfcce5)

자 모든 정보를 입력하면 이제 곧 바로 서비스를 이용할 수 있게 된다

## 서버 생성하기

![Image](https://github.com/user-attachments/assets/34d54222-5f6a-4d58-89d2-a942174b3567)

꽤 많은 크레딧이 들어왔다

![Image](https://github.com/user-attachments/assets/e2eda44e-c052-4aea-9a63-8fa192cb0b54)

시작하면 바로 `My First Project`에 들어와있을 것이다

![Image](https://github.com/user-attachments/assets/4630a765-563b-4435-b542-4c00eb6bd2be)

데시보드에서 가상 `VM 인스턴스`에 들어가 서버를 생성하자

![Image](https://github.com/user-attachments/assets/20f6d014-553d-4d3f-a942-02b90d1537b2)

처음 들어가면 위와 같은 사진이 뜰 텐데 사용을 누르면 2분 정도의 시간이 지난 후 

![Image](https://github.com/user-attachments/assets/2040582c-7ecd-48d0-bce9-0ae98a8b08d5)

무한 로딩에 빠졌다.. 뭐지? 컴퓨터에 랜선이 아닌 무선랜을 쓰고 있어서 그런가? 아닐텐데 뭐가 문제인지 모르겠다

![Image](https://github.com/user-attachments/assets/4b06394b-9aad-4b1c-a1f5-222f274b944a)

페이지를 다시 로딩하니 제대로 페이지에 들어가졌다. 버그였던걸로

이제는 나 처럼 이미 한번 `Azure` 서비스나 다른 서비스를 이용해 본 사람이라면 쉽게 할 수 있을 것이다

![Image](https://github.com/user-attachments/assets/e20f40d7-9dd6-419f-a06c-42e2e39b5364)

![Image](https://github.com/user-attachments/assets/63cb6209-65b9-4b63-bd79-0d374315ced1)

머신 구성

- 리전 : 서울 
- 영역 : 자유
- N2 CPU (마크 서버는 싱글 코어를 사용한다고 한다)
- 코어 : 2
- 메모리 : 16GB

만약에 이 세팅에서도 잘 돌아가고 메모리를 8GB로 맞춰도 잘 돌아간다면 라즈베리파이로 도전해 봐도 좋을 듯 하다

![Image](https://github.com/user-attachments/assets/2cd45a4b-90e1-42d4-b171-e83e204b2861)

만들기 전 OS 및 스토레지에서 기본 설정인 `Devian`에서 내게 더 익숙한 `Ubuntu`로 변경 후 SSD스토레지로 설정하여 25GB의 크기를 할당해 주었다

![Image](https://github.com/user-attachments/assets/aaabc66c-7435-4476-a932-1a713167d994)

아니 그냥 50GB로 설정해 주었다. 이건 내맘

![Image](https://github.com/user-attachments/assets/5b714463-8aea-411f-9ab9-1e80a0f50ebc)

90일이면 3달 월 114.22를 쓰게 되고 내가 가진 크레딧은 442.18 꽤나 잘 책정된 것 같다

![Image](https://github.com/user-attachments/assets/9a44084b-d5b4-4797-bd88-66a5ab9bea05)

이 때 네트워크 탭에서 위 두 트래픽을 허용해 주라는데 나는 왜인지 이해하지 못했다

마인크래프트 방화벽을 여는데 HTTP, HTTPS가 필요한가?

![Image](https://github.com/user-attachments/assets/6d19fdd1-8a2d-4ddd-9ec1-88034059414a)

예약 버튼을 눌러 서버 IP도 할당해보자. 결과는 이제 친구들과만 공유하길 바란다

**만들기** 버튼을 누르면 서버를 생성할 수 있다

![Image](https://github.com/user-attachments/assets/11582e8b-33c4-4570-9b7a-cd529efc3952)

머신 생성 성공

## 방화벽 설정

외부에서 이 서버에 접속하기 위해서는 당연히 방화벽을 설정해줘야 한다

하다못해 라즈베리파이도 FTP 쓰려고 방화벽을 설정하기도 한다

![Image](https://github.com/user-attachments/assets/6d8d8974-dd1d-498a-add8-4572b4f13ad3)

좌측 탭에서 방화벽 설정을 해보자

**방화벽 정책**에서 **방화벽 규칙 만들기**를 하자

![Image](https://github.com/user-attachments/assets/1e9d60b3-1833-4810-beea-1b47aab929f7)

이름은 본인이 기억하기 쉽게 설정

![Image](https://github.com/user-attachments/assets/3f065d96-4c14-460d-8c2d-60149e293443)

포트는 TCP 포트로 열고 마인크래프트 서버를 위한 포트인 `25565`을 넣어주자

![Image](https://github.com/user-attachments/assets/ff9f1329-d792-4fbb-b23b-2958fcb398af)

만들고 나면 이렇게 VPC 방화벽 규칙에서 볼 수 있다

## 마인크래프트 서버 구동 준비

다시 `Computer Engine` 탭에서 `VM 인스턴스`로 들어가 `SSH`에 접속하자

![Image](https://github.com/user-attachments/assets/60d28e41-6d5f-46b3-ba65-579c8e66a779)

새로운 창에서 SSH에 접속할 수 있다

![Image](https://github.com/user-attachments/assets/31b237ad-5079-400c-9c64-592c19dde37a)

`root`권한 설정을 위해 비밀번호를 결정했다

내 생일로 설정

![Image](https://github.com/user-attachments/assets/029da4eb-0242-4983-b321-6ff5a737c269)

매번 리눅스를 키면 하던 `apt update`와 `apt upgrade`를 하자

![Image](https://github.com/user-attachments/assets/7d1f76d3-715c-486d-8f0d-444b795b7604)

마인크래프트 서버용 포트인 `25565`와 `OpenSSH`를 쓰기 위한 포트를 열어주자

![Image](https://github.com/user-attachments/assets/22de2153-8fb4-4124-8f3c-d5a4d160f34d)

이제 방화벽을 활성화 시켜주면 끝!

![Image](https://github.com/user-attachments/assets/d71a05d8-9874-4663-bfad-9f4ca1c35ddd)

상태를 확인해 보아도 포트가 잘 열려있음을 알 수 있다

## 마인크래프트 서버 구동 설치 (papermc)

이제 정말로 마크 서버를 구동시키기 위한 마지막 단계를 시작하자

나는 서버 구동을 위해 `Paper MC`를 이용하고자 한다. 마크 버전은 가장 최신 버전을 쓸 것이다 

![Image](https://github.com/user-attachments/assets/5f3bd865-d132-4ce7-bbab-9cf845caf3f2)

일단 시작으로는 `OpenJDK 21`버전을 받아주자

![Image](https://github.com/user-attachments/assets/a6152bfc-e15e-454e-b5fe-f83318e53b1f)

그 후 마인크래프트 계정을 생성하여 Root 에서 일반 계정으로 전환한다

![Image](https://github.com/user-attachments/assets/5ea7a6e1-3501-4ec2-98d8-3e72310b1f01)

그 후 서버를 생성할 폴더를 만들자

![Image](https://github.com/user-attachments/assets/dbf90371-cbd2-4d1b-a782-f28e89fbdd60)

가장 최근 `Paper MC`를 설치할 것인데 [paper](https://papermc.io/downloads/paper)사이트에 들어가 Build를 우클릭하고 링크를 복사해서 다운받을 수 있다

![Image](https://github.com/user-attachments/assets/9609d3c2-d4e8-4c29-ae3a-394bfb31f291)

설치된 것을 확인했다면 서버를 열어보자

![Image](https://github.com/user-attachments/assets/1d22b0a4-7cca-4740-8ef5-27ed8ccce526)

라고 하기에는 문제가 발생하였다

아무래도 다운로드 상에 문제인지 76KB만 다운이 되어 파일이 손상되었기에 `wget https://api.papermc.io/v2/projects/paper/versions/1.21.4/builds/117/downloads/paper-1.21.4-117.jar` 명령어를 통해 다시 다운로드 하였다

![Image](https://github.com/user-attachments/assets/c6390373-98ee-41d2-bbcc-c1a8c7b7862a)

이제 서버 실행에 성공했고 `eula 동의`를 해야한다

`nano eula.txt` 명령어로 파일에서 `false`값을 `true`로 변경하고 다시 실행하자

![Image](https://github.com/user-attachments/assets/968a7756-dd2f-4f5b-86da-2d62db47d4ca)

서버 실행에 성공했다!!

![Image](https://github.com/user-attachments/assets/48cc05ea-47db-4167-9617-b2a34147e236)

그리고 매번 `java -Xms8G -Xmx16G -jar paper-1.21.4-117.jar nogui`을 치기 귀찮아서 그냥 bash 실행파일을 만들었다

![Image](https://github.com/user-attachments/assets/1fb33d3b-133a-4969-9a46-7978b37307ca)

이제 아주 쉽게 서버를 열 수 있게 되었다

만 문제가 생겼다. SSH를 닫으면 서버가 꺼진다

왜냐? SSH 터미널에서 실행시키고 있었는데 그 터미널이 꺼졌으니 당연히 서버도 꺼질 수 밖에

## 서버 24시간 유지하기

뭐 방법은 여러가지가 있다

- tmux
- screen
- 백그라운드 실행

이 중에 나는 screen을 써보기로 했다

chat gpt가 이걸 추천하더라고

참고로 서버 종료 명령어는 `stop`이다

### 서버 시작

```
screen ./start.sh
```

### 스크린 리스트 확인

```
screen -ls
```

### 스크린 접속

```
screen -r (이름)
```

### 스크린 삭제

```
screen -S (이름) -X quit
```

## 접속해보자 1차

![Image](https://github.com/user-attachments/assets/11a0ea02-6048-4aa8-8221-127c251ff0cf)

일단 성공이다.. 와!

![Image](https://github.com/user-attachments/assets/65f1667c-f466-4b67-934c-d28ed75e6a41)

다른 사람들도 들어와서 사용 가능 확인 완료!


## 마인크래프트 서버 구동 설치 (Fabric)

모드를 사용하기 위해서는 `Fabric`이나 `Forge`를 이용해야 한다

또한 `Forge`는 훨씬 많은 모드를 보유하고 있지만 조금 무겁고

`Fabric`은 모드의 수가 적지만 훨씬 가벼운 서버에 친화적이다 고로 `Fabric` 너로 정했다

![Image](https://github.com/user-attachments/assets/ce889742-d7dd-4571-aba5-9ee70e7689d5)

VM 설정은 위와 동일하다

![Image](https://github.com/user-attachments/assets/b20d6403-bae3-4c29-a3ed-f891fcb6660f)

또한 서버 구동 준비 까지의 과정은 완전 동일하다

이번에는 모드의 버전이 낮기에 OpenJDK의 버전은 17이 될 것이다

![Image](https://github.com/user-attachments/assets/815b7fa6-f4a5-446c-9c3d-9591bfa3ea81)

설치가 끝나면 minecraft 계정으로 돌아간다. 그리고 이제부터가 진짜 `Fabric`을 설치하는 과정이 되겠다

![Image](https://github.com/user-attachments/assets/8394ba5b-0517-4be4-b9c1-7265efbcd8e9)

공식 문서에 나온 대로 CLI 설치를 하고 실행을 해주면 서버는 실행하는데 성공한 것이다

```
java -Xmx12G -jar fabric-server-mc.1.20.1-loader.0.16.10-launcher.1.0.1.jar nogui
```

다시한번 실행용 `start.sh`를 만들고 `eula.txt`에서 동의로 바꿔주면

![Image](https://github.com/user-attachments/assets/622723bf-914b-4894-9c02-156e5c682cc8)

아 맞다 권한을 안줬네

![Image](https://github.com/user-attachments/assets/3f2f8ac7-c37c-4899-a4db-9c13ca6f3bb8)

권한을 주고 처음 실행시키게 되면

![Image](https://github.com/user-attachments/assets/aa076b2f-982a-41a3-bb75-01dadd5d0304)

이렇게 맵을 만들고 있는 것을 볼 수 있다

![Image](https://github.com/user-attachments/assets/535f1926-ef47-4ac1-b018-a1cf7b867272)

좋아 이제 서버를 만드는 작업은 끝났고 모드를 설치하고 맵을 다시 뒤엎어보자

내가 설치할 모드는 Prominence II RPG: Hasturian Era라는 모드팩이다

일단 설치해 보고 나중에 싹 다 갈아엎을 수도?

```
wget https://mediafilez.forgecdn.net/files/6088/184/Prominence%20II%20RPG%20Hasturian%20Era-v3.0.30hf.zip
```

모드 설치 경로는 위와 같다

다름 모드들도 그렇지만 파일 다운로드 경로를 못 찾겠다면 그냥 본인 컴퓨터에 한번 다운받아보고 그 다운로드 경로를 써넣으면 된다

월드를 새로 갈아 엎을 때에는 `world`파일을 그냥 통째로 날려버려라 그럼 다시 만들어질 것이다

![Image](https://github.com/user-attachments/assets/c2be2f1d-ec71-40d4-b1b5-ed59c7e96ecf)

그.. 그만.. 이번 오류는 바로 의존성 모드가 필요하다고 한다..

```
wget https://mediafilez.forgecdn.net/files/4731/991/Luna-FABRIC-MC1.19.X-1.0.1.jar

wget https://mediafilez.forgecdn.net/files/6036/402/fabric-api-0.92.3%2B1.20.1.jar

wget https://mediafilez.forgecdn.net/files/5772/682/Necronomicon-Fabric-1.6.0%2B1.20.1.jar
```

![Image](https://github.com/user-attachments/assets/b86110d2-1076-4ca7-b345-f541a8665dfa)

일...단은 다 삭제하고 내일 다시 해봐야겠다. 어쩌면 그냥 모드팩이 아니라 모드만 까는 것이 훨씬 쉬울지도

![Image](https://github.com/user-attachments/assets/28e6b3f6-6618-48a8-b7ad-a7a3a7158122)

충격적이게도 내 본 마인크래프트를 설치하였을 때에 curse forge 앱에 서버용 다운로드가 뜨는 것을 발견하였다

설마 이거를 따로 써야하나?