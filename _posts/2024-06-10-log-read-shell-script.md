---
title: "로그 생성 및 로그에서 에러 잡기 Shell Sciprt"
date: 2024-06-10 14:43:15 +09:00
categories: [Linux, shell script]
tags: [Bash]
pin: true
---

# 시작

예를 들어 `mlat.log`라는 파일이 있다고 했을 때 이 파일에서 오류(Error, critical, Down)을 찾아내기 위해 만들기 시작

하지만 이제 문제가 있다면 내가 직접 그 기계들이 작동하고 있는 동안(로그가 생성되는 동안)을 가정하고 스크립트를 짜야한다는 점..

내가 가지고 있는 파일은 이미 수많은 로그가 찍힌 `mlat.log`파일 뿐이다

그렇다면?

직접 한 줄씩 새로 로그 파일을 만드는 스크립트를 짜면 되겠네?

# log 생성 스크립트

``` bash
#!/bin/bash

LOG_FILE="./mlat.log"

NEW_LOG_FILE="./test.log"

if [ -f "$NEW_LOG_FILE" ]; then
	rm "$NEW_LOG_FILE"
fi

while read -r line
do
	echo "$line" >> "$NEW_LOG_FILE"
	echo "$line"
	sleep 1

done < "$LOG_FILE"
```

`mlat.log`파일을 불러오고 우리는 새로운 파일인 `test.log`파일을 만들어 낼 것이다

이 때 만약 파일이 이미 있다면? 삭제하고 없다면 그대로 진행한다

그 후 한줄씩 읽어가며 파일에 추가해주면 끝!

# 로그 읽고 에러 띄우기

그럼 이제 정말로 실시간 log가 생성될 때 에러를 잡는 코드를 짜보자

아 참고로 이름은 그냥 간단하게 RD LOD (로그 읽기)로 지었다

Ascii Art를 이용해서 이름을 꾸민 것이기에 필요 없다면 지우고 사용해도 된다

```bash
#!/bin/bash
echo "


██████╗ ██████╗     ██╗      ██████╗  ██████╗ 
██╔══██╗██╔══██╗    ██║     ██╔═══██╗██╔════╝ 
██████╔╝██║  ██║    ██║     ██║   ██║██║  ███╗
██╔══██╗██║  ██║    ██║     ██║   ██║██║   ██║
██║  ██║██████╔╝    ███████╗╚██████╔╝╚██████╔╝
╚═╝  ╚═╝╚═════╝     ╚══════╝ ╚═════╝  ╚═════╝ 

"

echo "프로그램을 실행합니다"
sleep 2
clear

LOG_FILE="./test.log"

PREVIOUS_CONTENT=""

#파일 변경 확인 함수
check_file_change() {
	if [ -f "$LOG_FILE" ]; then
		CURRENT_CONTENT=$(< "$LOG_FILE")
		if [ "$CURRENT_CONTENT" != "$PREVIOUS_CONTENT" ]; then
			echo "변경이 감지"
			PREVIOUS_CONTENT="$CURRENT_CONTENT"
		fi
	else
		echo "로그 파일이 없습니다"
		echo "1초 뒤 다시 실행합니다"
		sleep 1
		clear
	fi
}

while true
do
	check_file_change
	sleep 1 #요건 나중에 없앨 거에요~
done
```

일단은 파일의 변경을 감지하는 것 까지 만들었다

이제 log 변경이 감지된 후 파일을 읽어 오류 수와 오류 종류 등을 띄워줄 차례이다

## 1차

```bash
#!/bin/bash
echo "프로그램을 실행합니다"
sleep 1
clear

LOG_FILE="./test.log"

PREVIOUS_CONTENT=""

#파일 변경 확인 함수
check_file_change() {
	if [ -f "$LOG_FILE" ]; then
		CURRENT_CONTENT=$(< "$LOG_FILE")
		if [ "$CURRENT_CONTENT" != "$PREVIOUS_CONTENT" ]; then
			#echo "변경이 감지"
			echo "UP 수 : `cat $LOG_FILE | grep -c "UP"`"
			echo "DOWN 수 : `cat $LOG_FILE | grep -c "DOWN"`"
			if grep -q "CRITICAL" "$LOG_FILE"; then
				#CRITICAL 이 존재할 때
				echo "CRITICAL 수 : `cat $LOG_FILE | grep -c "CRITICAL"`"
			fi

			#이렇게 되면 안되는데...
			if grep -q "DOWN" "$LOG_FILE"; then
				echo -e '\a'
			fi
			PREVIOUS_CONTENT="$CURRENT_CONTENT"
		fi
	else
		echo "로그 파일이 없습니다"
		echo "1초 뒤 다시 실행합니다"
		sleep 1
		clear
		
	fi
}

while true
do
	check_file_change

	sleep 0.2
	clear
done
```

다 좋은데 지금 문제가 DOWN이 감지되면 소리가 난다는 것은 좋다 그런데 한번 문제를 감지하면 계속 소리를 낸 다는 것이다... 아이고

그렇다면 따로 변수를 지정해주어 DOWN의 현재 수를 기록해 두었다가 그것이 변경 될 때에 실행되게 만들면 될 것이다

일단 출력되는 것은 나중으로 생각하고 차근차근 해보자

## 2차

```bash
#!/bin/bash
echo "프로그램을 실행합니다"
sleep 1
clear

LOG_FILE="./test.log"

PREVIOUS_CONTENT=""

PREVIOUS_DOWN=0

CURRENT_DOWN=0

#파일 변경 확인 함수
check_file_change() {
	if [ -f "$LOG_FILE" ]; then

		#LOG_FILE 변수에 저장
		CURRENT_CONTENT=$(< "$LOG_FILE")

		#변경 감지
		if [ "$CURRENT_CONTENT" != "$PREVIOUS_CONTENT" ]; then

			#UP 수 확인
			echo "UP 수 : `cat $LOG_FILE | grep -c "UP"`"

			#DOWN 수 확인
			CURRENT_DOWN=$(grep -c "DOWN" "$LOG_FILE")
			echo "DOWN 수 : $CURRENT_DOWN"

			#CRITICAL 이 존재할 때
			if grep -q "CRITICAL" "$LOG_FILE"; then
				echo "CRITICAL 수 : `cat $LOG_FILE | grep -c "CRITICAL"`"
			fi

			#DOWN수가 변경되었을 때 소리내기
			if [ $CURRENT_DOWN -gt $PREVIOUS_DOWN ]; then
				echo -e '\a'
			fi

			PREVIOUS_DOWN=$CURRENT_DOWN
			PREVIOUS_CONTENT=$CURRENT_CONTENT
		fi
	else
		echo "로그 파일이 없습니다"
		echo "1초 뒤 다시 실행합니다"
		sleep 1
		clear
		
	fi
}



while true
do
	check_file_change
	sleep 0.2
	clear
done
```

이제 소리가 나오는 것 까지 완료 하였다

나중에 필요하면 `삡`소리 말고 다른 소리로 변경이 가능하기도 하다

이제 문제가 되는 DOWN이나 CRITICAL오류들을 따로 뽑아서 볼 수 있게 만들고자 한다

지금 생각한 바로는 따로 스크립트를 짜서 그 스크립트 실행 시 볼 수 있게 하거나

이미 있는 `RDlog.sh`에서 띄우거나 인데

로그를 좀 더 분석한 뒤에 결정해봐야 겠다