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

# 구성 - 2차

```
.
├── control.py - flask
├── graph.py - 그래프 생성
├── insert_data.php - 삭제
├── insert_data.py - DB 저장
├── mktable.py - 테이블 백업용
└── templates
    ├── graph1.html - Temp, Hue
    ├── graph2.html - Cool, Hot
    └── iot.html - 메인 html
```

외부에서 접속은 불가하기에 이번에는 굳이 ip를 가리지는 않겠다

## control.py

그래프 생성, 그래프 띄우기 등 여러가지가 추가되었다.

<details><summary>control.py</summary>
<div markdown = "1">

```python
import RPi.GPIO as GPIO
import Adafruit_DHT
from insert_data import insert_data
from graph import create_graph_temp_hue, create_graph_cool_hot
import os
from flask import Flask, render_template, jsonify, send_from_directory

GPIO.setwarnings(False)
app = Flask(__name__)

hum_temp_pin = 4
relay_Cool = 5  # 임시
relay_Hot = 14
sensor = Adafruit_DHT.DHT11
GPIO.setmode(GPIO.BCM)
GPIO.setup(relay_Cool, GPIO.OUT)
GPIO.setup(relay_Hot, GPIO.OUT)

G1path = "templates/graph1.html"
G2path = "templates/graph2.html"

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

@app.route('/graph1')
def graph1():
    if not os.path.exists(G1path):
        create_graph_temp_hue()  # 그래프가 없으면 생성
    return send_from_directory('templates', 'graph1.html')

@app.route('/graph2')
def graph2():
    if not os.path.exists(G2path):
        create_graph_cool_hot()  # 그래프가 없으면 생성
    return send_from_directory('templates', 'graph2.html')

@app.route('/generate_graph1')
def generate_graph1():
    create_graph_temp_hue()
    return send_from_directory('templates', 'graph1.html')

@app.route('/generate_graph2')
def generate_graph2():
    create_graph_cool_hot()
    return send_from_directory('templates', 'graph2.html')

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

## graph.py

Temperature과 Humidity, Cool, Hot 그래프를 생성한다

데이터는 라즈베리파이2에 있는 MariaDB에 있다

문제점 : 네트워크 통신이다보니 새로 만드는데 속도가 꽤나 느리다

해결 : 이전에 만들어 놓은 그래프를 띄워놓고 후에 새로운 그래프가 만들어지면 그것을 띄운다

<details><summary>graph.py</summary>
<div markdown = "1">

```python
import pymysql
import plotly.graph_objs as go
import plotly
import json

def create_graph_temp_hue():
    # MariaDB 연결
    connection = pymysql.connect(
        host='192.168.137.243',
        user='dev',
        password='pwd',
        db='TestDB',
        charset='utf8',
        cursorclass=pymysql.cursors.DictCursor
    )
    
    try:
        with connection.cursor() as cursor:
            # 쿼리 업데이트: Temperature, Humidity 데이터를 가져오기
            sql = """
                SELECT Daytime, 
                       SUM(Humidity) AS Humidity, 
                       SUM(Temperature) AS Temperature
                FROM TempHue
                GROUP BY Daytime
                ORDER BY Daytime
            """
            cursor.execute(sql)
            data = cursor.fetchall()
            
            # 데이터 가공
            daytimes = [row['Daytime'] for row in data]
            humidities = [row['Humidity'] for row in data]
            temperatures = [row['Temperature'] for row in data]

            # 그래프 생성
            fig = go.Figure()
            
            # Humidity 데이터 - 곡선 그래프
            fig.add_trace(go.Scatter(x=daytimes, y=humidities, mode='lines', name='Humidity'))
            
            # Temperature 데이터 - 곡선 그래프
            fig.add_trace(go.Scatter(x=daytimes, y=temperatures, mode='lines', name='Temperature'))
            
            # 레이아웃 업데이트
            fig.update_layout(
                title='Temperature and Humidity Over Time',
                xaxis_title='Daytime',
                yaxis_title='Value',
                legend_title='Legend'
            )

            # 그래프를 JSON 형식으로 변환
            graph_json = json.dumps(fig, cls=plotly.utils.PlotlyJSONEncoder)
            
            # 생성한 그래프를 HTML로 저장
            with open("templates/graph1.html", "w") as f:
                f.write(f'<div id="graph_div1"></div><script src="https://cdn.plot.ly/plotly-latest.min.js"></script><script>var graph_json = {graph_json}; Plotly.newPlot("graph_div1", graph_json.data, graph_json.layout);</script>')

    finally:
        connection.close()

