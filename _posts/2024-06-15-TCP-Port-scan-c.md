---
title: "[C] 포트 스캐닝"
date: 2024-06-15 23:56:15 +09:00
categories: [Linux, C, Port scanning]
tags: [C]
pin: true
---

# 지난번에 이어서

지난번에는 `Bash shell`을 이용한 포트 스캐너를 만들었다

사실 가장 최근에 다른 작업을 잠시 하게 되며 흥미를 가지게 된 언어여서 가능하면 `Bash`를 이용해 `nmap`에 있는 기능들을 구현해 보고자 하였다

허나 `shell`만의 기능으로는 `3-way-handshake` 외의 기능은 구현하기에 어려움이 있었고 다른 언어와 섞어 스크립트를 구현하고자 한다

사실 처음에는 LUA라는 언어를 사용해볼까 하는 생각을 한 적이 있다

LUA는 nmap의 근간이 되는 언어이기에 nmap과 비슷한 기능을 구현할 수 있다면 가장 최적의 언어가 아닐까 싶었다

그러다 [RustScan](https://github.com/RustScan/RustScan)이라는 nmap보다 더 빠른 속도를 가진다고 하는 다른 스캐너를 발견하게 되었다

그래서 또 다시 코드를 살펴보기 시작했지만...

아직 나의 실력으로는 코드를 이해하기에 어려움이 있었다

만약 코드를 뜯어본다 하여도 그 시간이 매우 오래 걸릴 것이라 생각해 이번에는 일단 내가 써본적이 있는 언어인 `C`언어를 선택하게 되었다

python이 탈락한 이유는... 느리다

사실 가장 쉽게 짤 수 있게 사람들이 많이 글을 올려놓기도 하였다만 나는 그 글에 나오는 스캔 방식이 결국 `bash`로 만들었던 기능과 유사했기에 이번에는 `C`를 이용할 것이다

## Tcp Half Open Scan - Tcp SYN Scan

![tcp_halfopenscan](https://github.com/oil-lamp-cat/oil-lamp-cat.github.io/assets/103806022/76f2a664-4fff-44d2-8170-08fe679dc673)

`Tcp SYN Scan`은 SYN을 보내고 SYN/ACK를 받으면 바로 RST패킷을 보내 연결되지 않도록하여 스텔스 스캔이라고 불리기도 한다

세션 연결에 대한 로그가 남지 않지만 SYN 전송에 대한 기록이 남기에 엄연히 말하자면 완전한 스텔스는 아닌 모양이다

# 코드 구현

자 이제 진짜 시작이다

1. 타겟에 SYN 신호를 보냄
2. SYN/ACK 신호가 돌아오면 열린 포트
3. RST 혹은 FIN 패킷을 보내 바로 연결 되기 전에 종료
4. RST/ACK 플래그가 돌아오면 닫힌 포트

일단 소켓 프로그래밍에 관해 아는 것이 없기에 [블로그를 참조](https://velog.io/@dltmdrl1244/%EC%86%8C%EC%BC%93%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D13-raw-%EC%86%8C%EC%BC%93)하여 공부를 좀 해보자
[요것도](https://velog.io/@ilov1112/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D-RAW-SOCKET)

[이것도](https://nitwit.tistory.com/18)

[IF_INET](https://coding-leaf.tistory.com/39)

[Raw Socket](https://velog.io/@euijoo3233/0223)

[TCP/IP 소켓](https://hannom.tistory.com/26)

[sys/socket](https://seongmok.com/43)

[C언어 인자 받기](https://bo5mi.tistory.com/165)

[구조체](https://www.tcpschool.com/c/c_struct_intro)

[소켓 프로그래밍](https://www.hackerschool.org/HS_Boards/zboard.php?id=Free_Lectures&no=32)

[sockaddr_in 구조체](https://blog.naver.com/jkssleeky/220603262892)

## 1차

일단 stealth scan을 구현하기 전 소켓 프로그래밍에 익숙해지기 위한 `TCP Full Open Scan`을 만들어보자

```c
#include <stdio.h>
#include <sys/socket.h>
#include <sys/types.h>
#include <string.h>
#include <arpa/inet.h>
#include <unistd.h> //close 쓰기 위해

int main(int argc, char* argv[]){ // 인자 받기
    int sockfd; //소켓 초기화
    struct sockaddr_in dest_addr; //소켓 구조체 선언
    int port = 0; // 포트 초기화
    int ret = 0; // 연결 결과 초기화
    for(port = 1; port <= 1024; port ++) // 스캔 포트 수 1 ~ 1024까지
    {
        sockfd = socket(PF_INET, SOCK_STREAM, 0); //소켓 생성
        memset((char*)&dest_addr,0,sizeof(dest_addr)); //메모리 초기화 함수
        dest_addr.sin_family = AF_INET; //프로토콜 주소체계 저장
        dest_addr.sin_port = htons(port); //사용할 포트 저장
        dest_addr.sin_addr.s_addr = inet_addr(argv[1]); //IP할당
        ret = connect(sockfd,(struct sockaddr*)&dest_addr,sizeof(dest_addr)); //연결
        if(ret != -1){ // 결과 -1 이면 포트 생존
            printf("%d Port Open\n", port);
        }
        close(sockfd); //소켓 닫기
    }
    printf("DONE \n");
    return 0;
}
```
![스크린샷 2024-06-16 10-51-03](https://github.com/oil-lamp-cat/oil-lamp-cat.github.io/assets/103806022/4a261884-cef7-480d-ada2-e66695690f75)

정확히 631번 포트에서 포트 열림을 확인할 수 있었다

다른 포트에서는 [RST, ACK]닫혔다는 패킷이 보인다

## 2차

이제 `Raw Socket`을 만들 차례이다

[Raw IP, Raw Socket](https://dev-jaeho.tistory.com/4)

[AF_INET vs PF_INET](https://www.bangseongbeom.com/af-inet-vs-pf-inet.html)

[PF_INET과 AF_INET의 차이점](https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=ifkiller&logNo=70081338273)

[[TCP/IP 소켓]소켓의 타입과 프로토콜의 설정](https://hannom.tistory.com/27)

[types.h 파일](https://www.ibm.com/docs/ko/aix/7.3?topic=files-typesh-file)

[소켓프로그래밍#1 : 소켓의 이해와 기본 뼈대](https://velog.io/@dltmdrl1244/%EC%86%8C%EC%BC%93%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D1-%EC%86%8C%EC%BC%93%EC%9D%98-%EC%9D%B4%ED%95%B4%EC%99%80-%EA%B8%B0%EB%B3%B8-%EB%BC%88%EB%8C%80)

[[C] 소켓 옵션 제어 ( getsockopt, setsockopt )](https://letitkang.tistory.com/243)

[setsockopt(2) - socket의 속성을 설정하는 함수](https://www.it-note.kr/122)

[TCP Sequence Number와 ACK Number](https://eunhyee.tistory.com/246)

[checksum](https://hojak99.tistory.com/246)

```c
socket(PF_INET, SOCK_STREAM, 0)
```

저번에는 소켓을 만들 때 `PF`를 사용하여 프로토콜 체계를 이용하여 `SOCK_STREAM`으로 안정적인 연결을 지양했었다

하지만 이번에는 `AF`를 이용해 `SOCK_RAW` raw 소켓을 만들어 스텔스 스캔을 구현해보자

```c
raw_socket = socket(AF_INET,SOCK_RAW,IPPROTO_RAW);
```
날것 그대로의 소캣

... 잠시 머리가 아파 중지.. 로컬호스트로 스캔을 했을 때 wireshark에도 잡히지 않아서 머리가 지끈지끈..

## 3차

[c언어 명령행 인자 argc argv : 옵션을 어떻게 처리할까?](https://codingdog.tistory.com/entry/c%EC%96%B8%EC%96%B4-%EB%AA%85%EB%A0%B9%ED%96%89-%EC%9D%B8%EC%9E%90-argc-argv-%EC%98%B5%EC%85%98%EC%9D%84-%EC%96%B4%EB%96%BB%EA%B2%8C-%EC%B2%98%EB%A6%AC%ED%95%A0%EA%B9%8C)

[[C언어와 친구들] int argc와 char *argv[]란?](https://bo5mi.tistory.com/165)

### C -Threading을 이용

처음 C로 3-way-handshaking을 구현했을 때에는 thread를 이용해 속도를 비약적으로 향상시켰었다

nmap으로 `127.0.0.1`을 통해 tryhackme 머신을 스캔해 보았을 때 2.3초가 걸리던 것을 C언어를 통해 0.5초까지 줄일 수 있었다

```c
//기본 함수
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#include <pthread.h> //스레드

// 추가적인 함수들
#include <sys/socket.h>
#include <arpa/inet.h>
#include <netinet/in.h>
#include <sys/time.h>


#define MAX_PORTS 100  // 최대 포트 번호 범위
#define NUM_THREADS 100  // 사용할 스레드 수 (임의로 설정)

struct ScanThreadArgs {
    const char *host;
    int start_port;
    int end_port;
};

// TCP 연결을 시도하여 포트가 열려 있는지 확인하는 함수 (스레드용)
void *scan_ports(void *args) {
    struct ScanThreadArgs *scan_args = (struct ScanThreadArgs *) args;

    for (int port = scan_args->start_port; port <= scan_args->end_port; port++) {
        struct sockaddr_in addr;
        int sockfd, result;

        // 소켓 생성
        if ((sockfd = socket(AF_INET, SOCK_STREAM, IPPROTO_TCP)) < 0) {
            perror("socket"); //오류잡이
            continue;
        }

        // 주소 구조체 초기화
        memset(&addr, 0, sizeof(addr));
        addr.sin_family = AF_INET;
        addr.sin_addr.s_addr = inet_addr(scan_args->host);
        addr.sin_port = htons(port);

        // 연결 시도
        result = connect(sockfd, (struct sockaddr *) &addr, sizeof(addr));

        // 연결 결과 확인
        if (result == 0) {
            printf("Port %d open\n", port);
        }

        // 소켓 닫기
        close(sockfd);
    }

    pthread_exit(NULL);
}

// 초 단위 시간을 시, 분, 초로 변환하여 출력하는 함수
void print_time(struct timeval *timeval) {
    struct tm *local_time;
    char time_str[30];

    // 초 단위 시간을 struct tm 구조체로 변환
    local_time = localtime(&timeval->tv_sec);

    // 시간을 시:분:초 형식의 문자열로 변환
    strftime(time_str, sizeof(time_str), "%T", local_time);

    // 변환된 문자열과 마이크로초 출력
    printf("%s.%06ld", time_str, timeval->tv_usec);
}

int main(int argc, char *argv[]) {
    if (argc < 2) {
        printf("Usage: %s <host>\n", argv[0]);
        return 1;
    }

    char *host = argv[1];
    struct timeval start_time, end_time, elapsed_time;

    // 프로그램 시작 시간 기록
    gettimeofday(&start_time, NULL);
    printf("Program start time: ");
    print_time(&start_time);
    printf("\n");

    // 스레드 배열 및 인자 초기화
    pthread_t threads[NUM_THREADS];
    struct ScanThreadArgs thread_args[NUM_THREADS];
    int ports_per_thread = MAX_PORTS / NUM_THREADS;

    // 스레드 생성 및 실행
    for (int i = 0; i < NUM_THREADS; i++) {
        thread_args[i].host = host;
        thread_args[i].start_port = i * ports_per_thread + 1;
        thread_args[i].end_port = (i + 1) * ports_per_thread;

        if (pthread_create(&threads[i], NULL, scan_ports, (void *) &thread_args[i]) != 0) {
            fprintf(stderr, "Error creating thread %d\n", i);
            return 1;
        }
    }

    // 모든 스레드 종료 대기
    for (int i = 0; i < NUM_THREADS; i++) {
        pthread_join(threads[i], NULL);
    }

    // 프로그램 종료 시간 기록
    gettimeofday(&end_time, NULL);
    printf("Program end time: ");
    print_time(&end_time);
    printf("\n");

    // 총 걸린 시간 계산
    timersub(&end_time, &start_time, &elapsed_time);
    printf("Total elapsed time: %ld seconds %ld microseconds\n", elapsed_time.tv_sec, elapsed_time.tv_usec);

    return 0;
}

```

### C++ - async await을 이용

하지만 가만히 생각해보니 쓰레드는 결국 하드웨어에 따라 사용할 수 있는 수가 달라지다보니 이번에는 다른 방법을 사용해 보기로 하였다

그렇게 생각해 낸 것이 async await을 이용한 비동기 처리다

하지만 지금까지 짰던 C언어에서는 불가능 하기에 이번에는 C++를 이용하기로 하였다

```cpp
#include <iostream>
#include <vector>
#include <future>
#include <cstring> //c언어 스트링 함수 쓰려고
#include <unistd.h>

// 추가적인 헤더파일
#include <sys/time.h> //시간
#include <sys/socket.h> //소켓
#include <arpa/inet.h>



#define MAX_PORTS 1000  // 최대 포트 번호 범위

// TCP 연결을 시도하여 포트가 열려 있는지 확인하는 함수 (비동기 방식)
int scan_port_async(const std::string &host, int port) {
    struct sockaddr_in addr;
    int sockfd, result;

    // 소켓 생성
    if ((sockfd = socket(AF_INET, SOCK_STREAM, IPPROTO_TCP)) < 0) {
        perror("socket");
        return -1;
    }

    // 주소 구조체 초기화
    memset(&addr, 0, sizeof(addr));
    addr.sin_family = AF_INET;
    addr.sin_addr.s_addr = inet_addr(host.c_str());
    addr.sin_port = htons(port);

    // 연결 시도
    result = connect(sockfd, (struct sockaddr *) &addr, sizeof(addr));

    // 소켓 닫기
    close(sockfd);

    // 연결 결과 반환
    if (result == 0) {
        return port;  // 포트 열림
    } else if (errno == ECONNREFUSED) {
        return 0;  // 포트 닫힘
    } else {
        perror("connect");
        return -1; // 에러 발생
    }
}

// 초 단위 시간을 시, 분, 초로 변환하여 출력하는 함수
void print_time(struct timeval *timeval) {
    struct tm *local_time;
    char time_str[30];

    // 초 단위 시간을 struct tm 구조체로 변환
    local_time = localtime(&timeval->tv_sec);

    // 시간을 시:분:초 형식의 문자열로 변환
    strftime(time_str, sizeof(time_str), "%T", local_time);

    // 변환된 문자열과 마이크로초 출력
    std::cout << time_str << "." << timeval->tv_usec;
}

int main(int argc, char *argv[]) {
    if (argc < 2) { //만약 옵션이 들어오지 않았다면
        std::cerr << "Usage: " << argv[0] << " <host>" << std::endl;
        return 1;
    }

    std::string host = argv[1]; //ip 할당
    struct timeval start_time, end_time, elapsed_time;

    // 프로그램 시작 시간 기록
    gettimeofday(&start_time, nullptr);
    std::cout << "Program start time: ";
    print_time(&start_time);
    std::cout << std::endl;

    std::vector<std::future<int>> futures;
    std::vector<int> open_ports;

    // 비동기적으로 포트 스캔 실행
    for (int port = 1; port <= MAX_PORTS; ++port) {
        futures.push_back(std::async(std::launch::async, scan_port_async, host, port));
    }

    // 모든 비동기 작업 완료 및 결과 처리
    int open_ports_count = 0, closed_ports = 0, error_ports = 0;
    for (auto &future : futures) {
        int result = future.get(); // 비동기 작업 결과 가져오기

        if (result > 0) {
            open_ports.push_back(result);
            open_ports_count;
        } else if (result == 0) {
            ++closed_ports;
        } else {
            ++error_ports;
        }
    }

    // 프로그램 종료 시간 기록
    gettimeofday(&end_time, nullptr);
    std::cout << "Program end time: ";
    print_time(&end_time);
    std::cout << std::endl;

    // 총 걸린 시간 계산
    timersub(&end_time, &start_time, &elapsed_time);
    std::cout << "Total elapsed time: " << elapsed_time.tv_sec << " seconds " << elapsed_time.tv_usec << " microseconds" << std::endl;

    // 결과 출력
    if (!open_ports.empty()) {
        std::cout << "Open ports:";
        for (auto port : open_ports) {
            std::cout << " " << port;
        }
        std::cout << std::endl;
    } else {
        std::cout << "No open ports found." << std::endl;
    }

    std::cout << "Open ports: " << open_ports_count << std::endl;
    std::cout << "Closed ports: " << closed_ports << std::endl;
    std::cout << "Errors: " << error_ports << std::endl;

    return 0;
}

```

매~우 흡족