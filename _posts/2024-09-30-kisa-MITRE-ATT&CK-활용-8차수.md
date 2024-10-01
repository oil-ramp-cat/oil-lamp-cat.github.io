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

### IOC (Indicator Of Compromise, 침해지표)

- 시스템이 악의적인 활동에 의해 침해되었을 가능성이 있음을 보여주는 운영체제 또는 네트워크 아티팩트
- IOC로 사용되는 정보 : 해시값, 파일 이름 및 경로, C2 도메인, IP Address, URL, 레지스트리 키, 서비스 이름/정보, 스케쥴된 태스크 정보 등
- IOC도 궁극적으로는 시그니처와 비슷하다고 본다.
- 최근에는 잠재적인 위협을 탐지하기 위한 보편적 이상 행위를 모두 포함하는 개념으로 확장되었다.
- 징후를 탐지하는 느낌으로 개념 확장되었다.

### 아티팩트 (Artifact)

> 부산물

하지만 쓸모없는 것이 아닌 소프트웨어 전체 구조와 기능을 설명하는데 도움이 되는 데이터베이스, 데이터 모델, 소프트웨어 매뉴얼 등 다양한 형태의 요소들이다.

### IOC 기반 탐지의 한계점

- reactive (리액티브) 하다. 존재하지 않던 악성코드를 탐지할 수는 없다. 이미 한번 겪어야 탐지 가능한 문제.
- 아직은 시그니처가 같고 있던 단점을 가지고 있다. [패턴과 시그니처 기반 보안 솔루션의 한계](https://jaysecurity.tistory.com/66)
- 공격에 사용되었던 특징을 뽑아낸 것이기에 유효 기간(수명)이 짧다.
- 시간이 지날 수록 IOC가 계속 쌓이게 되어 관리 및 탐지에 많은 시간이 소요된다.
- IOC 기반 탐지는 TTP에 비해 상대적으로 우회하기가 용이하다. 툴 수정등을 통해 우회할 수도 있다.
- false sense of security : 우리 IOC에서는 발견되지 않았기에 안전하다, 라고 하는 착각을 일으킬 수 있다. 안전하다고 100% 확신할 수 없음에도 안전하다고 믿을 수도 있다는 것. 블랙리스트 또한 같은 한계점을 가지고 있다.

### IOC 기반 탐지 VS 행위 기반 탐지

예시 1 )

|공격행위|IOC 기반 탐지|TTP 기반 탐지|
|:---:|:---:|:---:|
|LSASS(Local Security Authority Subsystem Service) 프로세스의 메모리 영역에 접근하여 사용자들의 크레덴셜 정보를 획득|사용한 도구(ex : Mimikatz)의 해시값을 이용하여 탐지| - LSASS의 메모리 영역에 접근하는 프로세스 모니터링 <br/> - LSASS를 대상으로 하는 프로세스 인젝션 모니터링 <br/> - LSASS에 로드된 DLL 중 서명되지 않은 것이 있는지 확인|

#### Mimikatz(미미캐츠)?

[야옹! 미미캐츠(Mimikatz)란 무엇일까요? 어떻게 피할 수 있을까요?](https://nordvpn.com/ko/blog/mimikatz/)

만약 위 상황에서 IOC 기반 탐지를 우회하기 위해 해시값이 다른 Mimikatz를 사용하거나 Mimikatz에 의해 만들어진 파일들을 삭제하거나 확장명을 다른 것으로 바꾸어 실행할 수도 있다.

예시 2 )

|공격행위|IOC 기반 탐지|TTP 기반 탐지|
|:---:|:---:|:---:|
|내부 시스템에 감염된 악성코드와 C&C 간 커뮤니케이션 (HTTP 활용)|C&C의 IP 주소, URL, User-Agent 문자열을 이용하여 탐지|- 내부 시스템에 있는 악성코드가 C2서버에 접속하여 명령을 하달받는다. 그렇기에 주기적으로 `아웃바운드 커넥션`(내부에서 외부 서버로 데이터를 전송하기 위해 연결되는 것)이 있는지 탐지한다. <br/> - 최근에 등록한 도메인(베이비 도메인) 혹은 랜덤하게 생성된 도메인에 속한 시스템과의 통신이 있는지 확인한다.|

## 3. MITRE ATT&CK 프래임워크의 이해(3)

