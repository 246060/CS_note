# 도메인 의미와 계층 구조 및 DNS 네임 서버


출처 : [쉽게 이해하는 네트워크](https://better-together.tistory.com/128?category=887984)


> 도메인과 DNS 네임 서버의 이해 


- [도메인 의미와 계층 구조 및 DNS 네임 서버](#도메인-의미와-계층-구조-및-dns-네임-서버)
  - [도메인이란?](#도메인이란)
  - [DNS란?](#dns란)
  - [도메인의 계층 구조](#도메인의-계층-구조)
  - [DNS 서버의 질의 과정](#dns-서버의-질의-과정)
  - [도메인 등록 대행](#도메인-등록-대행)





<br/><br/><br/>

## 도메인이란?
구글이나 네이버가 인터넷 공개한 IP 주소 대신 사용하는 `www.google.com`이나 `www.naver.com` 같은 형태의 영문 주소를 도메인(Domain) 또는 도메인 이름(Domain name, 또는 호스트 네임, Host name)이라고 합니다.

즉, 도메인이란 숫자 형태의 IP 주소를 사람이 기억하기 쉬운 문자로 표현한 주소입니다.

<br/><p align = "center">
<img src = "https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fb0r55g%2FbtqN014kHQs%2FTTCKKIQVaY5aj5Pp8mIZQK%2Fimg.png">
</p>
<p align = "center">
<sup><b><그림 1> IP 주소와 도메인</b></sup>
</p><br/>






<br/><br/>

## DNS란?

휴대폰 연락처와 같이 도메인과 IP 주소를 저장해 놓은 시스템을 DNS(Domain Name System)라고 합니다. 즉 DNS는 전 세계의 IP 주소에 대응하는 도메인을 효율적으로 관리하기 위해 개발된 시스템입니다. IP 주소와 도메인을 저장하고 관리하는 컴퓨터나 애플리케이션을 DNS 서버라고 합니다. 다시 말해 DNS 서버는 IP 주소와 도메인을 저장하고 맵핑(mapping)하는 일종의 데이터베이스입니다.

학교나 기업의 네트워크에서 자체적으로 DNS 서버를 만들어 운영하기도 하고, ISP가 관리하는 DNS 서버를 사용할 수도 있습니다. 이러한 DNS 서버들이 전 세계의 모든 호스트들의 IP 주소와 도메인을 관리하며 IP 주소를 도메인으로 변환하거나 도메인을 IP 주소로 변환하는 DNS 서비스를 제공하고 있습니다.

따라서 도메인에 대응하는 IP 주소를 알고 싶거나 IP 주소에 대응하는 도메인을 알고 싶으면 DNS 서버에게 물어보면 됩니다.

<br/><p align = "center">
<img src = "https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FlcZ3G%2FbtqN02hRxja%2FBDkIQYRXa00xHWlmYCHf51%2Fimg.png">
</p>
<p align = "center">
<sup><b><그림 2> DNS 서버의 역할</b></sup>
</p><br/>

여러분이 웹 브라우저의 주소창에 구글의 도메인 이름 www.google.com을 입력하고 구글의 웹사이트로 이동하는 짧은 순간에 여러분의 컴퓨터가 DNS 서버에 IP 주소를 문의하고 응답받은 IP 주소로 접속하는 일련의 과정이 <그림 3>와 같이 일어납니다.

<br/><p align = "center">
<img src = "https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbmscfJ%2FbtqNWgu7xW5%2FtmasTllwisgKmC49ck9ktK%2Fimg.png">
</p>
<p align = "center">
<sup><b><그림 3> DNS 서버에 IP 주소 질의</b></sup>
</p><br/>

DNS 서버에 IP 주소를 질의하기 위해서는 DNS 서버의 IP 주소를 내 컴퓨터가 알고 있어야 합니다. 보통은 ISP에서 기본적으로 제공하는 DNS 서버의 IP 주소가 내 컴퓨터에 설정되어 있습니다.

 




<br/><br/><br/>

## 도메인의 계층 구조
인터넷에서 사용되는 도메인은 IP 주소와 마찬가지로 전 세계적으로 고유하게 존재하여야 하므로 임의로 만들거나 변경할 수 없습니다. 따라서 도메인 이름을 지을 때는 전 세계에서 공통적으로 적용되는 몇 가지 규칙을 따라야 합니다.

- 숫자, 영문자 조합
- 특수문자 사용할 수 없음
- 256자 이상 불가능
- 이미 등록된 도메인 사용 불가능

*. 최근에는 한글로 되어 있는 도메인 이름도 볼 수 있는데, 이는 한글 도메인 이름을 기본 도메인으로 바꿔 주는 서비스를 이용하는 것일 뿐 DNS에서 사용하는 기본 도메인 이름은 아닙니다.

또한, 도메인의 체계적인 분류와 관리를 위해 도메인 이름은 몇 개의 짦은 영문자를 '. (닷, 점)'으로 연결한 **계층 구조**를 갖고 있습니다. <그림 4> 과 같이 도메인의 계층 구조는 나무를 거꾸로 한 것 같은 모양으로 되어 있어 '**역 트리(Inverted tree) 구조**'라고 하며 트리 구조의 정점을 루트(root, 뿌리)라고 합니다.

루트 아래로 갈라지는 가지를 단계별로 구분하여 'kr'과 같이 국가를 나타내는 국가 코드 도메인(ccTLD, country code Top Level Domain)이나 'com' 같이 등록인의 목적에 따라 사용되는 일반 도메인(gTLD, generic Top Level Domain)을 **1단계 도메인**(또는 최상위 도메인, 탑레벨 도메인, Top Level domain, TLD)이라고 합니다.

1단계 도메인의 하위 도메인인 **2단계 도메인**(또는 서브 도메인, Sub domain)에는 조직의 속성을 구분하는 'co'(영리 기업), 'go'(정부 기관), 'ac'(대학)과 같은 도메인이 있습니다.

2단계 도메인 아래 **3단계 도메인**은 조직이나 서비스의 이름을 나타내는 도메인 이름으로 도메인 사용자가 원하는 문자열을 사용할 수 있습니다.

그리고 **마지막**은 **컴퓨터의 이름을 나타내는 호스트**(Host)가 위치합니다.

도메인을 표기할 때는 낮은 단계부터 표현하여 최상위 도메인이 가장 뒤에 나타납니다.

<br/><p align = "center">
<img src = "https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FdE6y10%2FbtqOP4gTdWD%2FOvcpFElloTi1FFBFLteYbk%2Fimg.png">
</p>
<p align = "center">
<sup><b><그림 4> 도메인의 계층 구조</b></sup>
</p><br/>

도메인은 도메인 계층 구조를 반영한 네임 서버(네임 서버를 DNS 서버라고도 부릅니다)에 저장, 관리됩니다. **각 네임 서버는 도메인 계층의 일부 영역(zone)을 담당하고 그 영역에 속한 도메인을 관리합니다. 상위 계층의 네임 서버는 하위 계층의 도메인에 대한 정보를 관리하고 하위 계층 네임 서버의 IP 주소를 갖고 있습니다. 최상위 계층인 루트 네임 서버의 IP 주소는 모든 DNS 서버가 등록하여 관리하고 있습니다.**

<그림 5>의 루트 네임 서버에서는 하위 계층인 kr이나 com의 정보를 관리하고 kr이나 com을 관리하고 있는 네임 서버의 IP 주소가 등록되어 있습니다.

kr의 네임 서버에서는 하위 계층인 co.kr이나 go.kr의 정보를 관리하고 co.kr이나 go.kr 을 관리하고 있는 네임 서버의 IP 주소가 등록되어 있습니다.

go.kr의 네임 서버에서는 하위 계층인 korea.go.kr의 정보를 관리하고 korea.go.kr을 관리하고 있는 네임 서버의 IP 주소가 등록되어 있습니다.

<br/><p align = "center">
<img src = "https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fm3SSL%2FbtqNZMM62oH%2FYqEKX3MBYyGVXry1Mu4vrK%2Fimg.png">
</p>
<p align = "center">
<sup><b><그림 5> 네임 서버의 계층 구조</b></sup>
</p><br/>

이처럼 DNS는 전 세계의 수많은 도메인을 효율적으로 저장하고 관리하기 위해서 **도메인을 계층화하고, 계층의 일부 영역을 네임 서버가 분산 관리하는 시스템**으로 설계, 운영되고 있습니다.

 




<br/><br/><br/>

## DNS 서버의 질의 과정
모든 DNS 서버는 IP 주소를 알고 있는 루트 네임 서버에 접속하여 IP 주소를 찾기 시작합니다. 루트 네임 서버부터 도메인 계층을 따라서 질의 과정을 반복하며 찾고자 하는 도메인의 IP 주소를 찾습니다.

위 <그림 3>의 DNS 서버 질의의 상세한 과정은 아래 <그림 6>와 같습니다.

사용자가 도메인 이름 `www.google.com`을 입력하면 사용자의 컴퓨터는 DNS 서버에게 도메인 이름 `www.google.com`의 IP 주소를 문의합니다. DNS 서버는 IP 주소를 알고 있는 루트 네임 서버에 IP주소를 조회합니다. 

'`com`' 정보를 등록하고 있는 루트 네임 서버는 '`www.google.com`'의 IP 주소를 '`com`' 네임 서버에 문의하라고 DNS 서버에게 '`com`' 네임 서버의 IP 주소를 알려 줍니다.

DNS 서버는 루트 네임 서버가 알려준 '`com`' 네임 서버의 IP 주소로 '`com`' 네임 서버에게 '`www.google.com`'의 IP 주소를 질의해서 '`google.com`' 네임 서버의 IP 주소를 얻습니다.

최종적으로 DNS 서버는 '`google.com`' 네임 서버에 질의해서 `www.google.com`의 IP 주소를 얻어 사용자의 컴퓨터에 `www.google.com`의 IP 주소를 알려 줍니다. 

즉, 각 네임 서버가 IP 주소를 알고 있는 하위 네임 서버를 알려주는 과정을 반복하다 보면 최종적으로 DNS 서버가 알고자 IP 주소를 알고 있는 네임 주소에 도달해 IP 주소를 얻게 됩니다.

<br/><p align = "center">
<img src = "https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbwDkIw%2FbtqNVTGV9Ar%2FD6w3guBAUsphSffuQagCkk%2Fimg.png">
</p>
<p align = "center">
<sup><b><그림 6> DNS 서버와 네임 서버에 의한 IP 주소 조회</b></sup>
</p><br/>

이렇게 매번 루트 네임 서버에서부터 도메인의 트리 구조를 따라 순서대로 IP 주소를 찾아가는 과정을 반복하는 것은 효율적이지 않습니다. 그래서 **DNS 서버는 질의한 정보를 한동안 캐시(cache)에 저장하여 같은 질의가 들어오면 루트 네임 서버까지 가지 않고 바로 IP 주소를 알려 줍니다.**







<br/><br/><br/>

## 도메인 등록 대행
도메인 이름도 공인 IP 주소와 마찬가지로 ICANN에서 관리하고 있습니다. 그래서 IP 주소에 대응하여 사용하고 싶은 도메인이 있다면 관리 기관에 등록해야 인터넷에서 사용할 수 있습니다.

ISP 가 IP 주소 할당을 대행하는 것처럼 도메인 등록도 대행업체가 대행합니다. 국내에서도 국내 도메인을 관리하는 한국인터넷정보센터(KRNIC)에서는 직접 도메인 등록 신청을 받지 않고 한국인터넷정보센터와 도메인 등록 대행 계약을 맺은 가비아, 후이즈 등의 대행업체가 등록 대행을 합니다.