def create_graph_cool_hot():
    # MariaDB 연결
    connection = pymysql.connect(
        host='192.168.137.243',
        user='dev',
        password='pwd',
        db='TestDB',
        charset='utf8',
        cursorclass=pymysql.cursors.DictCursor
    )
    
    try:
        with connection.cursor() as cursor:
            # 쿼리 업데이트: Cool과 Hot 데이터를 가져오기
            sql = """
                SELECT Daytime, 
                       SUM(Cool) AS Cool, 
                       SUM(Hot) AS Hot
                FROM TempHue
                GROUP BY Daytime
                ORDER BY Daytime
            """
            cursor.execute(sql)
            data = cursor.fetchall()
            
            # 데이터 가공
            daytimes = [row['Daytime'] for row in data]
            cools = [row['Cool'] for row in data]
            hots = [row['Hot'] for row in data]

            # 그래프 생성
            fig = go.Figure()
            
            # Cool 데이터 - 막대 그래프
            fig.add_trace(go.Bar(x=daytimes, y=cools, name='Cool'))
            
            # Hot 데이터 - 막대 그래프
            fig.add_trace(go.Bar(x=daytimes, y=hots, name='Hot'))
            
            # 레이아웃 업데이트
            fig.update_layout(
                title='Cool and Hot Over Time',
                xaxis_title='Daytime',
                yaxis_title='Count',
                legend_title='Legend',
                barmode='group'  # 막대 그래프를 나란히 배치
            )

            # 그래프를 JSON 형식으로 변환
            graph_json = json.dumps(fig, cls=plotly.utils.PlotlyJSONEncoder)
            
            # 생성한 그래프를 HTML로 저장
            with open("templates/graph2.html", "w") as f:
                f.write(f'<div id="graph_div2"></div><script src="https://cdn.plot.ly/plotly-latest.min.js"></script><script>var graph_json = {graph_json}; Plotly.newPlot("graph_div2", graph_json.data, graph_json.layout);</script>')

    finally:
        connection.close()
```

</div>
</details>

## insert_data.py

데이터를 라즈베리파이2의 db로 보낸다

문제 : 느리다
해결법 : 라즈베리파이1에 db를 만들고 가져다 쓰는 것이 훨씬 빠를것

<details><summary>insert_data.py</summary>
<div markdown = "1">

```python
import pymysql
from datetime import datetime

def insert_data(data):
    try:
        # 데이터베이스 연결
        connection = pymysql.connect(
            host='192.168.137.243',
            user='dev',
            password='pwd',
            database='TestDB'
        )

        with connection.cursor() as cursor:
            # 현재 날짜와 시간 가져오기
            daytime = datetime.now().strftime('%Y-%m-%d %H:%M:%S')

            # SQL 쿼리 작성
            sql = "INSERT INTO TempHue (Daytime, Temperature, Humidity, Cool, Hot) VALUES (%s, %s, %s, %s, %s)"
            values = (daytime, data['temperature'], data['humidity'], data['cool'], data['hot'])

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

## mktable.py

실제 작동시에는 없어도 된다

테이블을 삭제하였을 때 백업 용으로 만들어 놓았다

<details><summary>mktable.py</summary>
<div markdown = "1">

```python
#테이블 삭제되면 백업용

import pymysql

conn = pymysql.connect(host='192.168.137.243', user='dev', password='pwd', db='TestDB', charset='utf8')

cur = conn.cursor()

sql = '''CREATE TABLE TempHue(Daytime varchar(50), Temperature varchar(20), Humidity varchar(20), Cool varchar(20), Hot varchar(20))'''

cur.execute(sql)

conn.commit()

conn.close()

print("done")
```
</div>
</details>

## graph1.html

`Temperature`과 `Humidity` 데이터를 담고 있는 html 그래프이다

`graph.py`를 통해 만들어진다

선 그래프이다

## graph2.html

`Cool`과 `Hot` 릴레이 데이터를 담고 있는 html 그래프이다

`graph.py`를 통해 만들어진다

