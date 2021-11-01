# TCP/IP 응용(애플리케이션) 계층과 URL 구성 요소 및 종류

출처 : [쉽게 이해하는 네트워크](https://better-together.tistory.com/144?category=887984)

> 애플리케이션 계층과 URL의 형식 및 종류의 이해


- [TCP/IP 응용(애플리케이션) 계층과 URL 구성 요소 및 종류](#tcpip-응용애플리케이션-계층과-url-구성-요소-및-종류)
  - [응용(애플리케이션) 계층의 특징과 애플리케이션](#응용애플리케이션-계층의-특징과-애플리케이션)
  - [응용(애플리케이션) 계층의 동작 방식과 프로토콜](#응용애플리케이션-계층의-동작-방식과-프로토콜)
  - [URL](#url)
    - [URL의 의미](#url의-의미)
    - [파일 경로(path)](#파일-경로path)
    - [URL의 형식(URL의 구성 요소)](#url의-형식url의-구성-요소)
    - [절대 URL 과 상대 URL](#절대-url-과-상대-url)
    - [클라이언트의 특정 서비스 요청](#클라이언트의-특정-서비스-요청)






<br/><br/><br/>

## 응용(애플리케이션) 계층의 특징과 애플리케이션

TCP/IP 모델의 4 계층 중 네트워크 인터페이스 계층, 인터넷 계층, 전송 계층이 데이터 전송을 담당하고, **응용 계층은 서비스를 제공하기 위한 데이터를 만들거나, 수신한 데이터의 내용을 보고 그에 맞는 서비스를 사용자에게 제공합니다.** 한마디로 **통신의 목적인 서비스를 실현하기 위해 서비스의 종류나 동작 방식 등을 결정하는 것이 응용 계층의 역할**입니다. 따라서 **응용 계층의 역할은 서비스를 제공하는 서버 애플리케이션과 서비스를 요청하고 이용하는 클라이언트 애플리케이션이 구현**합니다.

**운영체제** 위에서 동작하는 **애플리케이션은 데이터 전송 기능이 없기 때문에 서비스를 요청하거나 서비스를 제공하기 위한 데이터를 만들어낼 뿐, 데이터를 전송하는 것은 운영체제에게 위임합니다.** 보다 정확히는 운영체제에 내장된 **전송 계층 이하의 프로토콜을 구현하는 TCP/IP 소프트웨어**에게 **데이터 전송 처리를 부탁합니다.** 이처럼 운영 체제에서 데이터 전송을 알아서 처리해주기 때문에 애플리케이션 개발자는 데이터 전송을 신경 쓰지 않고 서비스 구현에 집중할 수 있습니다.

<br/><p align = "center">
<img src = "https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fb0k2Fy%2FbtqOVJ2O72y%2FFfKl5xoL3U4MKjksek8uDK%2Fimg.png">
</p>
<p align = "center">
<sup><b><그림 1> 응용 계층과 애플리케이션</b></sup>
</p><br/>

복잡한 네트워크 시스템을 계층화한 덕분에 각 계층을 구현하는 소프트웨어를 개발하는 개발자도 각자 필요한 계층의 역할과 프로토콜만 이해해서 프로그래밍을 하면 되는 것입니다.

 




<br/><br/><br/>

## 응용(애플리케이션) 계층의 동작 방식과 프로토콜

**응용 계층은 서비스를 요청하는 클라이언트와 서비스를 제공하는 서버가 서로 메시지를 보내는 방식으로 동작합니다.** 응용 계층은 **서버와 클라이언트가 메시지라는 데이터 단위를 사용하여 요청(request)과 응답(response)을 주고받으면서 서비스를 구현**합니다.

<br/><p align = "center">
<img src = "https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FBgfgu%2FbtqOUlODCVs%2FAIe0uwbdhprdUz1yrR1lD0%2Fimg.png">
</p>
<p align = "center">
<sup><b><그림 2> 응용 계층의 동작 방식과 프로토콜</b></sup>
</p><br/>

**클라이언트와 서버가 어떤 방식으로 요청과 응답을 할 것인지는 응용 계층의 프로토콜이 정합니다.** **서비스마다 필요한 기능이 다르기 때문에 고유한 프로토콜이 존재**합니다. 따라서 응용 계층에는 **다른 계층과 달리 서비스 종류만큼 다양한 프로토콜이 있습니다.** 인터넷에서 사용되는 서비스 중 가장 대표적인 웹 서비스(WWW)는 HTTP라는 프로토콜을 사용합니다.





<br/><br/><br/>

## URL

<br/>

### URL의 의미

**서비스마다 서비스를 요청하고 응답하는 방식을 정한 고유한 프로토콜을 사용하지만**, **클라이언트가 서버에게 특정 서비스를 요청할 때 공통적으로 URL을 사용**합니다.

**URL(Uniform Resource Locator)은 인터넷에 존재하는 리소스(Resource, 자원)의 위치를 지정하는 방법**입니다.

자원을 의미하는 리소스는 어떤 일에 이용되는 여러 가지 것들을 총칭하는 말입니다. URL의 리소스는 인터넷에서 서비스를 제공하기 위해 이용되는 문서, 이미지, 음성, 동영상 파일 등을 의미합니다.

다시 말해 URL(Uniform Resource Locator)은 인터넷에서 다양한 종류의 서비스를 제공하는 서버 컴퓨터에 저장된 파일의 위치를 표시하는 주소이며, 이 주소로 클라이언트가 원하는 서비스를 특정하게 됩니다.

서버는 특정 서비스를 제공하기 위한 리소스를 저장하고, 이 리소스가 저장된 위치인 URL을 공개하여 클라이언트가 URL을 통해 원하는 서비스를 요청할 수 있도록 합니다.

예를 들어, 웹 서비스를 제공하는 웹 서버는 자신만의 특징이 담긴 서비스를 제공하는 리소스(index.html과 같이 파일 확장자가 html 파일 등)가 저장된 위치인 URL을 공개하고, 웹 서비스를 요청하는 클라이언트, 즉 웹 브라우저는 원하는 서비스의 리소스가 저장된 URL을 입력함으로써 그 서비스를 이용하겠다는 의사를 전달하는 것입니다.

<br/><p align = "center">
<img src = "https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcBxb9B%2FbtqO0gGODqJ%2F65QTvBGgI8JPgPQ2d9XSL1%2Fimg.png">
</p>
<p align = "center">
<sup><b><그림 4> URL의 의미</b></sup>
</p><br/>



<br/>

### 파일 경로(path) 

내 컴퓨터에 저장된 파일의 위치인 파일의 경로를 '폴더명/파일명'으로 표시합니다. 즉, 폴더명과 파일명으로 파일의 경로를 표시하며 폴더는 또 다른 폴더를 포함할 수 있습니다. 

서버 컴퓨터에서도 마찬가지로 폴더명과 파일명으로 파일의 경로를 표시합니다. 다만 폴더 대신 디렉토리(Directory)라는 개념을 사용합니다. 폴더와 디렉토리는 파일의 묶음을 관리하는 논리적인 단위로 거의 동일한 개념입니다. 다만, 디렉토리를 사용할 때는 디렉토리 간에 위아래 계층구조가 있다는 것을 강조합니다. 즉, 디렉토리가 도메인의 구조처럼 역 트리 구조로 구성되어 최상위 디렉토리를 루트라고 부르고, 상위 디렉토리를 부모 디렉토리, 하위 디렉토리를 자식 디렉토리라고 부릅니다.

아이콘을 클릭하는 방식의 GUI(Graphic User interface) 환경에서는 파일 간의 관계가 그래픽에 의해 시각적으로 드러나기 때문에 주로 폴더로 파일 경로를 지정합니다. 대표적으로 GUI를 사용하는 윈도우가 폴더를 사용합니다. 반면 문자로 이루어진 명령어를 사용하는 CUI(Character User Interface) 환경에서는 파일 간의 관계를 분명히 하기 위해 계층구조를 담고 있는 디렉토리를 사용합니다. CUI 기반인 리눅스나, 윈도우에서도 CUI 방식을 사용할 경우는 디렉토리를 사용합니다.

<br/><p align = "center">
<img src = "https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FEOLCS%2FbtqOSxhMg7w%2FubVqDRjYEK1F9O3apqdJ0K%2Fimg.png">
</p>
<p align = "center">
<sup><b><그림 5> 파일 경로</b></sup>
</p><br/>

주로 CUI 방식의 운영체제를 사용하는 서버 컴퓨터에 저장된 파일의 경로는 '디렉토리명/파일명'으로 표시합니다.

내 컴퓨터에서는 파일 경로만 알면 파일에 접근할 수 있지만 클라이언트가 서버 컴퓨터에 저장된 파일에 접근하기 위해서는 파일 경로 뿐만 아니라 서버 컴퓨터의 위치도 알아야 합니다. 그래서 서버 컴퓨터에 저장된 파일의 위치를 표시하는 URL은 파일의 경로 외에 서버 컴퓨터의 위치를 알려주는 정보가 추가로 필요합니다.

 


<br/>

### URL의 형식(URL의 구성 요소)

URL은 애플리케이션이 제공하는 서비스의 종류를 식별하는 스킴(scheme), 인터넷에서 사용하는 서버 주소인 도메인(또는 IP 주소), 애플리케이션을 식별하는 포트 번호와 파일 경로로 구성됩니다. 웰 노운 포트를 사용하는 서버 애플리케이션은 서비스의 종류로 포트 번호를 알 수 있기 때문에 포트 번호를 생략하는 경우가 많습니다.

<br/><p align = "center">
<img src = "https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FGVbkI%2FbtqON74ZJBU%2FmGZ4hw1dpk0tZvNl4yfvZ0%2Fimg.png">
</p>
<p align = "center">
<sup><b><그림 6> URL</b></sup>
</p><br/>

서비스 종류를 나타내는 스킴은 주로 응용 계층의 프로토콜로 표시합니다. 스킴에 따라 :// 뒤에 이어지는 문자열의 형식이 변경되기도 합니다. 참고로 '//' 는 아무 의미가 없습니다. 

인터넷에서 가장 많이 사용하는 웹서비스는 HTTP 프로토콜을 따르기 때문에 http를 스킴으로 사용합니다. 웹 브라우저 주소 창에 http://로 시작하는 URL을 입력하기 때문에 **URL이 웹 서비스에서만 사용된다고 생각하기 쉬운데 웹 서비스뿐만 다른 서비스에서도 ftp, file, mailto 등의 스킴을 가진 URL을 사용합니다.**

웹브라우저는 웹 서비스의 클라이언트로 사용하는 경우가 많지만, 파일을 다운로드/업로드하는 FTP 클라이언트 기능이나 메일의 클라이언트 기능도 갖고 있습니다. 따라서 웹 브라우저의 주소 창에 ftp 스킴으로 시작되는 URL을 입력하여 FTP 서버에 접근할 수도 있습니다. 이런 면에서 웹 브라우저는 몇 개의 클라이언트 기능을 갖고 있는 복합적인 클라이언트 애플리케이션이라고 할 수 있습니다.

<br/><p align = "center">
<img src = "https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F4Txd4%2FbtqOUk93EKg%2FHKoaqrL9zHEfK9QOOxdDxk%2Fimg.png">
</p>
<p align = "center">
<sup><b><그림 7> 스킴의 종류</b></sup>
</p><br/>

[스킴 종류 자세히](https://www.iana.org/assignments/uri-schemes/uri-schemes.xhtml)

서버의 주소는 인터넷에서 사용하는 컴퓨터의 주소인 IP 주소를 사용할 수도 있지만, IP 주소보다는 기억하기 편하게 IP 주소를 문자로 변환한 도메인(또는 호스트 이름)을 많이 사용합니다.




<br/>

### 절대 URL 과 상대 URL
URL은 절대 URL과 상대 URL 두 가지로 나뉩니다. 위에서 살펴본 URL, 즉 스킴부터 파일 경로까지 리소스에 접근하는데 필요한 모든 정보를 가지고 있는 URL을 절대 URL(Absolute URL)이라고 합니다.

절대 URL을 짧게 표기하여 리소스에 접근하기 위한 일부 정보만을 담고 있는 URL을 상대 URL(Relative URL)이라고 합니다.

같은 스킴과 같은 도메인을 갖고 있는 웹 서버의 리소스에 접근하기 위해 리소스에 접근할 때마다 http부터 시작하는 절대 URL을 모두 입력하는 것은 비효율적입니다. 이런 비효율성을 해결하기 위해 웹 서버가 제공하는 리소스에 공통적으로 사용되는 URL 부분을 생략한 상대 URL을 사용해 리소스에 접근할 수 있도록 만든 것입니다. 즉, **스킴이나 도메인 같이 웹 서버가 제공하는 리소스에 공통적으로 사용되는 URL 부분**을 **기준 URL(기저 URL, Base URL)**로 정하고, **절대 URL에서 기준 URL을 생략한 나머지 부분만을 표시하는 것이 상대 URL입니다**. 기준 URL로부터 디렉토리 이하의 상대적인 경로만을 표시한다는 의미에서 상대 URL이라고 부릅니다.

<br/><p align = "center">
<img src = "https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fcc7Eny%2FbtqP0sAtE75%2FxXPItJBKDJ5Z4RkmKTc5IK%2Fimg.png">
</p>
<p align = "center">
<sup><b><그림 8> 절대 URL과 상대 URL</b></sup>
</p><br/>

클라이언트가 처음 웹 서버에 접속할 때는 절대 URL을 사용해야겠지만, 웹 서버에 접속한 이후 웹 서버의 리소스를 사용할 때, 다시 말해 웹 사이트 내에서 하이퍼링크로 웹 페이지를 이동할 때에는 길이가 짧은 상대 URL을 사용해서 웹 사이트를 만드는 것이 효율적입니다. 따라서 웹 서버 개발자(정확히는 프런트엔드 개발자)가 HTML 문서에 같은 웹 서버에 있는 리소스를 하이퍼링크로 삽입할 때 상대 URL을 사용합니다.

**웹 브라우저 사용자가 상대 URL로 웹 서버의 리소스에 접근할 수 있는 것은 웹 브라우저가 기본 URL을 사용해 상대 URL을 절대 URL로 변환하는 기능을 갖고 있기 때문**입니다. 웹 브라우저는 리소스에서 명시적으로 제공*하는 기본 URL을 사용하거나, 기본 URL이 명시되어 있지 않은 경우 절대 URL을 가진 다른 리소스에서 기본 URL을 추출하여, 상대 URL을 절대 URL로 변환한 후 리소스에 접근하는 역할을 합니다.

*. HTML 문서의 Head 태그 내에 base 태그를 사용하여 기본 URL을 기술합니다.

<br/><p align = "center">
<img src = "https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbeqtZ1%2FbtqP9rgc0JN%2Fq7sbQjthx0qj4z6GGUcx4K%2Fimg.png">
</p>
<p align = "center">
<sup><b><그림 9> 웹 브라우저의 URL의 변환</b></sup>
</p><br/>

상대 URL을 사용하면 수많은 파일로 구성된 리소스 집합의 위치를 쉽게 변경할 수 있습니다. 웹 서버를 이전하거나 도메인을 바꾸더라도 기본 URL만 변경하면 새로운 기본 URL에 따라 웹 브라우저가 리소스의 절대 URL을 생성해 주기 때문입니다.

 



<br/>

### 클라이언트의 특정 서비스 요청

클라이언트는 서버에게 서비스를 요청할 때는 URL을 사용하여 원하는 서비스를 특정합니다.

클라이언트가 '`http://www.better-together.com/network/index.html`'이라는 URL을 사용하여 서비스를 요청한 경우 이를 해석하면, 인터넷에서 `www.better-together.com`이라는 주소를 가진 웹(http) 서버에 설치된 포트 번호 80번(http로 식별한 웹 서버의 웰 노운 포트)을 가진 애플리케이션이 만든 network라는 디렉토리안에 있는 index.html이라는 파일을 전송해 달라는 요청입니다.

사용자는 보통 포털 사이트에서 검색 결과에 나타난 링크를 클릭하여 서버에 접속합니다. 이 경우에는 링크에 내장된 URL로 이동하는 것일뿐, 주소 창에 내장된 URL을 직접 입력하여 이동하는 것과 차이가 없습니다.

링크 접속 외에 보통 사용자는 스킴과 포트 번호 이하가 생략된 `www.google.com`과 같은 URL을 웹 브라우저의 주소 입력 창에 입력합니다. 웹 브라우저가 스킴이 반영된 올바른 URL 형태인 `http://www.google.com`으로 변환을 하고, 자동적으로 웹 서비스의 표준 포트인 80번 포트를 사용합니다. 따라서 `www.google.com`은 파일 경로명이 생략된 URL을 입력한 것이 됩니다.

이렇게 파일 경로명이 아무 것도 없는 경우에는 서버 애플리케이션의 최상위 디렉토리인 루트 디렉토리 아래에 저장되어 있는 파일에 접근하게 됩니다. 서버 측에서 파일경로가 지정되지 않았을 때 클라이언트에게 보여줄 웹 페이지를 미리 설정된 파일명, 주로 `index.html`이라는 파일명으로 루트 디렉토리에 저장해 놓기 때문입니다. 따라서 사용자가 `www.google.com`라고 입력한 URL은 `www.google.com/index.html`와 동일한 의미를 갖습니다.

<br/><p align = "center">
<img src = "https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FMF7Pc%2FbtqOS8hMPp6%2FTgHI0rW9sgPub2EVVdZKT0%2Fimg.png">
</p>
<p align = "center">
<sup><b><그림 10> 포트 번호 이하가 생략된 URL</b></sup>
</p><br/>
 



