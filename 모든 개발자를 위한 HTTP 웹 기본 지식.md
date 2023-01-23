# 모든 개발자를 위한 HTTP 웹 기본 지식

#### 강의 링크

[모든 개발자를 위한 HTTP 웹 기본 지식 - 인프런 | 강의](https://www.inflearn.com/course/http-%EC%9B%B9-%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC/dashboard)

모든 웹 개발자들은 HTTP를 잘 알고 써야 함!

많은 기술들은 HTTP를 당연히 알고 있을거라고 전제로 나타나기 때문에 아는 것이 필요

# 인터넷 네트워크

1:1 근거리 통신이 아닌 이상 우린 인터넷을 활용하여 통신을 하기에 근간이 되는 기본 용어와 기초지식을 가지고 있어야 함

- #### IP(인터넷 프로토콜) : 인터넷 상에 주소
  
  - 지정한 IP에 데이터를 전달 
  
  - 패킷이라는 통신 단위로 데이터 전달 / 출발지, 목적지, 전송하려던 데이터 
    
    * 데이터 요청이랑 응답할 때 가는 경로가 다를 수도 있음

> **<mark>IP프로토콜의 한계 - IP 스스로 해결할 수 없는 문제들</mark>**
> 
> > <mark>비연결성</mark> - 패킷을 받을 대상이 없거나 서비스 불능 상태여도 패킷 전송
> > 
> > <mark>비신뢰성</mark> - 중간에 패킷이 사라지거나 패킷이 순서대로 안 오는 것 대응 어려움
> > 
> >         통상, 1500바이트 이상이면 끊어서 보냄. 받아진 순서가 틀어질 수 있음
> > 
> > <mark>프로그램 구분</mark> - 같은 IP 사용하는 서버 내 통신APP 둘 이상일 시 대응 어려움
> > 
> > ex) 인터넷 게임하면서 음악들으면 받는 건 같은 주소인데 어떻게 구분할 거냐?

- #### TCP,UDP
  
  > ##### 인터넷 프로토콜 스택의 4계층
  > 
  > 애플리케이션 계층 - HTTP, FTP
  > 
  > 전송 계층 - TCP, UDP
  > 
  > 인터넷 계층 - IP
  > 
  > 네트워크 인터페이스 계층  / 이더넷 프레임을 마지막에 씌워서 데이터가 전송됨
  
  <img title="" src="file:///Users/mac/Desktop/code/학습정리/studynote/image/프로토콜계층.PNG" alt="" data-align="center">

<img title="" src="file:///Users/mac/Desktop/code/학습정리/studynote/image/TCP_IP패킷정보.PNG" alt="" width="591" data-align="center">

> ##### TCP(전송제어 프로토콜 Transmission Control Protocol) 특징
> 
> - 연결지향 - TCP 3 way handshake(가상연결) - SYN, SYN+ACK, ACK(+데이터 전송 가능) 
>   
>   - **사실 진짜 물리적으로 연결된 건 아니고 개념적으로 연결된 것**
>   
>   - 서버 클라이언트간에 논리적으로 연결이 되었다고 판단하되 경로간에 중개되는 서버, 노드들이 연결된 건지는 모름.
> 
> - 데이터 전달 보증 - 클라이언트가 데이터 전송시 서버에서 데이터를 잘 받았다고 알려줌
> 
> - 순서 보장 - 순서1,2,3 순서로 전송했는데 1,3,2순으로 도착했다면 서버는 패킷2부터 다시보내라고 요청함
>   
>   - 내부에서 최적화를 통해 해결할 수도 있음
> 
>   
> 
> - 신뢰할 수 있는 프로토콜
> 
> - 대부분 APP에서 TCP 사용

> ##### UDP(사용자 데이터그램 프로토콜 User Datagram Protocol) 특징
> 
> - 하얀 도화지로 비유됨. 사실 기능 거의 없어서
> 
> - 연결지향 3way Handshake 없음 / 그래서 다음 헤더도 적기도 하고.. 빠르긴 함.
> 
> - 데이터 전달 보증 못함 
> 
> - 순서 보장 못함
> 
> - 데이터 보장 못해도 단순하고 빠름
> 
> - 정리해보면, <mark>IP와 거의 같고 PORT, 체크섬 정도 추가됨. APP에서 추가 작업 필요</mark>
> 
> - 하나의 IP라도 여러 포트를 활용해서 프로그램이 구분해서 데이터를 받을 수 잇음
>   
>   - 최근에 각광받고 있음. TCP 프로토콜이 영상을 보낼때도 쓰고 있는 판국이지만 웹 브라우저에서 HTTP 통신을 할 때 HTTP3에서 3way Hand Shake를 줄여보자면서 관심을 받는 중

- #### PORT

한번에 둘 이상 연결해야 한다면? 패킷에 있는 출발지 PORT, 목적지 PORT를 활용함

> 0~65535 : 할당가능
> 
> 0~1023: 잘 알려진 포트, 사용하지 않는 것이 좋음
> 
> FTP - 20,21
> 
> TELNET - 23
> 
> HTTP - 80
> 
> HTTPS - 443

- #### DNS (도메인 네임  시스템 / Domain Naming System)

IP를 기억하기란 쉽지 않다. 또 IP는 변경될 수도 있음

DNS 서버에서 List를 가지고 있어서 IP 유지 관리가 쉬워짐

# URI와 웹 브라우저 요청 흐름

#### URI(Uniform Resource Identifier)

URI는 로케이터, 이름 또는 둘 다 추가로 분류될 수 있다.

-> ieff.org에서 정의하여줌

![URISet](/Users/mac/Desktop/code/학습정리/studynote/URI에%20대한%20집합관계.png)

URI는 리소스를 식별한다는 의미를 갖고 있습니다. 주민번호로 식별하듯 자원이 어디있는지 자체를 식별하는 방법이 크게 2가지가 있습니다. 

Resource Locator / Resource Name입니다.

<img title="" src="file:///Users/mac/Desktop/code/학습정리/studynote/image/URI_URL차이.png" alt="URI/URL" width="675">

URL은 우리가 주소창에서 보는 것이라서 이해하기 쉽습니다.

URN은 이름을 부여하는 것 

이름을 부여하면 거의 찾을 수가 없습니다. 

그래서 거의 URN만 씁니다..

##### URI (Uniform Resource Identifier)

- Uniform : 리소스 식별하는 통일된 방식

- Resource : 자원, URI로 식별할 수 있는 모든 것

- Identifier : 다른 항목과 구분하는데 필요한 정보

##### URL, URN

- URL - locater : 리소스가 있는 위치를 지정

- URN - Name : 리소스에 이름을 부여

- 위치는 변할 수 있지만, 이름은 변하지 않는다.

- urn:isbn:8960777331 ( 어떤 책의 isbn URN)

- URN 이름만으로 실제 리소스를 찾을 수 있는 방법이 보편화되지 않았음 그래서 종종 URI는 URL랑 같은 의미로보고 사용하기도 함

### URL

- scheme://[userinfo@]host[:port][/path][?query][#fragment]

- https://www.google.com:443/search?q=helllo&hl=ko

프로토콜(HTTPS) 호스트명(www.google.com) 포트번호(443) 패스(/search) 쿼리 파라미터(q=hello&hl=ko)

프로토콜 : 어떤 방식으로 자원에 접근할 것인가 하는 약속 규칙

> HTTP:80 / HTTPS:443 포트를 사용, 포트는 생략가능

[userinfo@] : URL에 사용자 정보를 포함해서 인증해야할 때 사용하지만 거의 사용하지 않음

HOST : 도메인명 또는 IP주소

PATH : 리소스 경로, 계층적 구조

> /home/file1.jpg
> 
> /members
> 
> /members/100, /items/galaxy10

QUERY : key=value 형태

> ?로 시작, &로 추가 가능 
> 
> ?keyA=valueA&keyB=valueB
> 
> query Parameter, query string 등으로 불림, 
> 
> > 숫자로 적어도 다 문자로 넘어가서 string이라고 불림.
> 
> 웹 서버에 제공하는 파라미터 문자 형태

fragment: HTML 내부 북마크 등에 사용, 서버에 전송되는 정보 X