막대 그래프이다

## iot.html

실제 가장 메인이 되는 html 코드이다

<details><summary>iot.html</summary>
<div markdown = "1">

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>IoT Dashboard</title>
    <script src="https://code.jquery.com/jquery-3.6.0.min.js"></script>
    <script src="https://cdn.plot.ly/plotly-latest.min.js"></script>
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

        function loadTempHueGraph() {
            if ($('#graph1').is(':visible')) {
                $('#graph1').slideUp(); // 그래프 숨기기
            } else {
                $.ajax({
                    url: '/graph1',
                    method: 'GET',
                    success: function(data) {
                        $('#graph1').html(data).slideDown();
                        //실행 후 새로운 그래프 가져오기
                        $.ajax({
                            url: '/generate_graph1',
                            method: 'GET',
                            success: function(newGraphData1) {
                                $('#graph1').html(newGraphData1);
                            },
                            error: function() {
                                $('#graph1').html('<p>Error generating new Temperature and Humidity graph.</p>');
                            }
                        });
                    },
                    error: function() {
                        $('#graph1').html('<p>Error loading Temperature and Humidity graph.</p>');
                    }
                });
            }
        }

        function loadCoolHotGraph() {
            if ($('#graph2').is(':visible')) {
                $('#graph2').slideUp(); // 그래프 숨기기
            } else {
                $.ajax({
                    url: '/graph2',
                    method: 'GET',
                    success: function(data) {
                        $('#graph2').html(data).slideDown();
                        //실행 후 새로운 그래프 가져오기
                        $.ajax({
                            url: '/generate_graph2',
                            method: 'GET',
                            success: function(newGraphData2) {
                                $('#graph2').html(newGraphData2);
                            },
                            error: function(){
                                $('#graph2').html('<p>Error loading Cool and Hot graph.</p>')
                            }
                        })
                    },
                    error: function() {
                        $('#graph2').html('<p>Error loading Cool and Hot graph.</p>');
                    }
                });
            }
        }

        $(document).ready(function() {
            $('#cool_button').click(toggleCool);
            $('#hot_button').click(toggleHot);
            $('#temphue_graph_button').click(loadTempHueGraph);
            $('#cool_hot_graph_button').click(loadCoolHotGraph);
            setInterval(updateData, 5000); // 5 seconds interval for data refresh
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

    <h2>Graphs</h2>
    <button id="temphue_graph_button">Temperature and Humidity Graph</button>
    <button id="cool_hot_graph_button">Cool and Hot Graph</button>
    <div id="graph1" style="display:none;"></div> <!-- Temperature and Humidity graph -->
    <div id="graph2" style="display:none;"></div> <!-- Cool and Hot graph -->
</body>
</html>
```

</div>
</details>

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

## 2024-08-08 3차

![image](https://github.com/user-attachments/assets/39fd918a-3385-498c-b824-491b86e16397)

일단 그래프를 넣는것 까지는 해냈다

지금 계속 시도해 보고 있는 것이 버튼을 누르면 그래프가 사라졌으면 좋겠는데... 정말 질기다

그래프를 사라지게 하면 그냥 시작부터 그래프가 없고

그래프를 냅두면 사라지지 않고... 끄응... 내가 디자인은 진짜 별로라 기능부터 만들고 있는데 쉽지 않다

다른 기능들 Hot, Cool 토글, 실시간 온도 저장 및 디스플레이는 아주 잘된다

사실 그래프가 보이는 것만으로 따지면 아주 잘된다만 쉽지 않다

리액트 쓰는게 좋으려나...

## 2024-08-09 

보아하니 데이터의 시간이 자꾸 이상하게 하루가 지났음에도 8월 8일 이라기에 확인을 해보니 라즈베리파이 시간이 유럽으로 잡혀있었다

어쩐지...

아니 그런데 다른 문제가 생겼다 

[![video](https://res.cloudinary.com/marcomontalbano/image/upload/v1723174577/video_to_markdown/images/youtube--E8QD6WVKMUY-c05b58ac6eb4c4700831b2b3070cd403.jpg)](https://youtu.be/E8QD6WVKMUY "video")

어째서 그래프를 띄우고 생성하는 부분이 데이터 통신을 하지도 않고 실행이 될 수 있단 말인가?

이거 해결해야하는데...