[TTP(Tactic, Technique, Procedures)란 무엇인가? 사이버 보안에서의 중요성](https://dev-with-wish.tistory.com/entry/TTPTactic-Technique-Procedures%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80-%EC%82%AC%EC%9D%B4%EB%B2%84-%EB%B3%B4%EC%95%88%EC%97%90%EC%84%9C%EC%9D%98-%EC%A4%91%EC%9A%94%EC%84%B1)

다시한번 보고 옵시다!

### TTPs (Tactics, Techniques, Procedures)

- TTP는 위협 행위를 체계적으로 설명하기 위한 일종의 모델이다.
- Tactics (전술)은 `위협 행위의 목적`을 나타낸다.
- Techniques (테크닉)은 위협 행위의 목적을 달성하기 위해 `사용하는 테크닉`을 의미한다.
- Procedures (프로시저)는 테크닉을 `구현하기 위한 구체적인 절차와 방법`을 의미한다.

### TTPs 예시

|전술(Tactics)|테크닉/서브테크닉|프로시저 (Procedures)|
|:---:|:---:|:---:|
|최초 접근 (Initial Access) : 교두보 확보와 관련 있는 기법| 스피어 피싱/첨부파일, 링크 등을 이용한 스피어 피싱|- 악성 고스트스크립트가 포함된 HWP 파일을 이메일에 첨부 <br/> - 사용자가 첨부된 HWP 파일을 오픈하면 mshta.exe가 실행되어 외부에서 세컨 스테이지 악성 스크립트를 다운로드 받아 실행 <br/> - 악성 스크립트가 실행되면 HTTP를 이용하여 비콘(beacon) 악성코드를 실행|

```
스피어피싱의 서브 테크닉

- 링크
- 서비스 : 카카오톡 등
- 첨부파일
- etc
```

#### 비콘 (beacon)?

감염된 장비가 명령이나 추가 페이로드를 전달받기 위해 특정 시간에 공격자의 C2 서버에 연결을 시도하는 클라이언트 악성코드

[C2 서버를 이용하는 악성행위 분석](https://www.igloo.co.kr/security-information/command-control-%EC%84%9C%EB%B2%84%EB%A5%BC-%EC%9D%B4%EC%9A%A9%ED%95%98%EB%8A%94-%EC%95%85%EC%84%B1%ED%96%89%EC%9C%84-%EB%B6%84%EC%84%9D/)

#### Ghost 스크립트?

[한글 파일에 숨어든 ‘고스트’](https://image.ahnlab.com/atip/content/atcp/tistory/1239_AhnLab_ASEC_%ED%95%9C%EA%B8%80%ED%8C%8C%EC%9D%BC%EC%97%90%EC%88%A8%EC%96%B4%EB%93%A0%EA%B3%A0%EC%8A%A4%ED%8A%B8.pdf)

#### 사이버 킬 체인?

[사이버전 - 사이버 킬 체인(Cyber Kill Chain)](https://charstring.tistory.com/473)

#### 탐지 기술의 발전

`시그니처 기반` -> `IOC 기반` -> `행위기반의 탐지(ex : TTPs 중 대표적으로 EDR)`

[엔드포인트 탐지 및 대응(EDR)이란 무엇인가요?](https://www.ibm.com/kr-ko/topics/edr)

`MITRE-ATT&CK(TTPs)`은 그 전에 비해 공격자의 관점에서 보는 보안 전략

## 4.[이론] MITRE-ATT&CK-프래임워크의 이해 (4)

### MITRE?

- 1958년 설립된 미국의 비영리 민간 연구 기관
- 미국의 정부 기관과 협력하여 국방, 보안, 헬스케어 등의 분야에서 전문적인 연구를 수행

### CVE?

> Common Vulnerabilities and Exposures

`취약점` 리스트

과거에는 취약점을 부르는 용어가 다 달랐었지만 CVE 프로젝트를 통해서 취약점의 공식적인 이름을 붙이게 되었다.

[CVE란?](https://www.redhat.com/ko/topics/security/what-is-cve)

### CWE?

> Common Weakness Enumeration

`보안의 약점` 리스트

[CVE v.s. CWE 차이점](https://blog.naver.com/bycho211/221508854566)

### CAPEC?

> Common Attack Pattern Enumerations and Classifications

> 흔한 공격 패턴 목록화 및 항목화 

[[EQST insight] 사이버 공격 패러다임 변화에 따른 시나리오 기반 모의해킹 트렌드](https://blog.naver.com/adtkorea77/222501529256)

### STIX, TAXII?

> Structured Threat Information eXpression

인텔리전스에 관련된 정보를 저장하는 방법을 통일했다.

위협 정보를 표현하기 위한 틀

> Trusted Automated eXchange of Intelligence Information

STIX로 표현되는 위협 정보를 교환하는 프로토콜, 프레임워크이다.

[STIX/TAXII란?](https://www.cloudflare.com/ko-kr/learning/security/what-is-stix-and-taxii/)

### Cybox, MAEC?

[Using MAEC Report within Cuckoo Sandbox](https://www.hakawati.co.kr/entry/Using-MAEC-Report-within-Cuckoo-Sandbox)

요 부분은 추가 필요, 아직 이해 못함.

### MITRE ATT&CK 프레임워크

> Adversarial Tactics Techniques & Common Knowledge

> 공격자의 전술 기술 & 일반적인 정보

공격자들이 사용하는 TTP들을 집대성하고 체계적으로 정리해 놓은 공개된 데이터 베이스이다.

- 공개된 침해사고분석 보고서, 악성코드 분석 보고서, 위협 그룹에 대한 정보를 분석하여 공격자들의 TTPs를 집대성하고 체계적으로 정리한 공개된 지식 베이스
- 특히 post-exploitation(침투 후) 과정에서 사용되는 위협 행위들을 분석 및 분류 해 놓았다.
- OSINT를 통해 확인된 모든 TTPs들을 포함하고 있으나 따로 중요도/관찰빈도에 따른 정보는 반영되지 않았다.
- 하지만 여전히 블라인드 스팟(blind spot)은 존재하기에 지속적으로 업데이트 되고 있다.

## 5. [이론] MITRE ATT&CK 프래임워크의 이해(5)

## 6. [이론] MITRE ATT&CK 활용(1)

## 7. [이론] MITRE ATT&CK 활용(2)

## 8. [이론] MITRE ATT&CK 활용(3)

## 시험