#### 웹 브라우저 요청 흐름

1. DNS를 통해서 주소를 알게되고서 포트번호도 파악을 하고나서 HTTP요청메시지를 생성

2. SOCKET 라이브러리를 통해 전달 
   
   A: TCP/IP 연결 
   
   B: 데이터 전달

3. TCP/IP 패킷 생성, HTTP메시지 포함

![웹브라우저요청흐름](/Users/mac/Desktop/code/학습정리/studynote/image/웹브라우저%20요청흐름.png)

서버에서는 패킷을 분해해서 HTTP 메세지를 읽게되고

그에 대한 응답을 HTTP응답 메시지로 만들어서 전달하게 됩니다.

HTTP version, HTTP status code, content type에 맞혀서 내부 데이터를 기록합니다.

클라이언트는 받은 응답메세지를 읽어서 브라우저에서 렌더링을 합니다.

## HTTP

##### HTTP(HyperText Transfer Protocol)

HTML을 전송하는 프로토콜로 시작되어서 지금은 HTTP 메시지에 모든 것을 전송함. 

- HTML, TEXT

- IMAGE, 음성, 영상, 파일

- JSON, XML(API)

- 거의 모든 형태의 데이터 전송 가능

- 서버간에 데이터를 주고 받을 때도 대부분 HTTP 사용

TCP를 직접 연결하는 경우는 게임 개발하는 분야나 특별한 경우에만 존재함..

> ### HTTP 역사
> 
> HTTP/0.9 (1991) : GET 메소드만 지원, HTTP 헤더X
> 
> HTTP/1.0 (1996) : 메소드, 헤더 추가
> 
> **HTTP/1.1 (1997) : 가장 많이 사용, 우리에게 가장 중요한 버전**
> 
> > RFC2069(1997) -> RFC2616(1999) -> RFC7230-7235(2014)
> 
> HTTP/2 (2015) : 성능 개선
> 
> HTTP/3 진행중 : TCP 대신에 UDP 사용, 성능 개선

**대부분의 기능은 1.1에서 만들어졌습니다.**

#### 기반 프로토콜

TCP : HTTP/1.1, HTTP/2

UDP : HTTP/3

현재 HTTP/1.1 주로 사용

HTTP/2, HTTP/3 도 점점 증가

### HTTP 특징

- 클라이언트 서버 구조

- 무상태 프로토콜(스테이트리스), 비연결성

- HTTP 메시지

- 단순함, 확장 가능

### 클라이언트 서버 구조

- Request Response 구조

- 클라이언트는 서버에 요청을 보내고, 응답을 대기

- 서버가 요청에 대한 결과를 만들어서 응답

이전에는 클라이언트와 서버가 분리되지 않아서 하나로 합쳐져있는 개념으로 존재했었습니다.

클라이언트는 UI와 사용성에만 집중하고, 비즈니스 로직, 데이터들을 다 서버에 밀어넣습니다

=> 각각 독립적으로 진화를 할 수 있게 됩니다.

#### 무상태 프로토콜 (Stateless)

고객이 필요한 데이터를 그때 그때 마다 다 넘겨버리는 형식

상태유지 - Stateful, Stateless 차이

상태 유지 -Stateful

- 서버가 클라이언트 상태를 보존함

상태유지 : 고객의 요청에 대해서 중간에 다른 점원으로 바뀌면 안 된다

(중간에 다른 점원으로 바뀔 때 상태 정보를 다른 점원에게 미리 알려줘야한다) 

서버가 클라이언트의 요청에 대응하도록 **유지되어야 한다.** 

<mark>장애가 나면 클라이언트는 처음부터 다시 해야한다.</mark>

무상태 : 중간에 다른 점원으로 바뀌어도 된다.

- 고객이 증가해도 점원을 대거 투입할 수 있다.

- 클라이언트 요청이 증가해도 서버를 대거 투입할 수 있다.

아무 서버나 호출해도 됩니다.

무상태는 응답 서버를 쉽게 바꿀 수 있다. -> **무한한 서버 증설 가능(스케일아웃)**

#### Stateless 실무의 한계

모든 것을 무상태로 설계할 수 있는 경우도 있고 없는 경우도 있다.

무상태 ex) 로그인 필요없는 서비스 소개 화면

상태유지 ex) 로그인

> 로그인 사용자 경우 로그인 했다는 상태를 서버에 유지
> 
> 일반적으로 브라우가 쿠키, 서버 세션 등으로 상태 유지
> 
> 상태 유지는 최소한만 사용

상태 보존이 안되다보니 데이터를 너무 많이 보낸다는 것도 있습니다.

### 비연결성

연결을 유지하는 모델은 여러 클라이언트가 서버에 연결된다면 서버는 요청이 없는 클라이언트와의 연결을 계속 유지해야하며 이는 서버 자원 소모로 이어집니다.

 연결을 유지하지 않는 모델은 요청응답 끝나면 연결을 끊어버리기 때문에 서버 자원을 절약할 수 있게되어 최소한의 자원만 유지하게 됩니다.

- HTTP는 기본이 연결을 유지하지 않는 모델

- 일반적으로 초 단위의 이하의 빠른 속도로 응답

- 1시간 동안 수천명이 서비스를 사용해도 실제 서버에서 동시에 처리하는 요청은 수십개 이하로 매우 작음
  
  -  웹 브라우저에서 계속 연속해서 검색 버튼을 누르지는 않는다.

- 서버 자원을 매우 효율적으로 사용할 수 있음

##### 비 연결성의 한계와 극복

- TCP/IP연결을 새로 맺어야 함 - 3 way Handshake 시간 추가
  
  웹 브라우저로 사이트를 요청하면 HTML 뿐 아니라, JS,CSS 추가 이미지 등 수 많은 자원이 함께 다운로드

- 지금은 HTTP 지속연결(Persistent Connections)로 문제 해결 

- HTTP/2, HTTP/3에서 더 많은 최적화

**대용량 트래픽 대응을 위해서 최대한 설계할때 스테이스리스로 만들려고 해야합니다.**

#### HTTP 메시지

요청메시지, 응답메시지 형식이 조금 다르긴 합니다.

HTTP요청 메시지의 구조는

> start-line 시작라인
> 
> header
> 
> empty line 공백라인 (CRLF == enter 같은 것)
> 
> message body

ex) HTTP 요청 메시지 : 요청 메시지도 body 본문을 가질 수 있음

> GET /search?q=hello&hl=ko HTTP/1.1 // start-line
> 
> Host: www.google.com // header
> 
> // [empty line] 

ex) HTTP 응답 메시지

> HTTP/1.1 200 OK   // start-line
> 
> Content-Type: text/html;charset=UTF-8 // header
> 
> Content-Length:3423 // header
> 
> // [empty line]
> 
> <html>
> <body><h2>hello World</h2></body>
> </html>
> 
> // above test is message body

##### 시작라인 : 요청 메시지

- start-line = request-line / status-line

- request-line = method SP(공백) request-target SP HTTP-version CRLF(엔터)

- HTTP 메소드 (GET 조회)
  
  - 종류 : GET, POST, PUT, DELETE
  
  - 서버가 수행해야 할 동작 지정
    
    - GET : 리소스 조회
    
    - POST : 요청 내역 처리

- 요청 대상(/search?q=hello&hl=ko)
  
  - absolute-path[?query] (절대경로[?쿼리])
  
  - 절대경로 = "/"로 시작하는 경로
  
  - 참고 : http://...?x=y처럼 다른 유형의 경로 지정 방법도 존재

- HTTP version

##### 응답 메시지

- start-line = request-line / status-line

- status-line = HTTP-version SP status-code SP reason-phrase CRLF

- HTTP 버전

- HTTP 상태 코드 : 요청 성공, 실패를 나타냅니다.
  
  - 200 : 성공
  
  - 400 : 클라이언트 요청 오류
  
  - 500 : 서버 내부 오류

- 이유 문구 : 사람이 이해할 수 있는 짧은 상태 코드 설명 글  

##### HTTP 헤더

- header-field = field-name":"OWS field-value OWS (OWS : 띄어쓰기 허용)

