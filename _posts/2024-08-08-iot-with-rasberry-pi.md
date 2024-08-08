---
title: "라즈베리파이를 가지고 iot를 만들어보자"
date: 2024-08-08 15:57:15 +09:00
categories: [raspberry pi, mariaDB, mysql, php]
tags: [command, iot]
pin: true
---

# 개요

## 준비물

|종류|수량|설명|
|------|---|---|
|라즈베리파이 ver.4|2|프론트엔드 및 백엔드 서버, DB 서버 총 2개|
|릴레이 모듈|SRD-05VDC-SL-C|에어컨, 히터 온오프 제어|
|습온 측정 모듈|DHT11|방 습도 및 온도 측정용 센서|

## 라즈베리파이 1

1. 사용자(나)가 접속하여 에어컨 제어 및 히터를 제어할 수 있게 한다
2. 방 습도 및 온도를 측정하여 사용자에게 띄워준다
3. 에어컨 및 히터의 사용량에 따른 그래프를 보여준다
4. 사용시간을 계산하여 전기사용량 및 비용을 계산한다

## 라즈베리파이 2

1. DB테이블을 세팅하여 일정 시간마다 온습정보와 에어컨 혹은 히터의 사용량을 저장한다
2. 끝이다

이렇게 보니까 되게 라즈베리파이 1에 db를 만들어서 하나로 구동시키는 것도 방법일 것 같다.. 만 이번에는 외부 db와 통신하는 연습을 할 것이기에 2대를 가지고 구현해본다

# 구성 - 1차

```cmd
.
├── control.py
├── insert_data.php - 삭제
├── insert_data.py
└── templates
    └── iot.html
```

## control.py

이름과 같이 실제 http 서비스를 띄워주고 백엔드 데이터들을 제어하는 역할을 한다

<details><summary>1차</summary>
<div markdown = "1">

```py
import RPi.GPIO as GPIO
import time
import Adafruit_DHT
import requests
from flask import Flask, render_template, jsonify

GPIO.setwarnings(False)
app = Flask(__name__)

hum_temp_pin = 4
relay_Cool = 5
relay_Hot = 6
sensor = Adafruit_DHT.DHT11
GPIO.setmode(GPIO.BCM)
GPIO.setup(relay_Cool, GPIO.OUT)
GPIO.setup(relay_Hot, GPIO.OUT)

relay_status = {'Cool': 0, 'Hot': 0}
hum_temp = {'Humidity': 0, 'Temperature': 0}

@app.route('/')
def home():
    return render_template('iot.html', relay_status=relay_status, hum_temp=hum_temp)

@app.route('/update')
def update():
    humidity, temperature = Adafruit_DHT.read_retry(sensor, hum_temp_pin)
    hum_temp['Humidity'] = humidity
    hum_temp['Temperature'] = temperature
    send_data_to_php(humidity, temperature)
    return jsonify(hum_temp=hum_temp)

def send_data_to_php(humidity, temperature):
    url = "http://라즈베리파이1_IP_ADDRESS/insert_data.php"
    data = {
        'humidity': humidity,
        'temperature': temperature
    }
    requests.post(url, data=data)

@app.route('/toggle_cool')
def toggle_cool():
    relay_status['Cool'] = 1 if relay_status['Cool'] == 0 else 0
    GPIO.output(relay_Cool, GPIO.HIGH if relay_status['Cool'] else GPIO.LOW)
    return jsonify(relay_status=relay_status)

@app.route('/toggle_hot')
def toggle_hot():
    relay_status['Hot'] = 1 if relay_status['Hot'] == 0 else 0
    GPIO.output(relay_Hot, GPIO.HIGH if relay_status['Hot'] else GPIO.LOW)
    return jsonify(relay_status=relay_status)

if __name__ == '__main__':
    app.run(debug=True, port=8080, host='0.0.0.0')

```

</div>
</details>

<details><summary>2차</summary>
<div markdown = "1">

