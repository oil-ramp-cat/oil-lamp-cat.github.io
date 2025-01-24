---
title: "[Python] TCP 서버 통신 & 로그 분석 프로젝트"
date: 2025-1-23 20:58:15 +09:00
categories: [python, 서버]
tags: [작업, GUI]
pin: true
---

## 파이썬으로 하는 TCP 통신
![Image](https://github.com/user-attachments/assets/f334310e-b5ec-4d1a-88c3-8f6d3278b968)

내가 이번에 하려는 작업은 다음과 같다

1. `장치(Device)`에서 정보를 받아서 `서버1(Server1)`에 데이터를 보낸다
2. `서버1(Server1)`에서 필요한 정보를 걸러낸 뒤에 걸러낸 정보는 또 따로 저장하고 나머지 필요한 정보들을 `서버2(Server2)`에 다시 보낸다
3. `서버2(Server2)`와 다시 `서버1(Server1)` 통신 하면서 빠진 파일이 있는지 확인
4. 만약 빠진 파일이 있거나 걸러낸 정보 중에 보내고 싶은 것이 있다면 `GUI`를 통해 확인 후 다시 전송

솔직히 뭔가 C언어로 할 때에는 꽤나 복잡한 과정이 있었던지라 (뭐 사실 단순 TCP 통신이 아니기는 했지만) [TCP 포트스캐너 with.C](https://oil-lamp-cat.github.io/posts/TCP-Port-scan-c/) 파이썬으로 이렇게 쉽게 뚝딱 만들 수 있을 것이라고는 생각을 못했다

## 간단하게 구현한 서버, 클라이언트 코드

- 서버측

<details><summary>Server_Receive.py</summary>
<div markdown = "1">

```python
import socket
import configparser

# ConfigParser 객체 생성
config = configparser.ConfigParser()

# 설정 파일 읽기
config.read("config.ini")

# 서버 설정 값 가져오기
host = config["server"]["host"] # 서버의 IP 주소 또는 도메인 이름
port = int(config["server"]["port"]) # 포트 번호

print(f"Server Host: {host}")
print(f"Server Port: {port}")   

# 서버 소켓 생성
server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
server_socket.bind((host, port))
server_socket.listen(5)

print(f"서버가 {host}:{port}에서 대기 중입니다...")

while True:
    # 클라이언트 연결 대기
    client_socket, client_address = server_socket.accept()
    print(f"클라이언트 {client_address}가 연결되었습니다.")
    
    try:
        # 클라이언트로부터 요청 받기
        data = client_socket.recv(1024).decode("utf-8")
        if not data:
            continue

        # 요청 파싱
        parts = data.split("&&")
        if len(parts) != 0:
            name = parts[0]
            message = parts[1]
            response = f"어서와! {name}"

            # 클라이언트 이름과 메시지 출력
            print(f"클라이언트 이름: {name}")
            print(f"클라이언트 메시지: {message}")
        else:
            response = "유효하지 않은 요청"

        # 응답 클라이언트에게 전송
        client_socket.send(response.encode("utf-8"))

    except Exception as e:
        print(f"오류 발생: {e}")

    finally:
        # 클라이언트 소켓 닫기
        print("연결종료")
        client_socket.close()
        user_input = input("다음 작업을 진행하려면 'y', 종료하려면 'n'을 입력하세요: ").strip().lower()
        
        if user_input == 'y':
            print("서버 연결을 다시 시도합니다 \n")
        elif user_input == 'n':
            print("프로그램을 종료합니다. \n")
            
            exit()
        else:
            print("유효하지 않은 입력입니다. 'y' 또는 'n'을 입력하세요. \n")
```

</div>
</details>

</br>

- 클라이언트 측

<details><summary>Client_Send.py</summary>
<div markdown = "1">

```python
import socket

# 서버 설정
server_address = "127.0.0.1"  # 서버의 실제 IP 주소 또는 도메인 이름
server_port = 12345         # 서버 포트 번호

# 서버에 연결
client_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
client_socket.connect((server_address, server_port))

# 데이터 전송
name = "tester"
message = "Hello, World!"

request = f"{name}&&{message}"
client_socket.send(request.encode("utf-8"))

# 서버로부터 응답 받기
response = client_socket.recv(1024).decode("utf-8")
print(f"{name} : {message}")
print(f"서버 : {response}\n")

# 클라이언트 소켓 닫기
client_socket.close()
```

</div>
</details>

일단 서버측과 클라이언트 코드는 끝났으나 사실상 이제 시작이다! GUI를 구현해야하고 GUI 뿐만 아니라 서버 통신 외의 추가적인 기능들을 더 만들어야 한다

하지만 이번 코드는 혹시 모를 보안 문제가 있을 수 있기에 작동 장면만 찍어서 영상으로 올리도록 하겠다

아 GUI는 괜찮을지도?

> 서버측 로그

![Image](https://github.com/user-attachments/assets/23f7f556-fa17-4aec-8519-f822bd2622b7)

> 클라이언트 측 로그

![Image](https://github.com/user-attachments/assets/130a4156-6db2-43a6-9a29-09b5c45947ad)

## 문제점(?) 발견 - Server_Receive.py 이미 사용 중인 주소 문제

내가 짠 코드를 실행시킨 후에는 내가 의도한 대로 실행되어 문제가 없었으나 처음 코드를 실행시키는 과정에서 왜인지 모르게 문제가 발생했다

일단 퇴근해서 윈도우에서도 한 번 실행시켜봐야겠다

> Ubuntu 20.04 LTS

![Image](https://github.com/user-attachments/assets/4cd13d43-fb60-4ff7-ac9e-b11de18c307f)

위 사진을 보면 알 수 있듯 처음 `Server_Receive.py`를 실행시켰을 때에는 분명히 코드에 소켓을 종료하는 `client_socket.close()`가 있음에도 불구하고 서버를 종료했다가 곧바로 실행시키면 위와 같이 운영체제가 소켓을 바로 해제하지 않고 `TIME_WAIT`상태로 유지하기 때문에 생기는 문제로 리눅스에서 발생하는 문제인가 싶어 윈도우에서 테스트를 해보았다

> Windows 10 Pro 22H2

![Image](https://github.com/user-attachments/assets/aa5dc012-70c4-4768-8e9b-782385c66c64)

윈도우 상황에서는 분명 우분투와 같이 바로 종료 후 실행해 보아도 문제 없이 실행 되는 것을 알 수 있다

음... 내가 생각한 가설은 몇가지 있다

1. 우분투에서 파이썬의 소켓 종료 속도가 느리다
2. 우분투에서 로컬로 서버를 실행시키고 있기에 같은 IP의 Client, Server가 존재하여 문제가 생긴다
3. IP의 문제가 아니라 포트의 문제일 것이다. 파이썬이 우분투의 포트를 열고 닫음에 있어 대기시간이 있다

실제로 태스트를 해보았을 때에 우분투에서는 제대로 다시 서버를 사용할 수 있을 때 까지 최소 13초의 시간이 필요했다. 그런데 다른 환경에서 테스트 하기에는 시간이 걸리니 다른 누군가가 위 코드를 복사해서 테스트 해볼 수 있기를