- field-name 은 대소문자 구문 없음 GET == get, Host == host 
  
  >  GET /search?q=hello&hl=ko HTTP/1.1
  > 
  > Host: [www.google.com](http://www.google.com) // 여기서 **<mark>: 이랑 Host가 붙어있어야 함!!</mark>**

>     HTTP/1.1 200 OK
> 
> Content-Type: text/html;charset=UTF-8
> 
> <html>

##### HTTP 헤더의 용도 - 바디 빼고 필요한 메타 정보를 다 들고 있다고 봐도 될 듯

HTTP 전송에 필요한 모든 부가정보

예) 메시지 바디의 내용, 메시지 바디의 크기, 압축, 인증, 요청 클라이언트(브라우저)정보, 서버 앱 정보, 캐시 관리 정보 등..

표준 헤더가 너무 많음

필요시 임의의 헤더 추가 가능

    - helloworld: hihi

##### HTTP 메시지 바디

실제 전송할  데이터

HTML, Doc, image, Mov, json 등등 byte로 표현할 수 있는 모든 데이터 전송 가능

#### 단순함 확장 가능

- HTTP는 단순하다, 스펙도 읽어볼만...

- HTTP 메시지는 매우 단순

- **크게 성공하는 표준 기술은 단순하지만 확장 가능한 기술**

****

### HTTP 메서드

ex) 이것이 과연 좋은 URI설계인가?

> 회원목록조회 /read-member-list
> 
> 회원조회 /read-member-by-id
> 
> 회원등록 /create-member
> 
> 회원수정 /update-member
> 
> 회원삭제 /delete-member
> 
> # 가장 중요한 건 리소스 식별

### API URI 고민

URI(Uniform Resource Identifier)

리소스의 의미는 뭘까?

- 회원을 등록하고 수정하고 조회하는게 리소스가 아니다
  
  - ex) 미네랄을 캐라 -> 미네랄이 리소스
  
  - **즉, 회원이라는 개념 자체가 바로 리소스다.**

- 리소스를 어떻게 식별하는게 좋을까?
  
  - 회원을 등록하고 수정하고 조회하는 것을 모두 배제
  
  - **회원이라는 리소스만 식별하면 된다.** -> **회원 리소스를 URI에 매핑**

ex) 이것이 과연 좋은 URI설계인가?

> **회원**목록조회 /mebers
> 
> **회원**조회 /members/{id}
> 
> **회원**등록 /mebers/{id}
> 
> **회원**수정 /members/{id}
> 
> **회원**삭제 /members/{id}
> 
> ### 그러면 조회부터 삭제는 어떻게 구분을 할 수 있을까?

### 리소스와 행위을 분리

가장 중요한 것은 리소스를 식별하는 것

- URI는 리소스만 식별!

- 리소스와 해당, 리소스를 대상으로 하는 행위을 분리
  
  - 리소스 : 회원
  
  - 행위 : 조회, 등록, 삭제, 변경

- 리소스는 명사, 행위는 동사 (미네랄을 캐라)

- 행위(메서드)는 어떻게 구분? HTTP 메서드를 활용해서!

### HTTP 메서드 - GET, POST

HTTP 메서드는 클라이언트가 서버에게 요청을 할 때 기대하는 행동입니다.

GET은 달라는 것, POST는 데이터를 줄테니 처리를 해달라는 요청

주요 메서드

- GET : 리소스 조회

- POST : 요청 데이터 처리, 주로 등록에 사용

- PUT : 리소스를 대체, 해당 리소스가 없으면 생성

- PATCH : 리소스 부분 변경

- DELETE : 리소스 삭제

기타 메서드 

- HEAD : GET과 동일하지만 메시지 부분을 제외하고, 상태 줄과 헤더만 반환

- OPTION : 대상 리소스에 대한 통신 가능 옵션(메서드)을 설명(주로 CORS에서 사용)

- CONNECT : 대상 자원으로 식별되는 서버에 대한 터널을 설정

- TRACE : 대상 리소스에 대한 경로를 따라 메시지 루프백 테스트를 수행

##### GET

> GET /search?q=hello&hl=ko HTTP/1.1 
> 
> Host:www.google.com

리소스 조회

서버에 전달하고 싶은 데이터는 query(쿼리 파라미터, 쿼리 스트링)를 통해서 전달

**메시지 바디를 사용해서 데이터를 전달할 수 있지만, 지원하지 않는 서버가 많아서 권장하지 않음**

![](/Users/mac/Desktop/code/학습정리/studynote/image/http_GET메서드.png)

![](/Users/mac/Desktop/code/학습정리/studynote/image/http_GET메서드2.png)

##### POST

> POST /members HTTP/1.1
> 
> Content-Type: application/json
> 
> {
> 
> "username":"hello",
> 
> "age":20
> 
> }

요청 데이터 처리

메시지 바디를 통해 서버로 요청 데이터 전달

서버는 요청 데이터를 처리

    **<mark>메시지 바디를 통해 들어온 데이터를 처리하는 모든 기능을 수행한다</mark>**

<mark>주로 전달된 데이터로 신규 리소스 등록, 프로세스 처리에 사용</mark>

리소스 등록 - 메시지 전달

![](/Users/mac/Desktop/code/학습정리/studynote/image/http_POST메서드.png)

리소스 등록2 - 신규 리소스 식별자를 생성

![](/Users/mac/Desktop/code/학습정리/studynote/image/http_POST메서드2.png)

리소스 등록3 - 응답 데이터 

201로 created되면 자원의 신규생성된 location을 같이 보내줍니다 

![](/Users/mac/Desktop/code/학습정리/studynote/image/http_POST메서드3.png)

#### POST

요청 데이터를 어떻게 처리한다는 뜻일까? 예시

스펙: POST 메서드는 대상 리소스가 리소스의 고유한 의미 체계에 따라 요청에 포함된 표현을 처리하도록 요청합니다.(구글 번역)

예를 들어 POST는 다음과 같은 기능에 사용됩니다.

    - HTML 양식에 입력 된 필드와 같은 데이터의 블록을 데이터 처리 프로세스에 제공

    ex) HTML FORM에 입력한 정보를 회원 가입, 주문 등에서 사용

    - 게시판, 뉴스 그룹, 메일링 리스트, 블로그 또는 유사한 기사 그룹에 메시지 게시

    ex) 게시판 글쓰기, 댓글 달기

    - 서버가 아직 식별하지 않은 새 리소스 생성

    ex) 신규 주문 생성

    - 기존 자원에 데이터 추가

    ex) 한 문서 끝에 내용 추가하기

#### 정리 : 이 리소스 URI에 POST 요청이 오면 요청 데이터를 어떻게 처리할지 리소스마다 따로 정해야 함 -> 정해진 것이 없다.

POST 정리

1. **새 리소스 생성(등록)**
   
   서버가 아직 식별하지 않은 새 리소스 생성

    2. **요청 데이터 처리**

        단순히 데이터를 생성하거나, 변경하는 것을 넘어서 프로세스를 처리해야 하는 경우

        ex) 주문에서 결제완료 -> 배달시작 -> 배달완료 처럼 단순히 값 변경을 넘어 프로세스의 상태가 변경되는 경우

        POST의 결과로 새로운 리소스가 생성되지 않을 수도 있음

        ex) POST /orders/{orderid}/start-delivery (컨트롤 URI) 

-> 동사는 다 떼어내라 했었는데??? 그렇지 못한 경우가 있습니다. 리소스로만 표현하는 것은 굉장히 이상적입니다. 위 start-delivert 같은 것 어쩔 수 없는 부분을 컨트롤 URI로 설계해야 합니다.

3. **다른 메서드로 처리하기 애매한 경우**
   
   JSON으로 조회용 데이터를 넘겨야 하는데, GET메서드를 사용하기 어려운 경우
   
   애매하면 POST

POST는 모든 걸 할 수 있습니다. 메세지에 보내는 모든 걸 할 수 있습니다

조회를 할 때는 GET을 쓰는게 유리한 거는 GET으로 오면 caching을 하겠다거나 하기 쉽지만 POST는 캐싱 같은 것이 어렵습니다 

**보통 서버에서 Resource URI를 만들어서 클라이언트에게 던져줍니다**

### PUT

