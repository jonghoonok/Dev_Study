# Internet

인터넷의 기본적인 원리





Index

* [How does the internet work?](#how-does-the-internet-work-)
- [What is HTTP?](#what-is-http-)
- [Browsers and how they work?](#browsers-and-how-they-work-)
- [DNS and how it works?](#dns-and-how-it-works-)
- [What is Domain Name?](#what-is-domain-name-)
- [What is hosting?](#what-is-hosting-)





## How does the internet work?





인터넷의 정의

- TCP/IP 프로토콜을 기반으로 전 세계의 **다양한 네트워크**(좁게는 AS)를 연결한 것

  - **Autonomous System** : 자율 시스템
    - 독립적인 네트워크로 한 조직에 의해 관리됨
    - AS 내의 라우터들은 단일한 **라우팅 프로토콜**을 사용함





ISP(Internet Service Provider)

- 집이나 사업장에 유료료 **인터넷을 제공하는 공급자**

- 각 ISP는 고유한 ASN(AS Number)를 갖고 있음: ASN은 **BGP라우팅**에서 사용됨

  - Border Gateway Protocol: AS간을 연결하는 Exterior Router Protocol의 한 예

- 인터넷 상의 모든 ISP는 **Tier1**를 경유하여 연결됨

  - Tier 1: a network that **can reach every other network** on the Internet **without purchasing** [IP transit](https://en.wikipedia.org/wiki/Internet_transit) or **paying** for peering(대륙-국가 간 인터넷 트래픽 교환)

  - Tier2는 1과 3을 연결해주며, Tier3이 개인이 흔히 사용하는 인터넷을 제공하는 ISP

    ![tier1](https://upload.wikimedia.org/wikipedia/commons/thumb/3/36/Internet_Connectivity_Distribution_%26_Core.svg/825px-Internet_Connectivity_Distribution_%26_Core.svg.png)





인터넷은 어떻게 동작하는가? [MDN](https://developer.mozilla.org/ko/docs/Learn/Common_questions/How_does_the_Internet_work)

1. 서버 쪽 호스트는 **DNS 서버**에게 요청하여 IP를 도메인 네임으로 바꿔 둠
2. 사용자는 ISP와 계약한 뒤 로컬 네트워크의 라우터를 ISP의 라우터와 연결함
3. **웹 브라우저** 작동
4. 클라이언트 로컬에서(**호스트 파일**) 접속할 도메인 이름을 찾음
5. 없으면 DNS에 해당 이름의 IP 주소를 요청, 응답을 받음
6. DNS가 제공한 IP주소를 이용해서 접속하여 통신





호스트란?

- 네트워크에 연결된 "컴퓨터"를 말함

  - 네트워크에 연결된 장치를 **노드**라 하고 **IP주소가 할당된 노드를 호스트**라 함

- 노드

  - 재분배 지점 또는 통신 종단점
  - 네트워크의 기본요소인 **지역 네트워크에 연결된 컴퓨터와 그 안에 속한 장비들**을 통틀어 일컫는 것
    - 예) 로컬 영역 네트워크 A에 컴퓨터 20대와 허브 2개 라우터 2개가 있을 때 네트워크A에 속한 장비들을 하나의 노드라고 함

- 호스트 파일(hosts)

  - 운영 체제가 **호스트 이름을 IP 주소에 매핑**할 때 사용하는 파일

  - 첫 문자 필드에는 IP 주소가, 그 다음에는 하나 이상의 호스트 이름이 위치함

    - ```
      127.0.0.1  localhost loopback
      ::1        localhost
      ```





## What is HTTP?





HTTP 프로토콜

- 웹상에서 데이터를 빠르게 교환하기 위해 **TCP를 기반으로 개발된 통신 프로토콜**
  - 원래 하이퍼텍스트 교환이 목적이었으나 최근에는 JSON, pdf등 이런저런 데이터를 다 이용함
  - HTTP 통신은 **Request**와 **Response**를 주고받으며 이루어짐





Request의 구성

- 요청라인 + 메시지 헤더 + (공백) + entity 바디
- 리퀘스트 라인: **HTTP method** + URL + version
  - HTTP method: GET, POST, PUT, PATCH, DELETE 등
  - version은 그냥 HTTP의 버전을 나타냄
- 메시지 헤더는 웹브라우저 종류/버전, 대응하는 데이터 타입 등을 기술
- entity 바디는 **POST에서 데이터를 담아 보낼 때** 사용





Response의 구성

- 상태라인(status) + 메시지 헤더 + (공백) + entity 바디
- 리스폰스 라인: 버전 + 상태 코드 + 설명문
  - [Response Code](https://developer.mozilla.org/ko/docs/Web/HTTP/Status)
  - 설명문은 상태 코드의 의미를 간단히 보여줌
- entity 바디에는 브라우저에 돌려보낼 데이터를 포함함
  - 주로 HTML 파일





HTTP 통신의 특징

- **비연결성**
  - 요청에 대해 서버가 **응답을 마치면 연결을 끊어버림**
  - 리소스를 아끼기 위해 비연결성을 띄는 것
    - 잦은 연결/해제는 오버헤드를 초래하기 때문
- **무상태** 
  - 비연결성으로 인해 클라이언트를 식별할 수 없는 것
  - 그럼 어떻게 식별하냐? : **쿠키/세션/토큰**을 이용하여 기억함





## Browsers and how they work?





웹 브라우저란?

- 웹 기반 컨텐츠(웹페이지)를 검색 및 열람하기 위한 **어플리케이션**
  - 서버에서 **컴포넌트 파일**(코드 파일 + 자원)을 가져옴
  - 코드 파일: HTML, CSS, JavaScript
  - 자원: 웹사이트를 만드는 모든 다른 것들 - 이미지, 음악, PDF 등





웹 브라우저의 기능

- 지원하는 기능에 따라 여러 가지 브라우저가 구분 됨
- 표준적으로 지원하는 기능
  - HTTP, HTTPS
  - HTML, XML, XHTML
  - 그래픽 파일 포맷 : GIF, PNG, JPEG 등
  - CSS
  - Javascript
  - Cookie
  - 디지털 인증서
  - 플러그인





웹 브라우저의 구성

![ㅇ](https://d2.naver.com/content/images/2015/06/helloworld-59361-1.png)

- **브라우저 엔진**
  - 웹 브라우저의 핵심 요소
  - HTML 문서와 기타 자원의 웹 페이지를 시각 표현으로 변환시킴
  - 문서들 간의 보안 정책을 강제하며 페이지 스크립트에 노출되는 **DOM 자료 구조를 구현**함
- **렌더링 엔진**
  - 
  - 렌더링 동작 과정
    - html가져옴 - **DOM트리 작성** - CSSOM트리 작성 - 2개 결합해 렌더트리 생성
    - DOM트리는 HTML태그들의 계층관계를 나타낸 것
    - 문서객체모델(DOM)에서 모든 HTML 태그는 객체로 취급
    - JS를 통해 페이지를 조작할 때 객체를 사용함
  - 객체들에게 위치,크기 지정(레이아웃) - CSS속성 적용 - 화면 업데이트
  - 도중에 JS를 발견하면 JS엔진 실행했다가 다시 DOM 생성함
- 통신
- **JS해석기**
  - JS 코드를 실행하는 프로그램 또는 인터프리터 : 자바스크립트 엔진이라고도 함
  - "대체로" 웹 브라우저에서 사용됨
    - JS 언어 자체가 원래 브라우저에 사용하기 위해 개발되었으나 현재는 그 외의 장소에서도 사용됨
    - JS 엔진의 구현체가 브라우저 엔진과는 분리되고 있는 것
- 임시파일저장소





## DNS and how it works?





Domain Name System

- 호스트의 도메인 이름을 네트워크 주소로 바꾸거나 반대의 변환을 수행하기 위해 개발된 시스템
  - IP가 아니라 이름으로 연결하게 해 줌 : **전화번호부**로 비유됨
- **도메인 네임 계층**을 관리하며 해당 네임 계층과 주소 공간 간의 변환 서비스를 제공함
  - 도메인을 위한 DNS 레코드를 저장하는 **DNS 서버**가 도메인 네임을 응답으로 제공함
  - PC는 설정된 DNS 서버에(**Local DNS 서버**﻿) 질의하여 IP 주소를 받아옴
    - 기업 : 보통 사내에 설치된 DNS 서버
    - 개인 : 통신 사용자가 제공하는 DNS





DNS의 구성 요소

- 도메인 네임 스페이스(Domain Name Space)
  - **도메인 이름을 트리 형태로 구성**한 것으로 DNS가 저장/관리함
  - 각 노드는 0개 이상의 리소스 레코드(resource record)를 가짐
  - 트리는 루트 존(root zone)에서 시작하여 여러 개의 하위 존으로 나뉨
    - 각 DNS 존은 하나의 **authoritative name server**에 의해 관리되는 노드들의 집합
    - 하나의 네임 서버가 여러 개의 존을 관리할 수도 있음
    - PC에서 질의한 서버에 호스트명이 없으면 루트까지 거슬러 올라가며 **재귀질의**함
- 네임 서버(Name Server)
  - 리졸버로부터 요청 받은 도메인 이름에 대한 **IP 정보를 다시 리졸버로 전달**해주는 역할을 수행하는 서버
    - 도메인 네임 스페이스의 트리 구조에 대한 정보를 가지고 있음
    - 도메인 이름을 IP 주소로 변환하는 것을 **네임 서비스**라고 하며 이를 제공하는 것이 네임 서버
  - Primary, Secondary로 구분됨
    - Primary 네임 서버 :  해당 도메인을 관리하는 주 네임 서버
    - Secondary 네임 서버 : Primary 네임 서버의 백업 역할을 하는 서버
    - Secondary 네임 서버는 주기적으로 Primary 네임 서버로부터 정보를 받아와 자신의 정보를 갱신하여 전체 네임 서버의 정보가 일관성 있게 유지 및 관리되도록 함
- 리졸버(Resolver)
  - DNS 클라이언트의 요청을 네임 서버로 전달하고, 도메인 이름과 IP 주소를 받아 클라이언트에게 제공
    - 하나의 네임 서버에게 DNS 요청을 전달하고 해당 서버에 정보가 없으면 다른 네임 서버에게 요청
    - 질의한 정보는 한동안 서버와 리졸버의 캐시에 저장됨
  - 스터브 리졸버(Stub Resolver)
    - 클라이언트 호스트에 **단순한 기능만을 지닌 리졸버 루틴**을 구현한 것
    - 시스템 자원의 한계가 있기 때문에 리졸버의 대부분의 기능은 DNS 서버에 구현하고 클라이언트에는 스터브 리졸버만을 둠 : 수 많은 네임 서버의 구조를 파악할 필요 없음
    - 리졸버는 OS에 내장되어 있음





DHCP란?

- Dynamic Host Configuration Protocol
- 호스트의 IP주소와 각종 TCP/IP 프로토콜의 기본 설정을 자동 제공
  - 인터넷에 연결하는 순간 DHCP가 **DNS서버의 주소를 세팅**하여 주소를 질의할 수 있도록 함
  - 할당된 IP주소는 임시적임
  - 자동이므로 편리하나 DHCP 서버가 다운되면 IP 할당이 안됨





## What is Domain Name?





도메인 네임이란?

- 넓은 의미 : 네트워크상에서 컴퓨터를 식별하는 **호스트명**
  - 호스트명 : 네트워크에 연결된 장치(컴퓨터, 파일 서버, 복사기 등)들에게 부여되는 고유한 이름
  - 사람이 읽고 이해할 수 있는 이름으로, IP 주소나 MAC 주소와 같은 기계적인 이름 대신 쓸 수 있음
  - 일반적으로 호스트명이라고 하면 **인터넷 상에서의 호스트명**을 가리킴(좁은 의미에서의 도메인 네임)
- 좁은 의미 : **도메인 레지스트리**에게서 등록된 이름
  - 인터넷에 연결된 호스트의 이름으로, 보통 호스트의 지역 이름에 도메인 이름을 붙인 것
    - 예) ‘ko.wikipedia.org’ : 도메인 이름 ‘wikipedia.org’, 지역 이름 ‘ko’를 붙여 호스트 이름을 만듦
  - 도메인 레지스트리 : 최상위 도메인에 등록된 모든 도메인 네임의 데이터베이스
  - **레지스트리 운영자**는 도메인 네임 데이터베이스를 유지하고 도메인 네임을 IP 주소로 변환해주는 존 파일을 생성
    - **네트워크 정보 센터** (NIC)라고도 불리며 DNS의 한 부분임
    - NIC는 자신이 관할하는 **최상위 도메인**의 도메인 네임 등록, 도메인 네임 정책 수립, 그리고 그 최상위 도메인 운영 시스템을 관리 하는 기관
  - 도메인 네임은 DNS 구조의 최상위인 루트 네임 서버 정보를 관리하는 IANA에 의해 계층 구조로 관리됨
    - Internet Assigned Numbers Authority : IP 주소, 최상위 도메인 등을 관리하는 단체





최상위 도메인이란?

- Top-level domain(TLD) : 인터넷에서 **도메인 네임의 가장 마지막 부분**
- 일반 최상위 도메인
  - com, net, org, edu, gov, mil 등
  - 초기에 위의 6개의 일반 최상위 도메인(gTLD)이 사용되었고 그 뒤 필요에 따라 도메인이 추가됨
- 국가 코드 최상위 도메인
  - Country Code Top-Level Domain(ccTLD) : 각 나라, 지역 또는 국제단체에 주어진 인터넷의 도메인 이름
  - 일반적으로 각 나라의 영어식 이름을 줄인 것





## What is hosting?





인터넷 호스팅 서비스란?

- 인터넷 **서버를 운영**하는 서비스 : 단체와 개인이 콘텐츠를 인터넷에 제공하는 것을 도와줌
- 호스팅 서비스의 종류
  - Full-featured hosting services
    - 모든 기능을 갖춘 호스팅 서비스
    - Dedicated hosting service : 서버를 소유한 업체가 고객에게 빌려주고 유지보수도 대행함
    - Colocation service : 고객에게 **데이터 센터**의 일정 공간과 회선을 임대해주는 서비스
    - **Cloud hosting** : 고객은 시스템의 자원(시간, 컴퓨팅 파워 등)을 사용한 만큼만 지불함
  - 기타
    - 애플리케이션 별로 호스팅 서비스를 제공하는 등 제한적인 서비스
    - 웹 호스팅 서비스, 파일 호스팅 서비스, 전자메일 호스팅 서비스 등





데이터 센터란?

- 서버 컴퓨터와 네트워크 회선 등을 제공하는 건물이나 시설 : 서버 호텔이라고도 함
- 구조
  - 건물의 각 층마다 사용자 그룹별로 케이지(cage)를 설치
  - 케이지 안에 여러 개의 랙(rack)을 설치
  - 각 랙마다 스위치(switch)를 두고 여러 대의 서버 컴퓨터를 연결
- 중단 없는 서비스를 제공하기 위해 안정적인 전력 공급과 인터넷 연결 및 보안이 중요