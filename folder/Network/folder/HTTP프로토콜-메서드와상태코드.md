# HTTP 프로토콜 - 메서드와 상태 코드

출처 : [쉽게 이해하는 네트워크](https://better-together.tistory.com/149?category=887984)


- [HTTP 프로토콜 - 메서드와 상태 코드](#http-프로토콜---메서드와-상태-코드)
  - [HTTP란?](#http란)
  - [HTTP 메시지](#http-메시지)
    - [HTTP 요청 메시지 시작줄 - 요청줄](#http-요청-메시지-시작줄---요청줄)
      - [HTTP 버전이란?](#http-버전이란)
    - [HTTP 응답 메시지 시작줄 - 응답줄(상태줄)](#http-응답-메시지-시작줄---응답줄상태줄)
      - [HTTP 상태 코드](#http-상태-코드)
      - [HTTP 응답줄](#http-응답줄)
      - [HTTP 헤더(헤더 필드)](#http-헤더헤더-필드)
        - [요청 헤더](#요청-헤더)
        - [응답 헤더](#응답-헤더)
        - [일반 헤더](#일반-헤더)
        - [엔터티 헤더](#엔터티-헤더)
        - [확장 헤더](#확장-헤더)
      - [HTTP 바디(본문)](#http-바디본문)






<br/><br/><br/>

## HTTP란?
HTTP는 HyperText Transfer Protocol의 약자로, 인터넷에서 하이퍼텍스트 문서인 HTML로 만든 웹페이지를 전송하기 위해 사용되는 프로토콜입니다. 즉, HTTP는 웹 브라우저와 웹 서버가 통신하는 절차와 형식을 규정한 것입니다. 구체적으로 HTTP는 웹 브라우저가 웹 서버에게 웹 페이지를 요청(리퀘스트, Request)하는 방식과 웹 서버가 웹 브라우저에게 응답(리스폰스, Response)하는 방식을 정합니다.

HTTP 통신은 반드시 웹 브라우저의 요청이 있어야 시작됩니다. 웹 서버가 요청을 받지 않고 응답을 하는 경우는 없습니다.

 







<br/><br/><br/>

## HTTP 메시지 
**응용 계층의 데이터 단위**는 메시지(Message)입니다. 응용 계층에서 HTTP로 통신하는 애플리케이션이 전송하는 데이터 단위를 HTTP 메시지라고 합니다. **HTTP 메시지**는 서비스를 제공하고 이용하기 위해 **애플리케이션이 만든 데이터에 헤더를 붙여 캡슐화한 것**입니다. HTTP 메시지는 웹 브라우저나 웹 서버 애플리케이션 같은 HTTP를 사용해 통신하는 애플리케이션이 만듭니다.

**헤더에는 각 계층에 적용되는 프로토콜이 데이터를 처리하여 계층의 역할을 수행할 때 필요한 정보가 담깁니다.** 다른 계층과 마찬가지로 **HTTP 헤더에도 HTTP 프로토콜이 요청과 응답이라는 형식으로 데이터를 처리하기 위한 정보가 담깁니다.**

HTTP 헤더(Header) 앞에는 시작 줄(Start-line)이라는 정보가 추가되기 때문에 HTTP 헤더와 시작 줄을 묶어 HTTP 헤드(Head)라고 합니다(HTTP 헤더를 HTTP 헤더 필드라고 하고, HTTP 헤더 필드와 시작줄을 묶어 HTTP 헤드라고 하기도 합니다).

모든 HTTP 메시지는 시작 줄로 시작합니다. HTTP 요청 메시지의 시작줄(요청줄)에는 서버가 실행해야 할 요청이, HTTP 응답 메시지의 시작줄(응답줄)에는 서버가 요청을 실행한 결과가 한 줄로 표시됩니다.

서버가 클라이언트에게 전송할 데이터가 담겨 있는 부분이 HTTP 바디(실제 애플리케이션이 주고받는 데이터)입니다.

따라서 HTTP 메시지는 HTTP 헤드와 HTTP 본문으로 구성됩니다. HTTP 헤드와 HTTP 본문은 빈 줄(Blank line)인 공백 라인(Empty Line)으로 구분합니다. 공백 라인으로 요청과 응답에 관한 모든 정보가 전송되었음을 알려주는 것입니다.

<br/><p align = "center">
<img src = "https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FDyvTT%2FbtqT3tNSlNH%2FPuEbPd8LHfnqnictse3V51%2Fimg.png">
</p>
<p align = "center">
<sup><b><그림 1> HTTP 메시지의 구성</b></sup>
</p><br/>








<br/><br/>

### HTTP 요청 메시지 시작줄 - 요청줄

웹 브라우저가 웹 서버에게 하는 요청은 URL과 메서드(Method)를 사용해서 전달

- URL : 웹 서버에 리소스가 저장된 위치
- HTTP 요청 매서드 : 웹 브라우저가 웹 서버가 수행해주길 바라는 동작
  - HTTP 동사라고 부르기도 함

HTTP가 정의한 대표적인 메서드는 아래 <그림 2>과 같습니다.

<br/><p align = "center">
<img src = "https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbJo6kQ%2FbtqT166rPvF%2FqOQUb0zMd9iM476kTqNNo1%2Fimg.png">
</p>
<p align = "center">
<sup><b><그림 2> HTTP 메서드</b></sup>
</p><br/>

텍스트 기반 문서를 전송하려는 목적으로 개발된 최초의 HTTP는 GET 메서드 하나만 존재했고, URL로 지정하는 리소스도 HTML 문서 밖에 없는 단순한 모델이었습니다(HTTP 버전 0.9). 따라서 초기 HTTP의 요청 메시지는 다음 <그림 3>과 같이 GET 메서드와 URL만으로 구성된 시작 줄이 전부였습니다. URL로 지정한 웹 페이지(/index.html)를 보내달라(GET)는 의미의 요청줄(Request Line)은 메서드와 URL을 공백으로 구분하여 'GET /section/page.html'로 표시하였습니다.

<br/><p align = "center">
<img src = "https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbieovE%2FbtqT3tmN1SC%2F61iXRgObFhSSQTwmQPClJ1%2Fimg.png">
</p>
<p align = "center">
<sup><b><그림 3> 초기 HTTP 요청 메시지</b></sup>
</p><br/>

현재는 메서드와 URL에 클라이언트가 사용하는 HTTP 버전 정보를 추가한 요청줄을 사용합니다. HTTP 버전에 따라 HTTP 메시지의 남은 구조(헤더와 본문)가 결정되기 때문에 HTTP 버전을 알려주는 것입니다.

 



<br/>

#### HTTP 버전이란? 
지나칠 정도로 단순했던 초기 HTTP는 사용하기 쉬워 빠른 속도로 보급되었습니다. HTTP를 사용하는 웹 서버가 증가하면서 텍스트뿐만 아니라 이미지와 동영상 등 다양한 종류의 콘텐츠를 담은 웹 페이지를 보낼 수 있는 기능을 개발하기 시작했습니다. 대다수의 웹 서버는 최초로 개발된 HTTP의 사양을 넘어 새로운 기능을 추가하고 HTTP를 확장하였습니다.

많은 웹 서버가 초기 사양을 넘어 확장된 HTTP를 구현하자 확장된 HTTP를 표준화하려는 시도가 나타났습니다. 그 결과 HTTP의 일반적인 사용법이 문서화되어 1996년 5월, RFC 1945라는 이름으로 공개되었습니다. 이때 공개된 HTTP 사양을 HTTP/1.0*(HTTP 버전 1.0)이라고 합니다. 즉, HTTP/1.0은 새로운 기능을 정의하기보다는 많은 웹 서버에서 이미 사용하고 있던 기능을 문서화하기 위한 작업에서 탄생한 것입니다.

> HTTP의 버전 번호는 HTTP/x.y 형식으로 표현

HTTP/1.0에서 HEAD와 POST라는 메서드가 추가되고, 요청과 응답 메시지에 헤더를 사용하기 시작했으며, 요청줄에 HTTP 버전 번호를 추가했습니다. 또한, 응답의 결과를 표시하는 3자리 상태 코드(아래서 설명합니다)가 도입되었습니다.

HTTP/1.0이 발표된 이후, 1990년에 공개되었던 최초의 HTTP 사양을 HTTP/1.0과 구분하기 위해 HTTP/0.9라고 칭하기 시작했습니다. HTTP/0.9 메시지에는 헤더가 없었습니다. HTTP 요청 메시지는 GET 메서드와 URL만으로 구성된 단순한 형태였고, HTTP 응답은 오로지 HTML 파일만 반환했습니다. 이렇게 HTTP/0.9는 웹 브라우저가 문서를 요청하면, 서버는 문서를 반환한다라는 웹의 기본 뼈대만을 완성해 놓은 매우 단순한 모델이었습니다.

텍스트 기반 문서를 전송하기 위해 설계된 것이 HTTP/0.9라면 이미지, 오디오, 동영상 등 다양한 콘텐츠 형식을 전달하기 위해 확장된 것이 HTTP/1.0입니다. 또한, HTTP/1.0은 웹 브라우저가 데이터를 요청하고 받는 것을 넘어서 데이터를 전송하거나 갱신 또는 삭제할 수 있는 기능도 갖추었습니다(POST 매서드). 이로써 텍스트 너머로 완전히 확장된 HTTP/1.0은 웹 서버가 다양한 콘텐츠를 제공하고 사용자와 대화가 가능한 동적인 서비스를 제공할 수 있는 기반이 되었습니다. HTTP가 출시된 지 불과 5년 만에 인터넷에 존재하는 대부분의 서비스를 구현할 수 있는 범용 프로토콜이 된 것입니다.

1996년 5월에 HTTP/1.0이 발표된 후 HTTP/1.1이 1997년 1월에 공개되었습니다. 버전 번호가 암시하는 것처럼 HTTP/1.1은 큰 변화가 있었다기보다 HTTP/1.0을 최적화해서 사용할 수 있도록 몇 가지 사항을 개선하고 추가하였습니다.

HTTP/1.1은 웹의 공식 표준이 되어 현재 가장 많이 사용되는 버전입니다. HTTP/1.1 사양은 1996년 6월에, 2014년 6월에 업데이트되었습니다. 업데이트된 사양이 나오면 이전 사양을 대체하고 구 사양은 사용되지 않습니다. 현재 표준적으로 사용되고 있는 HTTP/1.1은 2014년 6월에 업데이트된 사양입니다. HTTP/1.1이 도입한 가장 큰 변화는 지속 연결(Persistent Connections) 방법을 도입한 것입니다.

<br/><p align = "center">
<img src = "https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbcKvLU%2FbtqTV9iMBf6%2FIgRpLy8ICydrCS9XvKBj5K%2Fimg.png">
</p>
<p align = "center">
<sup><b><그림 4> HTTP 1.1 버전의 요청 메시지</b></sup>
</p><br/>






 

<br/><br/><br/>

### HTTP 응답 메시지 시작줄 - 응답줄(상태줄)

<br/>

#### HTTP 상태 코드
HTTP/1.0에서 도입한 상태 코드(Status Code)는 웹 서버가 HTTP 요청을 처리한 결과를 알려주는 것입니다.

상태 코드는 컴퓨터가 처리하기 쉬운 3 자리 숫자로 표시합니다. 상태 코드는 코드의 의미를 사람이 이해할 수 있는 짧은 메시지로 설명한 상태 텍스트(Status Text)와 함께 사용합니다. 

상태 코드는 다음 <그림 6>과 같이 5개의 그룹으로 분류합니다.

<br/><p align = "center">
<img src = "https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbBwIRz%2FbtqTYQDlt7l%2FRRfhw2QWAEyeQHXdP3drSk%2Fimg.png">
</p>
<p align = "center">
<sup><b><그림 6> 상태 코드의 분류</b></sup>
</p><br/>

요청에 대한 처리가 계속되고 있다는 정보를 제공하는 100번대 코드는 특수한 용도 외에는 거의 사용되지 않습니다. 200번대 코드는 클라이언트가 요청한 작업을 성공적으로 처리했음을 의미합니다. 300번대 코드는 클라이언트가 요청한 리소스의 위치, 즉 URL이 변경되었을 경우 다른 URL로 다시 요청하도록 알려줍니다. 400번대 코드는 클라이언트가 보낸 요청에 오류가 있음을 의미합니다. 500번대 코드는 서버 내부에서 오류가 발생한 경우 사용합니다.

현재 버전의 HTTP는 각 상태 코드 분류에 대해 적은 수의 코드만을 정의하고 있습니다. 코드 분류만 지키면 HTTP가 정의한 상태 코드를 변경하거나 독자적인 상태 코드를 만들어도 상관없습니다. 예를 들어, 서버 오류에 관한 새로운 상태 코드를 만든다면 500번대를 사용해 이해하기 쉬운 간단한 설명을 상태 텍스트와 함께 표현하면 됩니다.

가장 흔하게 사용되는 상태 코드는 다음 <그림 7>과 같습니다.

<br/><p align = "center">
<img src = "https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbSwLe4%2FbtqT015rvEj%2F1lbubCn6oLkiP4Qy2x48V0%2Fimg.png">
</p>
<p align = "center">
<sup><b><그림 7> 많이 사용되는 상태코드</b></sup>
</p><br/>




<br/><br/>

#### HTTP 응답줄
HTTP 메시지 첫 줄에서 HTTP 요청을 실행한 결과를 응답하는 응답줄(Response Line)은 HTTP 버전과 상태 코드로 구성되어 상태줄(Status Line)이라고도 합니다. 아래 <그림 8>의 응답줄 'HTTP/1.1 200 OK'는 HTTP 버전이 1.1이고, 요청이 성공적으로 수행되었음을 의미합니다.

<br/><p align = "center">
<img src = "https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcpTek3%2FbtqT17j0VAS%2FWdgvUFFv5RkbhZPlpMKoL1%2Fimg.png">
</p>
<p align = "center">
<sup><b><그림 8> HTTP 응답 메시지와 응답줄</b></sup>
</p><br/>





<br/><br/>

#### HTTP 헤더(헤더 필드)
HTTP 시작줄 다음에 오는 헤더는 요청과 응답에 필요한 부가 정보를 선택해서 넣기 때문에 0개, 1개 혹은 여러 개의 헤더가 올 수 있습니다.

기본적으로 헤더는 '헤더의 이름'을 대소문자의 구분 없는 문자열로 표시하고, 다음에 ':(콜론)'을 붙인 후 '헤더 값'을 입력하여 한 줄(Header Line)로 표시합니다. 헤더의 형태와 흔히 쓰이는 헤더는 다음 <그림 9>와 같습니다.

<br/><p align = "center">
<img src = "https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fbrl6fP%2FbtqT17xyTA2%2FzyskiPzrC8G9khFEvLteKK%2Fimg.png">
</p>
<p align = "center">
<sup><b><그림 9> 헤더의 형태와 흔히 쓰이는 헤더의 예</b></sup>
</p><br/>




헤더는 크게 5가지 요청 헤더(Request Header), 응답 헤더(Response Header), 일반 헤더(General Header), 엔터티 헤더(Entity Header), 확장 헤더(Extention)로 분류됩니다.


##### 요청 헤더
요청 메시지를 위한 헤더로 요청에 필요한 부가 정보, 예를 들면 클라이언트가 요청에 따라 받을 수 있는 데이터 타입이나, 클라이언트 애플리케이션(웹 브라우저)에 관한 정보 등을 담습니다.

##### 응답 헤더
응답 메시지를 위한 헤더로 클라이언트에게 보낼 데이터의 타입이나, 서버 애플리케이션에 관한 정보 등 응답에 필요한 부가 정보를 담습니다.


<br/><br/>

<u>**나머지 3개의 헤더는 요청 메시지와 응답 메시지 모두가 사용합니다.**</u>

##### 일반 헤더
메시지에 관한 기본적이고 유용한 정보를 제공하기 위한 일반 목적으로 사용하는 헤더입니다. 일반 헤더에 담기는 부가 정보는 HTTP 메시지를 생성한 일시나, 클라인트와 서버 간의 연결에 관한 옵션 등입니다.

##### 엔터티 헤더
HTTP 본문에 포함된 데이터의 타입, 데이터의 길이 등 본문이 담고 있는 데이터(또는 콘텐츠)에 관한 부가 정보를 담습니다. 당연히 HTTP 메시지 내에 본문이 없으면 엔터티 헤더는 추가되지 않습니다.

##### 확장 헤더
HTTP 표준 사양에는 추가되지 않았지만, 애플리케이션 개발자들에 의해 만들어져 사용되고 있는 비표준 헤더입니다

<br/>

헤더는 단순한 구조로 만들어지고, 애플리케이션이 사용하는 HTTP 버전과 전송하는 데이터의 종류 등을 반영하여 애플리케이션 개발자가 선택하고 추가할 수 있기 때문에 확장성이 매우 좋습니다. 이러한 확장성이 HTTP를 사용하면 어떤 형태의 서비스도 제공 가능한 범용성을 낳고, 지금과 같은 다양한 웹 애플리케이션이 개발되는 바탕이 되었습니다.

 


<br/><br/>

#### HTTP 바디(본문)

HTTP 헤더 밑에 한 줄의 공백을 띠고 추가되는 HTTP 바디에는 요청과 응답에 필요한 데이터가 들어갑니다. HTTP/0.9 바디에는 HTML 문서만 담을 수 있었으나, 이후 버전이 업그레이드되면서 이미지, 비디오, 동영상 등 다양한 형식의 데이터를 담을 수 있게 되었습니다.