> PUT /members/100 HTTP/1.1
> 
> Content-Type: application/json
> 
> {
> 
>     "username":"hello",
> 
>     "age":20
> 
> }

**리소스를 대체**

    리소스가 있으면 대체

    리소스가 없으면 생성

    쉽게 이야기해서 덮어버림

##### (중요) 클라이언트가 리소스를 식별

**<mark>위에서는 /mebers/100에다가 저장해달라고 요청을 하는 것</mark>**

클라이언트가 리소스 위치를 알고 URI 지정 **이것이 POST와 차이점**

##### 리소스가 있는 경우

![](/Users/mac/Desktop/code/학습정리/studynote/image/http_PUT메서드.png)

![](/Users/mac/Desktop/code/학습정리/studynote/image/http_PUT메서드2.png)

##### 리소스가 없는 경우 - 신규 리소스가 생성됨

![](/Users/mac/Desktop/code/학습정리/studynote/image/http_PUT메서드3.png)

![](/Users/mac/Desktop/code/학습정리/studynote/image/http_PUT메서드4.png)

##### PUT  주의! 리소스를 완전히 대체한다

![](/Users/mac/Desktop/code/학습정리/studynote/image/http_PUT메서드5.png)

![](/Users/mac/Desktop/code/학습정리/studynote/image/http_PUT메서드6.png)

##### 기존 것을 지워버리고 새로운 데이터로 갱신됨

일부를 수정하는 용도로 사용하기 위해서 존재하는 것이 PATCH

#### PATCH - 리소스 부분 변경

> PATCH /members/100 HTTP/1.1
> 
> Content-Type: application/json
> 
> {
> 
>     "age":50
> 
> }

PATCH가 안 먹힐때는 POST를 쓰시면 됩니다.![](/Users/mac/Desktop/code/학습정리/studynote/image/http_PATCH메서드.png)

![](/Users/mac/Desktop/code/학습정리/studynote/image/http_PATCH메서드2.png)

#### DELETE - 리소스 제거

> DELETE /members/100 HTTP/1.1
> 
> Host: localhost:8080

![](/Users/mac/Desktop/code/학습정리/studynote/image/http_DELETE메서드.png)

#### 데이터가 완전히 삭제됨

#### HTTP 메서드의 속성

- 안전(Safe Methods)

- **멱등(Idempotent Methods)**

- 캐시가능(Cacheable Methods)

![](/Users/mac/Desktop/code/학습정리/studynote/image/http메서드표.png)

### 안전

- 호출해도 리소스를 변경하지 않는다

- Q: 그래도 계속 호출해서, 로그 같은게 쌓여서 장애가 발생하면요?

- A: 안전은 해당 리소스만 고려한다. 그런 부분까지 고려하지 않는다.
  
  여러번이든 한번이든 호출했을 때 변경이 일어나지 않는 걸 safe하다라고 말합니다.
  
  GET, HEAD는 안전합니다

### 멱등

f(f(x)) =f(x) 

한번 호출하든 두번 호출하든 백번 호출하든 결과가 똑같아야 한다.

멱등 메서드

- GET : 한 번 조회하든, 두 번 조회하든 같은 결과가 조회된다

- PUT : 결과를 대체한다. 따라서 같은 요청을 여러번 해도 최종 결과는 같다

- DELETE : 결과를 삭제한다. 같은 요청을 여러번 해도 삭제된 결과는 똑같다

- <mark>POST : 멱등이 아니다! 두 번 호출하면 같은 결제가 중복해서 발생할 수 있다</mark>

**PUT도 멱등이 됩니다 똑같은 파일을 계속 put으로 보내면 계속 기존 거를 날리고 새로 덮기 때문에 최종적으로 결과물은 동일하기 때문에 멱등을 인정합니다**

**DELETE도 한번 요청하건 여러번 요청하건 삭제된 결과는 동일하므로 멱등합니다**

POST는 멱등하지 않습니다. 결제라고 생각하면 중복결제가 됩니다.

멱등이 왜 필요한가요?

자동복구 메커니즘을 생각해봅시다

클라이언트에서 delete 요청을 보냈는데 서버에서 응답이 없어서 클라이언트가 delete를 재시도하는거에요. 멱등하니까요 두번 해도 괜찮기에 자동복구 메커니즘을 쓸 수 있습니다.

Q: 재요청 중간에 다른 곳에서 리소스를 변경해버리면?

- 사용자1: GET -> username:A, age:20

- 사용자2: PUT -> username:A, age:30

- 사용자1: GET -> username:A, age:30 -> 사용자 2의 영향으로 바뀐 데이터 조회

**<mark>=> A: 멱등은 외부 요인으로 중간에 리소스가 변경되는 것 까지는 고려하지 않는다.</mark>**

### 캐시가능

**응답 결과 리소스를 캐시해서 사용해도 되는가**?

**GET, HEAD, POST, PATCH 캐시가능**

**<mark>실제로는 GET, HEAD 정도만 캐시로 사용</mark>**

    => POST,  PATCH는 본문 내용까지 캐시 키로 고려해야 하는데, 구현이 쉽지 않음

### HTTP 메서드 활용

**클라이언트에서 서버로 데이터 전송**

HTTP API 설계 예시

##### <mark>데이터 전달 방식은 크게 2가지</mark>

- **쿼리 파라미터를 통한 데이터 전송**
  
  - GET
  
  - 주로 정렬 필터(검색어)

- **메시지 바디를 통한 데이터 전송**
  
  - POST, PUT, PATCH
  
  - 회원가입, 상품주문, 리소스 등록, 리소스 변경

### 클라이언트에서 서버로 데이터 전송 (4가지 상황)

- **정적 데이터 조회**
  
  - 이미지, 정적 텍스트 문서

- **동적 데이터 조회**
  
  - 주로 검색, 게시판 목록에서 정렬 필터(검색어)

- **HTML Form을 통한 데이터 전송**
  
  - 회원가입, 상품주문, 데이터 변경

- **HTTP API를 통한 데이터 전송**
  
  - 회원가입, 상품주문, 데이터 변경
  
  - 서버 TO 서버, 앱 클라이언트, 웹 클라이언트

#### 정적 데이터 조회

**쿼리파라미터 미사용**

![](/Users/mac/Desktop/code/학습정리/studynote/image/정적데이터조회.png)

/star를 요청하여 서버에서 star 데이터를 받아오게 됩니다.

**정리**

- 이미지, 정적 텍스트 문서

- 조회는 GET 사용

- 정적 데이터는 일반적으로 쿼리 파라미터 없이 리소스 경로로 단순하게 조회 가능

#### 동적 데이터 조회

**쿼리파라미터 사용**

![](/Users/mac/Desktop/code/학습정리/studynote/image/동적데이터조회.png)

**정리**

- 주로 검색, 게시판 목록에서 정렬 필터(검색어)

- 조회 조건을 줄여주는 필터, 조회 결과를 정렬하는 정렬 조건에 주로 사용

- 조회는 GET 사용

- GET은 쿼리 파라미터 사용해서 데이터를 전달

#### HTML Form 데이터 전송

**POST 전송 - 저장**

![](/Users/mac/Desktop/code/학습정리/studynote/image/htmlForm전송.png)

![](/Users/mac/Desktop/code/학습정리/studynote/image/htmlForm전송을Get으로보내는경우.png)

**method를 get으로 보낼수는 있지만 그렇게되면 바디에 담긴 정보가 쿼리 파라미터로 전달됨**

<mark>Get은 반드시 조회할때 이외에서는 사용하지 마세요!</mark>

**multipart/form-data**

![](/Users/mac/Desktop/code/학습정리/studynote/image/htmlForm전송multipart.png)

바디에 파일 데이터도 같이 넣고 있습니다. 이럴 때 데이터를 웹 브라우저는 Multipart로 쪼개고

위에서는 boundary = ---xxx가 구분해줍니다

여러개 멀티로 컨텐츠 타입을 달리해서 보낼 수 있는 것입니다.

HTML Form 데이터 전송

**정리**

- HTML Form submit시 POST전송
  
  ex) 회원 가입, 상품 주문, 데이터  변경