```python
import RPi.GPIO as GPIO
import Adafruit_DHT
from insert_data import insert_data
from flask import Flask, render_template, jsonify

GPIO.setwarnings(False)
app = Flask(__name__)

hum_temp_pin = 4
relay_Cool = 5 #임시
relay_Hot = 14
sensor = Adafruit_DHT.DHT11
GPIO.setmode(GPIO.BCM)
GPIO.setup(relay_Cool, GPIO.OUT)
GPIO.setup(relay_Hot, GPIO.OUT)

relay_status = {'Cool': 0, 'Hot': 0}
hum_temp = {'Humidity': 0, 'Temperature': 0}

@app.route('/')
def home():
    return render_template('iot.html', relay_status=relay_status, hum_temp=hum_temp)

@app.route('/update')
def update():
    humidity, temperature = Adafruit_DHT.read_retry(sensor, hum_temp_pin)
    hum_temp['Humidity'] = humidity
    hum_temp['Temperature'] = temperature
    send_data(humidity, temperature)
    return jsonify(hum_temp=hum_temp)

def send_data(humidity, temperature):
    data = {
        'humidity': humidity,
        'temperature': temperature,
        'cool': relay_status['Cool'],
        'hot': relay_status['Hot']
    }
    insert_data(data)

@app.route('/toggle_cool')
def toggle_cool():
    relay_status['Cool'] = 1 if relay_status['Cool'] == 0 else 0
    GPIO.output(relay_Cool, GPIO.HIGH if relay_status['Cool'] else GPIO.LOW)
    return jsonify(relay_status=relay_status)

@app.route('/toggle_hot')
def toggle_hot():
    relay_status['Hot'] = 1 if relay_status['Hot'] == 0 else 0
    GPIO.output(relay_Hot, GPIO.HIGH if relay_status['Hot'] else GPIO.LOW)
    return jsonify(relay_status=relay_status)

if __name__ == '__main__':
    app.run(debug=True, port=8080, host='0.0.0.0')
```

GPIO핀 값 조정 및 `insert_data` 파이썬으로 교체

</div>
</details>

## insert_data.php - 삭제

라즈베리파이 1에서 얻은 temphue, on,off 데이터를 라즈베리파이 2의 mariaDB로 옮겨 저장

하곤 했었는데 내가 처음 접하는 언어라 그런건가? 아니면 데이터 흐름을 이해하지 못하여 그런것인지 자꾸만 

```html
<!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN">
<html><head>
<title>404 Not Found</title>
</head><body>
<h1>Not Found</h1>
<p>The requested URL was not found on this server.</p>
<hr>
<address>Apache/2.4.61 (Raspbian) Server at 192.xxx.xxx.xxx Port 80</address>
</body></html>
```

위와 같은 오류를 마주하게 되어서 차라리 파이썬을 이용하기로 하였다... 

파이썬이 더 편해용...


<details><summary>1차</summary>
<div markdown = "1">

```php
<?php
$servername = "192.xxx.xxx.xxx";
$username = "xxxx";
$password = "xxxx";
$dbname = "TestDB";

// Create connection
$conn = new mysqli($servername, $username, $password, $dbname);

// Check connection
if ($conn->connect_error) {
    die("Connection failed: " . $conn->connect_error);
}

$humidity = $_POST['humidity'];
$temperature = $_POST['temperature'];
$daytime = date('Y-m-d H:i:s');

$sql = "INSERT INTO TempHue (Daytime, Temperature, Humidity) VALUES ('$daytime', '$temperature', '$humidity')";

if ($conn->query($sql) === TRUE) {
    echo "New record created successfully";
} else {
    echo "Error: " . $sql . "<br>" . $conn->error;
}

$conn->close();
?>
```

</div>
</details>
</br>

일단은 내 노트북 하스팟을 이용하기에 ip를 가릴 필요는 없다만 혹시 모르기에 가렸다

## insert_data.py
`inset_data.php`사용하는 것에 실패하고 내가 자주 쓰던 언어인 python으로 넘어왔다

다시 구현을 해보자면 일단은 다음과 같다

<details><summary>1차</summary>
<div markdown = "1">

```python
import pymysql
from datetime import datetime

def insert_data(data):
    try:
        # 데이터베이스 연결
        connection = pymysql.connect(
            host='192.xxx.xxx.xxx',
            user='xxxx',
            password='xxxx',
            database='TestDB'
        )

        with connection.cursor() as cursor:
            # 현재 날짜와 시간 가져오기
            daytime = datetime.now().strftime('%Y-%m-%d %H:%M:%S')

            # SQL 쿼리 작성
            sql = "INSERT INTO TempHue (Daytime, Temperature, Humidity) VALUES (%s, %s, %s)"
            values = (daytime, data['temperature'], data['humidity'])

            # 쿼리 실행
            cursor.execute(sql, values)
            connection.commit()
            print("New record created successfully")

    except pymysql.MySQLError as e:
        print(f"Error: {e}")

    finally:
        connection.close()
```

</div>
</details>

## iot.html

사용자에게 보여줄 페이지

