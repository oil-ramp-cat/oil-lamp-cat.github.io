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

> 서버측
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

.

> 클라이언트 측

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

## GUI 구현하기 (Customtkinter)

나는 GUI를 다뤄보기는 했지만 정말 가지고 놀 생각으로 `shimejiee`를 파이썬으로 구현하고 싶어서 건들여만 봤지 이번처럼 아예 프로그램에 쓸 GUI를 만드는 것은 처음이 되겠다

[tkinter를 공부해보자!](https://oil-lamp-cat.github.io/posts/tkinter-%EA%B3%B5%EB%B6%80%ED%95%98%EA%B8%B0/), [PySide6를 공부해보자!](https://oil-lamp-cat.github.io/posts/PySide6%EB%A5%BC-%EA%B3%B5%EB%B6%80%ED%95%B4%EB%B3%B4%EC%9E%90/), [Pyqt5를 공부해보자!](https://oil-lamp-cat.github.io/posts/Pyqt5-%EA%B3%B5%EB%B6%80%ED%95%98%EA%B8%B0/)와 같은 여러 시도는 있었으나 결국 그 이후 CLI만 다루다가 이번에 갑작스럽게 도전하게 되었다

파이썬으로 GUI를 구현하는 방법은 여러가지가 있다

- flask로 html 코드로 구현
- tkinter
- pyqt
- pyside

그리고 이번에 사용해볼 방법은 여기에는 포함되어있지 않은 [customtkinter](https://customtkinter.tomschimansky.com/documentation/)되시겠다

![Image](https://github.com/user-attachments/assets/44c22e4d-e29e-4cdc-9860-ad042ade4bcb)

일단 가장 간단한 기본 코드를 `ChatGPT`에게 받았다 이제 이걸 통해서 내가 원하는 GUI를 구현할 차례이다

만... GUI 너무 어렵다 나한테는... 도당체 뭐가 뭔지..

## 서버 클라이언트 코드 2차

<details><summary>Client_Send.py</summary>
<div markdown = "1">

```python
import socket
import time

# TCP 서버의 IP와 포트 설정
TCP_IP = '127.0.0.1'  # 서버 IP
TCP_PORT = 12345      # 서버 포트

# 파일 경로 설정
file_path = "합치는중/test.txt"

try:
    # 소켓 생성
    client_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    client_socket.connect((TCP_IP, TCP_PORT))
    print(f"서버 {TCP_IP}:{TCP_PORT}에 연결되었습니다.")

    # 파일 읽기
    with open(file_path, 'r', encoding='utf-8') as file:
        for line in file:
            line = line.strip()  # 줄 끝의 공백 제거
            if line:  # 빈 줄 무시
                client_socket.sendall(line.encode('utf-8'))  # 데이터 전송
                print(f"보냄: {line}")

                # 서버로부터 응답 받기
                response = client_socket.recv(1024).decode('utf-8')  # 최대 1024 바이트 수신
                print(f"서버 응답: {response}")

                time.sleep(1)  # 1초 대기

    print("파일의 모든 줄을 전송했습니다.")
except FileNotFoundError:
    print(f"파일을 찾을 수 없습니다: {file_path}")
except ConnectionError:
    print("TCP 서버와 연결할 수 없습니다.")
except Exception as e:
    print(f"오류 발생: {e}")
finally:
    client_socket.close()
    print("연결이 종료되었습니다.")
```

</div>
</details>

테스트 파일을 한 줄씩 읽어서 서버로 보내는 작업을 추가하였다

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
host = config["server"]["host"]  # 서버의 IP 주소 또는 도메인 이름
port = int(config["server"]["port"])  # 포트 번호

print(f"Server Host: {host}")
print(f"Server Port: {port}")

def run_server():
    # 서버 소켓 생성
    server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    server_socket.bind((host, port))
    server_socket.listen(5)

    print(f"서버가 {host}:{port}에서 대기 중입니다...")

    while True:
        client_socket, client_address = server_socket.accept()
        print(f"클라이언트 {client_address}가 연결되었습니다.")

        try:
            while True:
                # 클라이언트로부터 요청 받기
                data = client_socket.recv(1024).decode("utf-8")  # 최대 1024 바이트 수신
                if not data:  # 클라이언트가 연결을 종료한 경우
                    print(f"클라이언트 {client_address} 연결 종료.")
                    break

                # 요청 파싱 및 응답 생성
                response = f"수신된 데이터: {data}"  # 적절한 응답 설정
                print(f"받은 데이터: {data}")

                # 응답 클라이언트에게 전송
                client_socket.send(response.encode("utf-8"))
                print(f"응답 전송 완료: {response}")

        except Exception as e:
            print(f"오류 발생: {e}")
        finally:
            # 클라이언트 소켓 닫기
            client_socket.close()
            print(f"클라이언트 {client_address}와의 연결이 종료되었습니다.")

if __name__ == "__main__":
    run_server()
```

</div>
</details>

서버에서는 한 줄씩 받아서 확인 후에 다시 클라이언트에게 응답을 전송하는 식으로 작동중이다

## 앱 코드 1차 (2025-01-28)

음.. 일단 간단하게 GUI를 구현하고 그에 따른 통신 기능까지는 넣어봤다

![Image](https://github.com/user-attachments/assets/d7b71a7c-9aa6-455c-aec0-9661b3eba2a8)

오는 통신을 리스트에 넣어서 보여준다

<details><summary>App.py</summary>
<div markdown = "1">

```py
import customtkinter as ctk
import socket
import configparser
import threading

class LTCApp:
    def __init__(self, root):
        self.root = root
        self.root.title("LTC")
        self.root.geometry("800x600")  # 초기 크기 설정 (원하는 크기로 조정 가능)
        ctk.set_appearance_mode("dark")
        
        
        # 초기화: 화면 구성
        self.create_main_frame()

        # TCP 소켓 초기화
        self.load_config()
        self.server_socket = None
        self.client_socket = None
        self.server_thread = None
        self.stop_thread_flag = threading.Event()  # 스레드 종료 플래그 추가
        
        # TCP 통신 연결 상태
        self.device1_connected = False  # Device 1 연결 상태
        self.device2_connected = False  # Device 2 연결 상태

    def load_config(self):
        # ConfigParser 객체 생성
        config = configparser.ConfigParser()

        # 설정 파일 읽기
        config.read("config.ini")

        # 서버 설정 값 가져오기
        self.Receive_IP = config["TCP_Receive"]["host"]
        self.Receive_Port = int(config["TCP_Receive"]["port"])
        self.Send_IP = config["TCP_Send"]["host"]
        self.Send_Port = config["TCP_Send"]["port"]

        print(f"TCP Receive IP : {self.Receive_IP}")
        print(f"TCP Receive Port : {self.Receive_Port}")
        print(f"TCP Send IP : {self.Send_IP}")
        print(f"TCP Send Port : {self.Send_Port}")
        
    def create_main_frame(self):
        # 메인 프레임 (모니터링 영역)
        self.main_frame = ctk.CTkFrame(self.root)
        self.main_frame.pack(fill="both", expand=True)

        # Monitor 구역
        self.monitor_frame = ctk.CTkFrame(self.main_frame)
        self.monitor_frame.pack(padx=10, pady=10, fill="x")

        self.monitor_frame.grid_rowconfigure(0, weight=1)  # 한 줄로 크기 비례 조정
        self.monitor_frame.grid_columnconfigure(0, weight=1)  # 왼쪽 열 비율 50%
        self.monitor_frame.grid_columnconfigure(1, weight=1)  # 오른쪽 열 비율 50%

        # 왼쪽 부분 (Device1 to PC)
        left_frame = ctk.CTkFrame(self.monitor_frame)
        left_frame.grid(row=0, column=0, padx=10, pady=10, sticky="nsew")  # "nsew"로 네 방향으로 확장

        # Device1 to PC 레이블
        self.device1_label = ctk.CTkLabel(left_frame, text="Device1 to PC", font=("Helvetica", 20, "bold"))
        self.device1_label.pack(padx=10)

        # Device1 상태 레이블
        self.device1_status = ctk.CTkLabel(left_frame, text="Disconnected", text_color="red", font=("Helvetica", 20, "bold"))
        self.device1_status.pack(padx=10)

        # 오른쪽 부분 (Device2 to PC)
        right_frame = ctk.CTkFrame(self.monitor_frame)
        right_frame.grid(row=0, column=1, padx=10, pady=10, sticky="nsew")  # "nsew"로 네 방향으로 확장

        # Device2 to PC 레이블
        self.device2_label = ctk.CTkLabel(right_frame, text="PC to Device2", font=("Helvetica", 20, "bold"))
        self.device2_label.pack(padx=10)

        # Device2 상태 레이블
        self.device2_status = ctk.CTkLabel(right_frame, text="Disconnected", text_color="red", font=("Helvetica", 20, "bold"))
        self.device2_status.pack(padx=10)

        # 설정 버튼 (상단 우측)
        self.settings_button = ctk.CTkButton(self.root, text="Settings", command=self.setup_settings_tab, font=("Helvetica", 20, "bold"))
        self.settings_button.pack(padx=10, pady=10, side="right", anchor="ne")

        # 로그 출력 구역
        self.log_frame = ctk.CTkFrame(self.main_frame)
        self.log_frame.pack(padx=10, pady=10, fill="both", expand=True)

        # 왼쪽과 오른쪽 로그 영역 (동적으로 크기 조정)
        self.left_log = ctk.CTkTextbox(self.log_frame, font=("Helvetica", 20, "bold"))
        self.left_log.grid(row=0, column=0, sticky="nsew", padx=10, pady=10)

        self.right_log = ctk.CTkTextbox(self.log_frame, font=("Helvetica", 20, "bold"))
        self.right_log.grid(row=0, column=1, sticky="nsew", padx=10, pady=10)

        # 실패한 로그 모음 구역
        self.failed_frame = ctk.CTkFrame(self.main_frame)
        self.failed_frame.pack(padx=10, pady=10, fill="x")

        self.failed_left = ctk.CTkTextbox(self.failed_frame, font=("Helvetica", 20, "bold"))
        self.failed_left.grid(row=0, column=0, padx=10, pady=10, sticky="nsew")

        self.failed_right = ctk.CTkTextbox(self.failed_frame, font=("Helvetica", 20, "bold"))
        self.failed_right.grid(row=0, column=1, padx=10, pady=10, sticky="nsew")

        # 레이아웃 비율 설정 (창 크기 변경 시 위젯 크기 비례적으로 변경)
        self.main_frame.grid_rowconfigure(0, weight=1)
        self.main_frame.grid_columnconfigure(0, weight=1)
        self.main_frame.grid_columnconfigure(1, weight=1)

        self.log_frame.grid_rowconfigure(0, weight=1)
        self.log_frame.grid_columnconfigure(0, weight=1)
        self.log_frame.grid_columnconfigure(1, weight=1)

        self.failed_frame.grid_rowconfigure(0, weight=1)
        self.failed_frame.grid_columnconfigure(0, weight=1)
        self.failed_frame.grid_columnconfigure(1, weight=1)

    ###
    ### 설정창
    ###
    def setup_settings_tab(self):
        # Toplevel을 사용하여 새 창 생성
        settings_window = ctk.CTkToplevel(self.root)
        settings_window.title("Settings")
        settings_window.geometry("700x400")
        
        # 설정 창을 최상위 창으로 설정
        settings_window.attributes("-topmost", True)  # 설정 창을 항상 최상위로 설정
        
        # settings_frame을 창 가운데에 배치
        settings_frame = ctk.CTkFrame(settings_window, width=600, height=200)
        settings_frame.place(relx=0.5, rely=0.5, anchor="center")  # 창의 가운데로 배치
        
        # Device 1 레이블
        device1_label = ctk.CTkLabel(settings_frame, text="Device 1", font=("Helvetica", 20, "bold"))
        device1_label.grid(row=0, column=0, padx=10, pady=10, columnspan=2, sticky="w")  # Device 1 레이블
        
        # IP 입력 (Device 1)
        ip_label1 = ctk.CTkLabel(settings_frame, text="IP:", font=("Helvetica", 20, "bold"))
        ip_label1.grid(row=1, column=0, padx=10, pady=10, sticky="w")  # 왼쪽 정렬
        ip_entry1 = ctk.CTkEntry(settings_frame, font=("Helvetica", 20, "bold"))
        ip_entry1.grid(row=1, column=1, padx=10, pady=10, sticky="ew")  # 가로로 확장
        ip_entry1.insert(0, self.Receive_IP)
        
        # Port 입력 (Device 1)
        port_label1 = ctk.CTkLabel(settings_frame, text="Port:", font=("Helvetica", 20, "bold"))
        port_label1.grid(row=2, column=0, padx=10, pady=10, sticky="w")  # 왼쪽 정렬
        port_entry1 = ctk.CTkEntry(settings_frame, font=("Helvetica", 20, "bold"))
        port_entry1.grid(row=2, column=1, padx=10, pady=10, sticky="ew")  # 가로로 확장
        port_entry1.insert(0, self.Receive_Port)
        
        # Device 1 Connect/Disconnect 버튼
        self.connect_button1 = ctk.CTkButton(settings_frame, text="Connect", command=self.connect_device1, font=("Helvetica", 20, "bold"))
        self.connect_button1.grid(row=3, column=0, pady=10)  # Device 1의 버튼을 하단에 배치
        self.disconnect_button1 = ctk.CTkButton(settings_frame, text="Disconnect", command=self.disconnect_device1, state="disabled", font=("Helvetica", 20, "bold"))
        self.disconnect_button1.grid(row=3, column=1, pady=10)  # Device 1의 Disconnect 버튼

        # Device 2 레이블
        device2_label = ctk.CTkLabel(settings_frame, text="Device 2", font=("Helvetica", 20, "bold"))
        device2_label.grid(row=0, column=2, padx=10, pady=10, columnspan=2, sticky="w")  # Device 2 레이블
        
        # IP 입력 (Device 2)
        ip_label2 = ctk.CTkLabel(settings_frame, text="IP:", font=("Helvetica", 20, "bold"))
        ip_label2.grid(row=1, column=2, padx=10, pady=10, sticky="w")  # 왼쪽 정렬
        ip_entry2 = ctk.CTkEntry(settings_frame, font=("Helvetica", 20, "bold"))
        ip_entry2.grid(row=1, column=3, padx=10, pady=10, sticky="ew")  # 가로로 확장
        ip_entry2.insert(0, self.Send_IP)
        
        # Port 입력 (Device 2)
        port_label2 = ctk.CTkLabel(settings_frame, text="Port:", font=("Helvetica", 20, "bold"))
        port_label2.grid(row=2, column=2, padx=10, pady=10, sticky="w")  # 왼쪽 정렬
        port_entry2 = ctk.CTkEntry(settings_frame, font=("Helvetica", 20, "bold"))
        port_entry2.grid(row=2, column=3, padx=10, pady=10, sticky="ew")  # 가로로 확장
        port_entry2.insert(0, self.Send_Port)
                
        # Device 2 Connect/Disconnect 버튼
        self.connect_button2 = ctk.CTkButton(settings_frame, command=self.connect_device2, font=("Helvetica", 20, "bold"))
        self.connect_button2.grid(row=3, column=2, pady=10)  # Device 2의 버튼을 하단에 배치
        self.disconnect_button2 = ctk.CTkButton(settings_frame, command=self.disconnect_device2, state="disabled", font=("Helvetica", 20, "bold"))
        self.disconnect_button2.grid(row=3, column=3, pady=10)  # Device 2의 Disconnect 버튼
        
        # Device 1 버튼 상태 설정
        if self.device1_connected:
            self.connect_button1.configure(state="disabled")
            self.disconnect_button1.configure(state="normal")
        else:
            self.connect_button1.configure(state="normal")
            self.disconnect_button1.configure(state="disabled")
        
        # Device 2 버튼 상태 설정
        if self.device2_connected:
            self.connect_button2.configure(state="disabled")
            self.disconnect_button2.configure(state="normal")
        else:
            self.connect_button2.configure(state="normal")
            self.disconnect_button2.configure(state="disabled")
    
        # 값을 저장할 변수
        def save_settings():
            # IP 입력된 값 가져오기
            ip_value1 = str(ip_entry1.get())
            ip_value2 = str(ip_entry2.get())
            # Port 입력된 값 가져오기
            port_value1 = str(port_entry1.get())
            port_value2 = str(port_entry2.get())
            
            # config.ini 파일에 저장
            config = configparser.ConfigParser()
            config.read("config.ini")
            config['TCP_Receive']['host'] = ip_value1
            config['TCP_Receive']['port'] = port_value1
            config['TCP_Send']['host'] = ip_value2
            config['TCP_Send']['port'] = port_value2
            
            with open('config.ini', 'w') as file:
                config.write(file)  # config.ini 파일에 저장
            
            self.Receive_IP = ip_value1
            self.Receive_Port = port_value1
            self.Send_IP = ip_value2
            self.Send_Port = port_value2
            
            print(f"Device 1 : {ip_value1}, {port_value1}\nDevice 2 : {ip_value2}, {port_value2}")  # 콘솔에 출력
            
            # 콘솔에 출력, 실제로는 파일에 저장하는 로직 등을 추가할 수 있음
    
        # Save Settings 버튼
        save_button = ctk.CTkButton(settings_frame, text="Save Settings", font=("Helvetica", 20, "bold"), command=save_settings)
        save_button.grid(row=4, column=0, columnspan=5, padx=10, pady=20)
    
    ###
    ### 통신 구현부 (받기)
    ###
    def connect_device1(self):
        print("Device 1 연결 중")
        self.device1_status.configure(text="Trying...", text_color="orange")
        
        # 연결 시도 상태로 업데이트
        self.stop_thread_flag.clear()  # 스레드 종료 플래그 초기화
        
        # 서버 소켓을 별도의 스레드에서 실행
        self.server_thread = threading.Thread(target=self.start_server)
        self.server_thread.daemon = True
        self.server_thread.start()

    def start_server(self):
        try:
            self.server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
            self.server_socket.bind((self.Receive_IP, self.Receive_Port))
            self.server_socket.listen(5)
            print(f"서버가 {self.Receive_IP}:{self.Receive_Port}에서 대기 중입니다...")
            self.connect_button1.configure(state="disabled")
            self.disconnect_button1.configure(state="normal")
            self.device1_connected = True
            
            while not self.stop_thread_flag.is_set():
                try:
                    if self.server_socket is None:
                        break
                    client_socket, client_address = self.server_socket.accept()
                    print(f"클라이언트 {client_address}가 연결되었습니다.")
                    self.device1_status.configure(text="Connected", text_color="green")
                    self.device1_connected = True
                    while not self.stop_thread_flag.is_set():
                        data = client_socket.recv(1024).decode("utf-8")
                        if not data:
                            print(f"클라이언트 {client_address} 연결 종료.")
                            self.connect_button1.configure(state="normal")
                            self.disconnect_button1.configure(state="disabled")
                            self.device1_connected = False
                            break

                        print(f"받은 데이터: {data}")
                        self.left_log.insert(ctk.END, f"{data}\n")
                        self.left_log.yview_moveto(1)

                        client_socket.send(f"수신된 데이터: {data}".encode("utf-8"))

                except Exception as e:
                    print(f"클라이언트 처리 중 오류 발생: {e}")
                    self.device1_connected = False
                finally:
                    if 'client_socket' in locals() and client_socket:
                        client_socket.close()
                        print(f"클라이언트 소켓 종료.")
                        self.device1_connected = False

        except Exception as e:
            print(f"서버 설정 중 오류 발생: {e}")
        finally:
            if self.server_socket:
                self.device1_connected = False
                try:
                    self.server_socket.close()
                    print("서버 소켓이 성공적으로 종료되었습니다.")
                except Exception as e:
                    print(f"서버 소켓 종료 중 오류 발생: {e}")
            self.server_socket = None


    def disconnect_device1(self):
        print("Device 1 연결 해제")
        self.device1_status.configure(text="Disconnecting...", text_color="orange")

        # 먼저 소켓 통신을 종료하기 전에 스레드 종료 플래그를 설정
        self.stop_thread_flag.set()  # 스레드 종료 플래그 설정

        # 서버 종료를 별도의 스레드로 처리
        disconnect_thread = threading.Thread(target=self._disconnect_server)
        disconnect_thread.daemon = True
        disconnect_thread.start()

    def _disconnect_server(self):
        # 서버와 클라이언트 소켓이 아직 열려 있다면 종료
        if self.server_socket:
            try:
                self.server_socket.close()
                self.server_socket = None
                print("서버 소켓이 성공적으로 종료되었습니다.")
            except Exception as e:
                print(f"서버 소켓 종료 중 오류 발생: {e}")

        if self.client_socket:
            try:
                self.client_socket.close()
                self.client_socket = None
                print("클라이언트 소켓이 성공적으로 종료되었습니다.")
            except Exception as e:
                print(f"클라이언트 소켓 종료 중 오류 발생: {e}")

        # UI 상태 업데이트
        self.device1_status.configure(text="Disconnected", text_color="red")
        self.connect_button1.configure(state="normal")
        self.disconnect_button1.configure(state="disabled")
        
    ###
    ### 통신 구현부 (보내기) - 아직 작업중
    ###
    def connect_device2(self):
        print("Device 2 연결 중")
        # 연결 시도 상태로 업데이트
        self.device2_status.configure(text="Trying...", text_color="orange")
        
        # 연결 로직을 추가할 수 있습니다.
        # 예를 들어, 연결 성공 후
        self.root.after(2000, self.device2_connected)  # 2초 후 연결 성공 처리 (여기서는 예시)

    def device2_connected(self):
        # 연결 성공 시 상태를 초록색으로 업데이트
        self.device2_status.configure(text="Connected", text_color="green")

        # 연결 버튼 상태 업데이트
        self.connect_button2.configure(state="disabled")
        self.disconnect_button2.configure(state="normal")

    def disconnect_device2(self):
        print("Device 2 연결 해제")
        # 연결 해제 로직을 추가할 수 있습니다.
        self.device2_status.configure(text="Disconnected", text_color="red")

        self.connect_button2.configure(state="normal")
        self.disconnect_button2.configure(state="disabled")

# 메인 실행부
if __name__ == "__main__":
    root = ctk.CTk()
    app = LTCApp(root)
    root.mainloop()
```

</div>
</details>

일단 오늘은 여기까지만 하고 정리를 먼저 해보자

> 지금까지 만들어진 것들

1. TCP 통신 연결(내가 서버일 경우) 후에 로그를 받아서 추가하는 기능
2. 연결이 끊기거나 연결되었을 때 Disconnect, Connect가 뜨며 색 변경
3. 왐마 많이 남았네

> 추가하거나 처리해야하는 부분

1. 내용을 TCP로 보내는 부분을 완성해야한다
2. 로그를 선택할 수 있고 로그를 조절(다시 보내기, 선택)할 수 있어야 한다. 지금은 text로 추가되는 중이라...
3. 추가적으로 선택할 로그 파일을 선택할 수 있어야 한다
4. 로그를 1시간 단위로 저장할 수 있어야 한다
5. 들어오거나 나가는 로그에서 뺴내야할 부분을 빼야한다
6. GUI 개정.. 가능하려나?