- Content-Type: application/x-www.form-urlencoded사용
  
  - form의 내용을 메시지 바디를 통해서 전송(key=value, 쿼리 파라미터 형식)
  
  - 전송 데이터를 url encoding 처리
    
    - ex) abc김 -> abc%EA%B9%80

- HTML Form은 GET 전송도 가능

- Content-Type: multipart/form-data
  
  - 파일 업로드 같은 바이너리 데이터 전송시 사용
  
  - 다른 종류의 여러 파일과 폼의 내용 함께 전송 가능(그래서 이름이 multipart)

- <mark>참고 : HTML Form 전송은 GET, POST만 지원</mark>

##### HTTP API 데이터 전송

![](/Users/mac/Desktop/code/학습정리/studynote/image/http%20API%20데이터%20전송.png)

##### 정리

- 서버 to 서버
  
  - 백엔드, 시스템 통신

- 앱 클라이언트
  
  - 아이폰, 안드로이드

- 웹 클라이언트
  
  HTML에서 Form 전송 대신 자바 스크립트 통한 통신에 사용(AJAX)
  
  ex) React, Vue.js 같은 웹 클라이언트와 API 통신

- POST, PUT, PATCH: 메시지 바디를 통해 데이터 전송

- GET: 조회, 쿼리 파라미터로 데이터 전달

- Content-Type : application/json을 주로 사용 (사실상 표준)
  
  - TEST, XML, JSON 등등

#### HTTP API 설계 예시

ex)

> HTTP API - 컬렉션
> 
> - POST 기반 등록
> 
>     ex) 회원 관리 API 제공
> 
> HTTP API - 스토어
> 
> - PUT 기반 등록
> 
>    ex) 정적 컨텐츠 관리, 원격 파일 관리
> 
> HTML FORM 사용
> 
> - 웹 페이지 회원 관리
> 
> - GET,POST만 지원

##### 회원관리 시스템

API 설계 - POST 기반 등록

- 회원 목록/members -> GET

- 회원 등록/members -> POST

- 회원 조회 /members/{id} -> GET

- 회원 수정 /members/{id} -> PATCH, PUT, POST // put은 완전히 다 덮을일이 있을때 쓰면 좋습니다.

- 회원 삭제 /members/{id} -> DELETE

##### 회원 관리 시스템

POST - 신규 자원 등록 특징

- **클라이언트는 등록될 리소스의 URI를 모른다.**
  
  - 회원 등록 / members/ -> POST
  
  - POST/ members

- **서버가 새로 등록된 리소스 URI를 생성해준다.**
  
  - HTTP/1.1 201 created
  
  - **Location: /members/100**

- **<mark>컬렉션</mark>**
  
  - 서버가 관리하는 리소스 디렉토리
  
  - 서버가 리소스의 URI를 생성하고 관리
  
  - 여기서 컬렉션은 /members

ex2)

#### 파일 관리 시스템

API설계 - PUT 기반 등록

- 파일 목록 /files -> GET

- 파일 조회 /files/{filenames} -> GET

- 파일 등록 /files/{filename} -> PUT

- 파일 삭제 /files/{filename} -> DELETE

- 파일 대략 등록 /files -> POST

클라이언트가 filename을 알고 있으므로 PUT을 쓰면 입력한 URI를 리소스로 사용합니다

아까전의 API설계와의 차이는 파일을 등록할 때 PUT을 썼습니다. 이럴 때!!

- 클라이언트가 리소스 URI를 알고 있어야 한다
  
  - 파일 등록 /files/{filename} -> PUT
  
  - PUT /files/star.jpg

- **클라이언트가 직접 리소스의 URI를 지정한다**

- **<mark>스토어(Store)</mark>**
  
  - 클라이언트가 관리하는 리소스 저장소
  
  - 클라이언트가 리소스의 URI를 알고 관리
  
  - 여기서 스토어는 /files

POST기반은 요청 후 서버가 알아서 정해서 알려주는 것이되고 PUT기반은 클라이언트가 직접 리소스를 관리하게 됩니다. 

##### 대부분 POST 기반 신규 자원 등록을 사용합니다

##### HTML FORM사용

- HTML FORM은 GET, POST만 지원

- AJAX 같은 기술을 사용해서 해결 가능 -> 회원 API참고

- 여기서는 순수 HTML, HTML FORM 이야기

- **컨트롤 URI**
  
  - GET, POST만 지원하므로 제약이 있음
  
  - 이런 제약을 해결하기 위해서 **<mark>동사로 된 컨트롤 URI를 활용한 리소스 경로 사용</mark>**
  
  - POST의 /new, /edit, /delete가 컨트롤 URI
  
  - HTTP 메서드로 해결하기 애매한 경우 사용(HTTP API포함)

ex) GET, POST로만 구현하는 상황

- 회원 목록 /members -> GET

- 회원 등록 폼 /members/new -> GET

- 회원 등록 둘 중 한가지 방법을 사용할 수 있음
  
  - /members/new -> POST
  
  - /members -> POST

- 회원 조회 /members/{id} -> GET

- 회원 수정 폼 /members/{id}/edit -> GET

- 회원 수정 /members/{id}/edit, /members/{id} -> POST

- 회원 삭제 /members/{id}/delete -> POST
  
  - 방법이 없으므로 컨트롤 URI를 사용

##### 컨트롤 URI는 무식하게 사용하면 안 됩니다

일단 설계할때는 리소스라는 개념을 가지고 하되 정말 안 될때 대체재로 사용해야 합니다

HTTP 메서드로 안 떨어질 때 사용합니다

#### 참고하면 좋은 URI 설계 개념

- 문서 (document)
  
  - 단일 개념(파일 하나, 객체 인스턴스, 데이터베이스 row)
    
    - ex) /members/100, /files/star.jpg

- 컬렉션(Collection)
  
  - 서버가 관리하는 리소스 디렉터리
  
  - 서버가 리소스의 URI를 생성하고 관리
    
    - ex) /members

- 스토어(Store)
  
  - 클라이언트가 관리하는 자원 저장소
  
  - 클라이언트가 리소스의 URI를 알고 관리

- 컨트롤러(Controller), 컨트롤 URI
  
  - 문서, 컬렉션, 스토어로 해결하기 어려운 추가 프로세스 실행
  
  - 동사를 직접 사용
  
  - ex) /members/{id}/delete

참고 url : https://resfulapi.net/resource-naming

****

# HTTP 상태코드

상태코드 : 클라이언트가 보낸 요청의 처리 상태를 응답에서 알려주는 기능

- 1XX(informational) : 요청이 수신되어 처리중

- 2XX(Successful) : 요청 정상 처리

- 3XX(Redirection) : 요청을 완료하려면 추가 행동이 필요

- 4XX(Client Error) : 클라이언트 오류, 잘못된 문법 등으로 서버가 요청을 수행할 수 없음

- 5XX(Server Error) : 서버 오류, 서버가 정상 요청을 처리하지 못함

만약 모르는 상태코드가 나온다면?

- 클라이언트가 인식할 수 없는 상태코드를 서버가 반환하면?

- 클라이언트는 상위 상태코드(0xx)로 해석해서 처리

- 미래에 새로운 상태 코드가 추가되어도 클라이언트를 변경하지 않아도 됨
  
  ex)
  
  299 ??? > 2XX 
  
  451 ??? > 4XX
  
  599 ??? > 5XX

##### 1XX(Informational) : 요청이 수신되어 처리 중

- 거의 사용 않음

##### 2XX(Successful) : 클라이언트의 요청을 성공적으로 처리

- 200 OK

- 201 Created / 요청 성공해서 새로운 리소스가 생성됨

- 202 Accepted / 요청이 접수되었으나 처리가 완료되지 않았음
  
  - 배치 처리 같은 곳에서 사용
    
    - ex) 요청 접수 후 1시간 뒤에 배치 프로세스 요청 처리

- 204 No Content / 서버가 요청을 성공적으로 수행했지만, 응답 페이로드 본문에 보낼 데이터가 없음
  
  - ex) 웹 문서 편집기에서 save 버튼
    
    - save 버튼의 결과로 아무 내용이 없어도 된다
    
    - save 버튼을 눌러도 같은 화면을 유지해야 한다
    
    - 결과 내용이 없어도 204 메시지(2xx)만으로 성공을 인식할 수 있다