<details><summary>1차</summary>
<div markdown = "1">

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>IoT Dashboard</title>
    <script src="https://code.jquery.com/jquery-3.6.0.min.js"></script>
    <script>
        function updateData() {
            $.ajax({
                url: '/update',
                method: 'GET',
                success: function(data) {
                    $('#humidity').text(data.hum_temp.Humidity);
                    $('#temperature').text(data.hum_temp.Temperature);
                }
            });
        }

        function toggleCool() {
            $.ajax({
                url: '/toggle_cool',
                method: 'GET',
                success: function(data) {
                    $('#cool_status').text(data.relay_status.Cool ? '1' : '0');
                }
            });
        }

        function toggleHot() {
            $.ajax({
                url: '/toggle_hot',
                method: 'GET',
                success: function(data) {
                    $('#hot_status').text(data.relay_status.Hot ? '1' : '0');
                }
            });
        }

        $(document).ready(function() {
            $('#cool_button').click(toggleCool);
            $('#hot_button').click(toggleHot);
            setInterval(updateData, 5000); // 5초마다 데이터 갱신
        });
    </script>
</head>
<body>
    <h1>IoT Dashboard</h1>
    <h2>Relay Status</h2>
    <p>Cool: <span id="cool_status">{{ relay_status['Cool'] }}</span></p>
    <button id="cool_button">Toggle Cool</button>
    <p>Hot: <span id="hot_status">{{ relay_status['Hot'] }}</span></p>
    <button id="hot_button">Toggle Hot</button>

    <h2>Temperature and Humidity</h2>
    <p>Humidity: <span id="humidity">{{ hum_temp['Humidity'] }}</span>%</p>
    <p>Temperature: <span id="temperature">{{ hum_temp['Temperature'] }}</span>°C</p>
</body>
</html>
```

</div>
</details>
</br>

# 구성 - 2차

# 중간점검 모음집

## 2024-08-08

php에서 파이썬으로 넘어오고 테스트를 해보았다

```
192.xxx.xxx.xxx - - [08/Aug/2024 02:53:00] "GET / HTTP/1.1" 200 -
New record created successfully
192.xxx.xxx.xxx - - [08/Aug/2024 02:53:16] "GET /update HTTP/1.1" 200 -
```

설마?

```
MariaDB [TestDB]> SELECT * FROM TempHue;
+---------------------+-------------+----------+
| Daytime             | Temperature | Humidity |
+---------------------+-------------+----------+
| 2024-07-27 10:57:48 | 31          | 56       |
| 2024-07-27 10:57:50 | 31          | 56       |
| 2024-07-27 10:57:51 | 31          | 56       |
| 2024-07-27 10:57:53 | 31          | 55       |
+---------------------+-------------+----------+
4 rows in set (0.001 sec)
```

에서

```
MariaDB [TestDB]> SELECT * FROM TempHue;
+---------------------+-------------+----------+
| Daytime             | Temperature | Humidity |
+---------------------+-------------+----------+
| 2024-07-27 10:57:48 | 31          | 56       |
| 2024-07-27 10:57:50 | 31          | 56       |
| 2024-07-27 10:57:51 | 31          | 56       |
| 2024-07-27 10:57:53 | 31          | 55       |
| 2024-08-08 02:53:16 | 31          | 65       |
+---------------------+-------------+----------+
5 rows in set (0.001 sec)
```
이거지!!!!!!!!!!!!!!!!

일단 데이터가 저장되는 부분은 해결했으니 릴레이 저장할 수 있도록 테이블을 추가해야겠다

## 2024-08-08 2차

```
asde@raspberrypi:~/iot/webapp $ python control.py
Traceback (most recent call last):
  File "/home/asde/iot/webapp/control.py", line 4, in <module>
    from graph import mkgraph
  File "/home/asde/iot/webapp/graph.py", line 2, in <module>
    import pandas as pd
  File "/usr/local/lib/python3.9/dist-packages/pandas/__init__.py", line 19, in <module>
    raise ImportError(
ImportError: Unable to import required dependencies:
numpy: Error importing numpy: you should not try to import numpy from
        its source directory; please exit the numpy source tree, and relaunch
        your python interpreter from there.
```

라즈베리파이에서 판다스 문제인지 뭔 짓을 다 해봐도 문제가 생기기에 찾아보니 의존성 문제인 것 같다고는 하는데

[[라즈베리파이] numpy 관련 Pandas import error](https://amatoroi.tistory.com/54)

난 모르겠다