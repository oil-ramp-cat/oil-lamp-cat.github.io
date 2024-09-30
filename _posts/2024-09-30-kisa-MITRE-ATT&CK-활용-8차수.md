---
title: "[KISA Academy] [초급] 침해사고 대응훈련 - MITRE ATT&CK 활용"
date: 2024-09-30 17:19:15 +09:00
categories: [KISA, 보안]
tags: [MITRE ATT&CK]
pin: true
---

# MITRE ATT&CK 활용 8차수

![image](https://github.com/user-attachments/assets/07c75c87-a813-4981-9da1-ffb89fb13140)

[KISA 아카데미](https://academy.kisa.or.kr/main.kisa)에서 처음으로 신청사게 되어 듣게 된 `MITRE ATT&CK`에 관한 강의이다.

[초급]이기에 이번 8회차에서는 개념에 관해 많이 나왔다.(내가 가장 필요한 것들!)

강의 내용을 요약해서 기록하고 아무리 기초라고 하여도 나에게 생소한 단어들이 많았기에 기록한다.

## 1. [이론] MITRE ATT&CK 프레임워크의 이해(1)

1. `MITRE ATT&CK`은 처음 2013년에 처음 등장했으나 2018년도즘에 들어서 관심이 급속도로 증가하기 시작했다.

그 이유는 다음과 같다.

- 사이버 보안 전략이 차단, 보호 중심에서 탐지 전략으로 바뀌기 시작했다.

### 차단 및 보호 중심의 방어 전략

- 사전 위협을 식별하고 이를 제거했다.
- 제거되지 못한 위협은 방화벽이나 인증 시스템 등을 활용하여 정보 자산에 위협 요소가 접근하는 것을 차단하였다.
- 현재까지도 차단 중심의 보안 기술은 중요하지만 그것만으로는 다양한 위협에 완벽히 대응하기 어려워졌다.

[공격 표면(Attack Surface)?](https://www.igloo.co.kr/service/klu-attack-surface-management/)

[제로 트러스트(zero trust)?](https://www.hpe.com/kr/ko/what-is/zero-trust.html)

### 탐지의 중요성(침투)

전형적인 APT 스타일의 공격자는 아래와 같은 규칙을 통해 침투할 수 있다고 한다.

1. 초기 침투 (initial compromise)
2. 교두보 확보 (establish foothold)
3. 권한 상승 (escalate privilege)
4. 내부정찰 (internal recon)
   - 레터럴 무브먼트(lateral movement)
5. 목적 행위 수행 (action on objectives)

이것도 하나하나 찾아볼까 했는데 매우 자세히 설명되어있는 것을 찾아 첨부한다.

[[KDT_AISEC] 4주차 - 지능형 지속 공격 APT](https://velog.io/@gloomy_passion/KDTAISEC-4%EC%A3%BC%EC%B0%A8-%EC%A7%80%EB%8A%A5%ED%98%95-%EC%A7%80%EC%86%8D-%EA%B3%B5%EA%B2%A9-APT)

[[Gloomy]KDT_AISEC](https://velog.io/@gloomy_passion/series/KDTAISEC) 공부할 블로그 추가!

#### APT?

> Advanced Persistent Threat

다른 글에도 한 번 썻던거 같은데 익숙하지 않으니 다시 찾게된다.

지능적이고(Advanced) 지속적인(Persistent) 공격(Threat)

[APT-1 (APT 공격이란 무엇인가?)](https://brunch.co.kr/@ka3211/23)에 정말로 자세히 설명되어있다. 개념 뿐 아니라 공격 세부 진행상황까지 설명되어있다.

### 공격자가 갖는 약점

- 내부망의 환경을 파악할 시간이 필요하다.
- 레터럴 무브먼트를 통해 최종 목표에 도달하기까지 시간이 소요된다.

고로 방어자는 내부망에 침투하는 것을 차단하는데 실패하더라도 실질적으로 피해가 발생하기 전까지 위협을 제거할 기회가 존재한다. 그렇기에 최근에는 탐지의 중요성도 높아지고 있다.

### DWELL TIME

> 잠복기

최초 침투 후에 발견되기 까지의 소요 시간

### 탐지 전략의 변화

> 고통의 피라미드

공격자의 활동을 탐지하는데 사용할 수 있는 공격 유형의 지표이며 그에 따라 소요되는 노력을 피라미드로 만든 것

![image](https://github.com/user-attachments/assets/b5043852-0f73-4bf0-9c70-ff2426b290f9)

왼쪽 탐지의 기준이 되는 값

오른쪽은 공격적자가 보았을 때 왼쪽 지표들을 우회하기 위해 소요되는 노력의 정도

- 해시값 (Trivial) : 가벼운 수준의 코드 수정만으로도 우회가 가능하다. 예를 들어 스크립트 변경으로 우회 가능.

- IP 어드레스(Easy) : 간단한 노력만으로 IP 주소 변경 가능하다. 예를들어 C2의 아이피 변경하거나 코드의 IP를 변경하는 것으로도 우회 하여 블랙리스트 등의 방법을 우회할 수 있다.
- 도메인 네임(Simple) : 도메인 변경 가능.
- 네트워크/호스트 아티팩트(Annoying) : 코드 수정, 아티팩트 제거 등의 노력이 필요. IOC(Indicator Of Compromise) == 침해지표 기반 탐지 우회 필요.
- 툴(Challenging) : 많은 노력 필요. 하지만 우회가 불가능 하지는 않다.
- TTP(Tough) : 새로운 방식의 공격 방법이 필요함.

과거에는 IOC 기반 탐지를 많이 사용했으나 최근에는 TTP 기반 탐지를 많이 사용한다.

### EDR?

> Endpoint Detection and Response

기본적으로는 TTP 기반 탐지 솔루션이다.

[[공지] 그래서 EDR이 뭐야? (EDR 기초지식 알아보기)](https://m.blog.naver.com/cybereason/222473264298)

### TTP?

> Tactic, Technique, Procedures

- Tactic : 공격 목표
- Technique : 공격 방법
- Procedure : 공격의 세부 실행 방법

[TTP(Tactic, Technique, Procedures)란 무엇인가? 사이버 보안에서의 중요성](https://dev-with-wish.tistory.com/entry/TTPTactic-Technique-Procedures%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80-%EC%82%AC%EC%9D%B4%EB%B2%84-%EB%B3%B4%EC%95%88%EC%97%90%EC%84%9C%EC%9D%98-%EC%A4%91%EC%9A%94%EC%84%B1)

### IOC?

> Indicator of Compromise

[[보안상식] IOC(Indicator Of Compromise)란?](https://m.blog.naver.com/kaoni_gw/221035228430)

[What are indicators of compromise (IOCs)?](https://www.microsoft.com/en-us/security/business/security-101/what-are-indicators-of-compromise-ioc)

## 2. [이론] MITRE ATT&CK 프레임워크의 이해(2)

## 3. MITRE ATT&CK 프래임워크의 이해(3)

## 4.[이론] MITRE-ATT&CK-프래임워크의 이해 (4)

## 5. [이론] MITRE ATT&CK 프래임워크의 이해(5)

## 6. [이론] MITRE ATT&CK 활용(1)

## 7. [이론] MITRE ATT&CK 활용(2)

## 8. [이론] MITRE ATT&CK 활용(3)

## 시험