##### 3XX (Redirection) : 요청을 완료하기 위해 유저의 추가 조치 필요

- 300 Multiple Choices / 이건 거의 안 씀

- 301 Moved Permanetely

- 302 Found

- 303 See Other

- 304 Not Modified

- 307 Temporary Redirect

- 308 Permanent Redirect

##### 리다이렉션 이해

웹 브라우저는 3XX 응답의 결과에 Location 헤더가 있으면, Location 위치로 자동 이동(리다이렉트)

##### 리다이렉션 이해

종류

- 영구 리다이렉션 - 특정 리소스의 URI가 영구적으로 이동 
  
  - ex) members -> /users
  
  - ex) event -> /new-event

- 일시 리다이렉션 - 일시적인 변경
  
  - 주문 완료 후 주문 내역 화면으로 이동
  
  - PRG : Post/Redirect/Get

- 특수 리다이렉션 
  
  - 결과 대신 캐시를 사용

##### 영구 리다이렉션 - 301, 308

- 리소스의 URI가 영구적으로 이동

- 원래의 URL를 사용 X, 검색 엔진 등에서도 변경 인지

- **301 Moved Permanently**
  
  - **리다이렉트시 요청 메서드가 GET으로 변하고, 본문이 제거될 수 있음(MAY)**
    
    - 클라이언트에서 POST요청에 대해서 리다이렉션을 받았으나 처음 요청시 보냈던 데이터들은 사라져있는 채 GET 요청을 보냄 / 새로운 이벤트 페이지 화면만 나옴

- **308 Permanently Redirect**
  
  - 301과 기능은 같음
  
  - **리다이렉트 시 요청 메서드와 본문 유지(처음 POST를 보내면 리다이렉트 이후에도 POST로 유지한 것 그대로 보냄)**

!여기서 본문이 제거될수도 있는 경우가 나타나게 된 배경에는 구현 스펙을 정리할 때 자세하게 정리하지 않았어서 많이들 브라우저를 개발하는 부분에서 리다이렉트 되면 브라우저가 알아서 GET으로 바뀌어서 요청을 보내버리게 만들어버려서 이미 그렇게 정착이 되어버리니 스펙을 수정하게 된 것

##### 일시적인 리다이렉션 - 302, 307, 308

- 리소스의 URI가 일시적으로 변경

- 따라서 검색 엔진 등에서 URL을 변경하면 안됨

- **302 Found**
  
  - **리다이렉트시 요청 메서드가 GET으로 변하고, 본문이 제거될 수 있음(MAY)**

- **307 Templorary Redirect**
  
  - 302와 기능은 같음
  
  - 리다이렉트시 요청 메서드와 본문 **유지**
  
  - **<mark>(요청 메서드를 변경하면 안 된다. Must Not)</mark>**

- **303 See Other**
  
  - 302와 기능은 같음
  
  - 리다이렉트시 요청 메서드가 GET으로 변경

**언제 리다이렉트를 시키는 걸까?**

##### PRG: Post / Redirect / Get : 일시적인 리다이렉션

ex)

Post로 주문후에 웹 브라우저를 새로고침하면?

새로고침은 다시 요청, 중복 주문이 될 수 있다

- POST로 주문 후에 새로 고침으로 인한 중복 주문 방지

- POST로 주문 후에 주문 결과 화면을 GET메서드로 리다이렉트

- 새로고침해도 결과 화면을 GET으로 조회

- 중복 주문 대신에 결과 화면만 GET으로 다시 요청

PRG 이후 리다이렉트

- URL이 이미 POST -> GET으로 리다이렉트 됨

- 새로고침해도 GET으로 결과 화면만 조회

그러면 뭘 써야할까?

역사

- 처음 302 스펙의 의도는 HTTP 메서드를 유지하는 것

- 그런데 웹 브라우저들이 대부분 GET으로 바뀜(일부는 다르게 동작)

- 그래서 모호한 302대신에 307,303 등장

현실

- 307,303 권장하지만 이미 많은 앱 라이브러리가 302를 기본값으로 사용

- 자동 리다이렉션 시 GET으로 변해도 되면 302사용해도 문제없음

##### 기타 리다이렉션

- 300 Multiple Choices : 안 씀

- **<mark>304 Not Modified </mark>**
  
  - <mark>**캐시를 목적으로 사용**</mark>
  
  - 클라이언트에게 리소스가 수정되지 않았음을 알려준다. 따라서 클라이언트는 로컬PC에 저장된 캐시를 재사용한다 (캐시로 리다이렉트 한다)
  
  - 304 응답은 응답에 메시지 바디를 포함하면 안 된다. (로컬 캐시를 사용해야 하므로)
  
  - 조건부 GET, HEAD 요청 시 사용

##### 4XX (Client Error) : 클라이언트 오류

- 클라이언트의 요청에 잘못된 문법 등으로 서버가 요청을 수행할 수 없음

- 오류의 원인이 클라이언트에 있음

- **중요! 클라이언트가 이미 잘못된 요청, 데이터를 보내고 있기 때문에 똑같은 재시도도 계속 실패함 / 반면 서버에서 문제인 것이라면 문제를 해결하게 되면 클라이언트에서 요청을 재시도를 했을 때 성공할 가능성이 있음**

- 400 Bad Request : 클라이언트가 잘못된 요청을 해서 서버가 요청을 처리할 수 없음
  
  - 요청 구문, 메시지 등등 오류
  
  - 클라이언트는 요청 내용을 다시 검토하고, 보내야함
  
  - ex) 요청 파라미터가 잘못되거나, API스펙이 맞지 않을 때

- 401 Unauthorized : 클라이언트가 해당 리소스에 대한 인증이 필요함
  
  - 인증(Authentication) 되지 않음
  
  - 401 오류 발생시 응답에 WWW-Authenticate헤더와 함께 인증방법을 설명

- 참고
  
  - 인증(Authentication) : 본인이 누구인지 확인, (로그인)
  
  - 인가(Authorization) : 권한부여 (Admin 권한처럼 특정 리소스에 접근할 수 있는 권한, 인증이 있어야 인가가 있음)
  
  - 오류 메시지가 Unauthorized 이지만 인증 되지 않음

- 403 Forbidden : 서버가 요청을 이해했지만 승인을 거부함
  
  - 주로 인증 자격 증명은 있지만, 접근 권한이 불충분한 경우
    
    - ex) admin등급이 아닌 사용자가 admin 등급 리소스 접근하는 경우

- 404 Not Found : 요청 리소스를 찾을 수 없음
  
  - 요청 리소스가 서버에 없음
  
  - 또는 클라이언트 권한이 부족한 리소스에 접근할 때 해당 리소스를 숨기고 싶을 때, 403에러 보여주기 싫을 때

##### 5XX (Server Error) : 서버오류

- 서버 문제로 오류 발생

- 서버에 문제가 있기 때문에 재시도 하면 성공할 수도 있음(복구되거나 해서..)

- 500 Internal Server Error : 서버 문제로 오류 발생, 애매하면 500 오류

- 503 Service Unavailabe : 서비스 이용 불가
  
  - 서버가 일시적인 과부하 또는 예정된 작업으로 잠시 요청을 처리할 수 없음
  
  - Retry-After 헤더 필드로 얼마뒤에 복구되는지 보낼 수도 있음

### <mark>왠만해서는 서버에서 500대 에러를 만들면 안 됩니다</mark>

서버에 문제가 터졌을 때 500대 에러를 만들어야 합니다

로직이나 쿼리나 DB 등의 문제가 있을 때 일으키는 겁니다

### HTTP 헤더 개요

- header-field = field-name ":" OWS field-value OWS (OWS : 띄어쓰기 허용)

- field-name은 대소문자 구문 없음

### HTTP 헤더

용도

- HTTP 전송에 필요한 모든 부가정보

- ex) 메시지 바디의 내용, 메시지 바디의 크기, 압축, 인증, 요청 클라이언트, 서버 정보, 캐시 관리 정보...

- 표준 헤더가 너무 많음

- 필요시 임의의 헤더 추가 가능
  
  - helloworld: hihi

  **분류 - RFC2616(과거)**

![](/Users/mac/Desktop/code/학습정리/studynote/image/RFC2616.png)

헤더 분류

- **General 헤더** : 메시지 전체에 적용되는 정보, ex) Connection: close

- **Request 헤더** : 요청 정보 ex) User-Agent: Mozilla/5.0 (Macintosh: ..)

- **Response 헤더** : 응답 정보 ex) Server: Apache

- **Entity 헤더** : 엔티티 바디 정보 ex) Content-Type : text/html, Content-Length:3423

메세지 본문

- 메시지 본문(message body)은 엔티티 본문(entity body)을 전달하는데 사용

- 엔티티 본문은 요청이나 응답에서 전달할 실제 데이터

- **엔티티 헤더**는 **엔티티 본문**의 데이터를 해석할 수 있는 정보 제공
  
  - 데이터 유형(html, json), 데이터 길이, 압축 정보 등등

RFC2616은 폐기되고 RFC7230~7235(2014) 등장

entity -> representation 

representation = representation Metadata + representation Data

표현 = 표현 메타데이터 + 표현 데이터

메시지 바디를 통해 표현 데이터 전달

메시지 본문 = 페이로드

표현은 요청이나 응답에서 전달할 실제 데이터

표현 헤더는 표현 데이터를 해석할 수 있는 정보 제공

- 표현 헤더는 표현 메타데이터와 페이로드 메시지를 구분해야 하지만 일단은 생략

### 표현

표현 헤더는 전송, 응답 둘다 사용

Content-Type

Content-Encoding

    데이터를 압축하기 위해 사용

    읽는 쪽에서 인코딩 헤더 정보로 압축해제

Content-Language

Content-Length 

    바이트 단위

    Transfer-Encoding(전송 코딩)을 사용하면 Content-Length를 사용하면 안 됩니다

#### 협상(Content Negotitaion) : 클라이언트가 선호하는 표현 요청

- Accept : 클라이언트가 선호하는 미디어 타입 전달

- Accept-Charset : 클라이언트가 선호하는 문자 인코딩

- Accept-Encoding, Accpt-Language: 위 내용과 비슷하게 클라이언트가 좋아하는 헤더정보를 요청

협상 헤더는 요청시에만 사용하되 서버가 반드시 그대로 제공해줄 수 있는 건 아닙니다

협상과 우선순위

> GET /event
> 
> Accept-Language: ko-KR,ko;q=0.9,en-US;q=0.8;en;q=0.7

Quality Values(q)

- Quality Values(q)값 사용

- 0~1, 클수록 높은 우선순위

- 생략하면 1

- Accept-Language: ko-KR로 보내되 그게 서버에서 불가능하면 2순위, 3순위 순서로 훑어서 가능한 것으로 보내주게 됩니다

구체적인 것이 우선합니다

Accept :text/*, text/plain, text/plain;format=flowed*

1순위 text/plain;format=flowed

2순위 text/plain

3순위 text/*

구체적인 것을 기준으로 미디어 타입을 맞춥니다

Accept text.*;q=0.3*, text/html;1.0.7, text/html;level=1, text/html;level=2;q=0.4, */*;q=0.5

### 전송 방식

- 단순 전송

- 압축 전송

- 분할 전송 : 오면 바로바로 처리하게 되니 레이턴시가 줄어드는 장점은 있으며 ContentLength는 예상할 수 없어서 적지 못함

- 범위 전송 : 중간에 실패하면 그 이후 부분에 대해서 바이트 범위 정해서 요청하여 받음

### 일반 정보

- From: 유저 에이전트 이메일 정보
  
  - 검색 엔진 같은 곳에서 주로 사용
  
  - 요청에서 사용

- Referer : 이전에 요청했던 웹 페이지 주소
  
  - A -> B로 이동하는 경우 Referer: A를 포함해서 요청
  
  - <mark>이를 통해 유입 경로 분석 가능</mark>
  
  - !! referrer의 오타인데 referer가 너무 일반적이어서 못 고치고 그대로 쓰고 있음..

- User-Agent : 내 웹 브라우저 앱 정보
  
  - 클라이언트 앱 정보
  
  - 통계 정보
  
  - <mark>이걸 통해서 특정 브라우저 또는 버전에서 생기는 문제를 파악하는데 용이</mark>

- Server : 요청 처리하는 Origin 서버의 SW 정보
  
  중간에 거치는 Proxy, cache서버가 아닌 최종목적지 서버를 Origin 서버라고 합니다

- Date: 메시지가 발생한 날짜와 시간

### 특별한 정보

- Host: 요청한 호스트 정보(도메인)
  
  - <mark>요청에서 사용하는 필수 헤더입니다</mark>
  
  - 하나의 서버가 여러 도메인을 처리해야 할 때 host 값을 보고서 그에 맞는 도메인으로 접속 요청하게 됩니다
  
  - 하나의 IP주소에 여러 도메인이 적용되어 있을 때

    서버에서 내부적으로 호스팅 테이블을 만들어서 정리해줍니다

- Location : 페이지 리다이렉트
  
  - 201상태코드에서도 Location값은 요청에 의해 생성된 리소스 URI

- Allow : 허용 가능한 HTTP 메서드
  
  - 405(Method Not Allowed)에서 응답에 포함해야함
  
  - Allow: GET, HEAD, PUT

- Retry-After : 유저 에이전트가 다음 요청을 하기까지 기다려야 하는 시간
  
  - 503 (Service Unavailabe): 서비스가 언제까지 불능인지 알려줄 수 있음
  
  - Retry-After: Fri, 31 Dec 1999 ...
  
  - Retry-After: 120(초단위 표기)



### 인증

- Authorization: 클라이언트 인증 정보를 서버에 전달

-  WWW-Authenticate: 리소스 접근시 필요한 인증방법 정의



Authorization: 클라이언트 인증 정보를 서버에 전달

헤더를 제공하므로 인증 관련된 값을 넣으면 됩니다

WWW-Authenticate : 리소스 접근시 필요한 인증 방법 정의

- 리소스 접근시 필요한 인증 방법 정의

- 401 Unauthorized 응답과 함께 사용

- WWW-Authenticate: Newauth realm="apps", type=1, title="Login to ..~~~~"
  
  - 서버에서 클라이언트에게 이런식으로 요청을 하라고 반환



### 쿠키

클라이언트와 서버는 서로 상태를 유지않으므로 이런 대안을 사용하는 것

- Set-Cookie: 서버에서 클라이언트로 쿠키 전달(응답)

- Cookie: 클라이언트가 서버에서 받은 쿠키를 저장하고, HTTP 요청시 서버로 전달

모든 요청에 다 쿠키를 보내면 문제소지가 있기 때문에

쿠키를 서버에서 세팅을 할 때 세션ID, 만료시간, pathy, 도메인 등을 같이 담습니다



쿠키 사용처

- 사용자 로그인 세션 관리

- 광고 정보 트래킹

쿠키 정보는 항상 서버에 전송

- 네트워크 트래픽 추가 유발

- <mark>최소한의 정보만 사용</mark>(세션 id, 인증 토큰)

- 서버에 전송하지 않고 웹 브라우저 내부에 데이터를 저장하고 싶으면 웹 스토리지(localStorage,sessionStorage)참고

- 주의!
  
  - **<mark>보안에 민감한 데이터는 저장하면 안됨(주민번호,신용카드 번호 등등)</mark>**

쿠키 생명주기

- Set-Cookie : expires(만료일이 되면 쿠키 삭제) max-age=00s(0이나 음수 지정시 쿠키삭제)

- **세션 쿠키 : 만료 날짜 생략하면 브라우저 종료시까지만 유지**

- **영속 쿠키 : 만료 날짜를 입력하면 해당 날짜까지 유지**



쿠키 도메인

명시: 명시한 문서 기준 도메인 + 서브 도메인 포함

- domain = example.org를 지정해서 쿠키 생성
  
  - example.org는 물론이고
  
  - dev.example.org도 쿠키 접근

**생략 : 현재 문서 기준 도메인만 적용**

- example.org에서 쿠키를 생성하고 domain 지정을 생략
  
  - example.org에서만 쿠키 접근
  
  - dev.example.org는 쿠키 미접근



쿠키 경로 Path

ex) path=/home

이 경로를 포함한 하위 경로 페이지만 쿠키 접근

일반적으로 path=/루트로 지정



쿠키 보안 - Secure, HttpOnly, SameSite

- Secure
  
  - 쿠키는 원래 http, https를 구분하지 않고 전송
  
  - Secure를 적용하면 https인 경우에만 전송

- HttpOnly
  
  - XSS 공격 방지
  
  - 자바스크립트에서 접근 불가(document.cookie)
  
  - HTTP 전송에만 사용

- SameSite
  
  - XSRF 공격 방지
  
  - 요청 도메인과 쿠키에 설정된 도메인이 같은 경우만 쿠키 전송



***

### HTTP 헤더 - 캐시와 조건부 요청

#### 캐시 기본 동작

캐시가 없을 때 첫 번째 요청 이후에 똑같은 요청을 하게 되면 똑같은 용량의 데이터를 받아야 합니다

즉,  데이터 변경이 없어도 같은 데이터를 다운받아야 하며 인터넷 네트워크는 매우 느리고 비싸므로 브라우저 로딩 속도는 느려지니 느린 사용자 경험을 주게 됩니다



캐시를 사용하게 되면

서버에서 정보를 전달할 때 헤더에 cache-control에 시간을 지정해서 주게되면

해당 시간범위 안에서 같은 요청이 오게 될 경우 요청보내기도 전에 브라우저 캐시를 찾아서 해결하게 됩니다

매우 빨라지고 빠른 사용자 경험을 제공해줍니다



#### 만약! 캐시 시간이 초과한다면?

다시 요청을 하게 되고 헤더에 cache-control을 포함한 데이터를 제공해주게 되고 그걸 다시 브라우저 캐시에 덮어 씌워줍니다



#### 검증헤더와 조건부 요청



서버에서 기존 데이터를 변경한 것이라면 새로 받는게 맞지만 서버에서 기존 데이터를 변경한 것이 아니면 보내고 다운받는 건 비용 낭비가 됩니다



##### 캐시 시간 초과

캐시 만료후에도 서버에서 데이터를 변경하지 않았음을 확인할 방법이 필요



#### 검증 헤더 추가

last-Modified: Date 값을 헤더에 추가하여 클라이언트에게 전달

웹 브라우저는 last-Modified를 기록하고 있습니다. 60초가 지나버리면 HTTP 요청을 보낼 때 <mark>**if-modifed-since**</mark>라는 http 요청 헤더를 붙여서 같이 보내주게 됩니다

서버에서 <mark>**last-modified**</mark>값의 비교를 통해서 검증하게 됩니다

같으면 304 Not Modified로 반환하면서 cache-control값이 채워지고 전과 동일하지만 http Body가 없는채로 보내주게 됩니다. 헤더 용량만 차지하는 작은 사이즈의 데이터 전송을 하게 되므로 빠릅니다 



검증헤더와 조건부 요청을 동시에 사용한 것입니다

**정리**

- 클라이언트는 서버가 보낸 응답 헤더 정보로 캐시의 메타 정보를 갱신

- 클라이언트는 캐시에 저장되어 있는 데이터 재활용



검증 헤더

- 캐시 데이터와 서버 데이터가 같은지 검증하는 데이터

- Last-Modified, ETag

조건부 요청 헤더

- 검증 헤더로 조건에 따른 분기

- If-Modified-Since: Last-Modified 사용

- If-None-Match: ETag사용

- 조건(변경이 되었는가?)이 만족하면 200 OK

- 조건 불만족시 304 NOT Modified



last Modified, if-Modified-Since 단점

- 1초 미만 단위로 캐시 조정이 불가능

- 날짜 기반의 로직 사용

- **데이터를 수정해서 날짜가 다르지만, 같은 데이터를 수정해서 데이터 결과가 똑같은 경우**

- 서버에서 별도의 캐시 로직을 관리하고 싶은 경우
  
  - ex) 스페이스나 주석처럼 크게 영향이 없는 변경에서 캐시를 유지하고 싶은 경우



### ETag, If-None-Match

ETag(Entity Tag)

- 서버에서 캐시용 데이터에 임의의 고유한 버전 이름을 달아둠
  
  - ex) ETag: "v1.0", ETag: "g3gwgrg42fgg3" 
    
    -> 해시는 컨텐츠가 같으면 똑같은 해시값이 나온다는 것을 착안함

- 데이터가 변경되면 이 이름을 바꾸어서 변경함(Hash를 다시 생성)
  
  - ex) ETag: "aaaaa" -> ETag: "bbbbb"

- 진짜 단순하게 ETag만 보내서 같으면 유지, 다르면 다시 받기



정리

- 캐시 제어 로직을 서버에서 완전히 관리



### 캐시와 조건부 요청 헤더

- <mark>**Cache-Control : 캐시 지시어**</mark>

- Pragma: 캐시 제어(하위 호환)

- Expires: 캐시 유효 기간(하위 호환)



#### Cache-Control

캐시 지시어(directives)

- Cache-Control: max-age
  
  - 캐시 유효 시간, 초 단위

- Cache-Control: no-cache
  
  - 데이터는 캐시해도 되지만, 항상 origin 서버에 검증하고 사용

- Cache-Control: no-store
  
  - **데이터에 민감한 정보가 있으므로 저장하면 안됨**(메모리에서 사용하고 최대한 빨리 삭제)

#### Pragma

캐시 제어 (하위 호환)

- Pragma: no-cache

- HTTP 1.0 하위 호환



#### Expires

캐시 만료일 지정(하위 호환)

- expires: ~~~

- 캐시 만료일을 정확한 날짜로 지정

- HTTP1.0부터 사용

- **지금은 더 유연한 Cache-Control: max-age 권장**

- **Cache-Control: max-age와 함께 사용하면 Expires는 무시**



검증 헤더 : ETag, Last-modified

조건부 요청 헤더 

if-Match, If-None-Match: ETag 값 사용

if-Modified-Since, If-Unmodified-Since: Last-Modified 값 사용



#### 프록시 캐시

중간에 캐시 서버를 두어서 클라이언트의 요청을 프록시 캐시 서버에 접근하도록 만듭니다 원서버로의 접근보다 빠른 반응을 제공합니다

로컬이나 웹브라우저에 저장되는 것을 private 캐시

공용으로 사용하는 cache 서버를 public 캐시



캐시 지시어 - 기타

- Cache-Control : public - 응답이 public 캐시에 저장되어도 됨

- Cache-Control : private - 응답이 해당 사용자만을 위한 것, private 캐시에 저장해야함**(기본값)**

- Cache-Control: s-maxage - 프록시 캐시에만 적용되는 max-age

- Age: 60(HTTP 헤더)
  
  - 오리진 서버에서 응답 후 프록시 캐시 내에 머문 시간(초)



#### 캐시 무효화

Cache-Control 

확실한 캐시 무효화 응답

브라우저에서 일부는 스스로 캐시를 하는 경우가 존재

**Cache-Control: no-cache, no-store, must-revalidate** 째로 다 넣어야 함

**Pragma: no-cache**

    HTTP1.0 하위 호환



**no-cache** : 데이터는 캐시해도 되지만 항상 원 서버에 검증후 사용

**no-store** : 민감한 정보이므로 저장하면 안 됨, 메모리에서 사용후 빠르게 삭제

**must-revalidate**: 캐시 만료 후 최초 조회시 원 서버에서 검증해야 함

    원 서버 접근 실패시 반드시 오류가 발생해야 함 - 504 Gateway Timeout

    must-revalidate는 캐시 유효 시간이라면 캐시를 사용함



must-revalidate와no-cache의 차이

no-cache는 프록시에서 오리진으로 가던 중 순간 네트워크 오류가 나면 캐시 서버 설정에 따라서 캐시 데이터를 반환할 수도 있음. 오류보다는 오래된 데이터라도 보여주자는 의도



must-revalidate는 오리진에 접근 못할 경우 항상 오류 발생

돈 같은 중요한 부분이라면 오류처리 해버리는 것이 맞음..
