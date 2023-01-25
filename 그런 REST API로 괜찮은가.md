# 그런 REST API로 괜찮은가

[Day1, 2-2. 그런 REST API로 괜찮은가 - YouTube](https://www.youtube.com/watch?v=RP_f5dMoHFc)

### REST : REpresentational State Transfer

REST : a way of providing interoperablility between computer systems on the Internet

WEB이 개발된 1991년에 어떻게 인터넷에서 정보를 공유할까를 고민하다가 정보들을 HTML이라는 형식으로 전달을 하고 식별자는 URI를 사용하며 전송방법은 HTTP를 사용하자로 정하였습니다

HTTP/1.0 (1994-1996)

대학원생인 Roy T.Fielding이 프로토콜 작업에 참여하면서 94년에 1.0작업을 참여하게 되었는데 1.0 명세가 나오기 전에 당연히 HTTP는 WWW의 전송 프로토콜로 쓰고 있었습니다

이미 웹이 급속도로 성장하며  수많은 웹 서버가 사용되고 있었습니다 

HTTP의 기능을 더하고 고쳐야 하는 상황에 놓이게 된 Roy Fielding이 기존에 구축된 웹상의 호환성과 충돌을 피할 수 없다고 생각을 하였습니다. 그래서 고안한 해결책이 

**HTTP Object Model** 입니다 

이걸 4년후에 REST(1998)라고 Microsoft Research에서 발표하게 됩니다 

2000년에 박사 논문으로 발표하게 됩니다

"Architectural Styles and the Design of Network-based Software Architectures"

한편 

API라는 것이 만들어지기 시작합니다

XML-RPC(1998) 프로토콜 개발 By MS => SOAP

Salesforce API(2000. 2) 공개 / 당시 SOAP를 사용해서 만들었습니다

flickr API(2004. 8) 공개 / SOAP라는 형태와 REST라는 형태로 만들어냅니다

REST라는 형태가 SOAP에 비해서 훨씬 짧았습니다

REST는 단순하고 규칙도 적고 쉽다고 느끼게 해줬고 갈수록 REST API인기가 상승합니다

2006년 AWS가 자사 API의 사용량의 85%가 REST API임을 밝힘

2010년 Salesforce.com, REST API 를 지원하게 됨

CMIS(2008)라는 것이 등장하였습니다

- CMS를 위한 표준

- EMC, IBM, MS등이 함께 작업

- **REST 바인딩 지원**

**<mark>로이필딩은 거기에는 REST가 없다고 주장합니다</mark>**

#### "No REST in CMIS"

##### MS REST API Guidelines(2016) 등장

- uri는 https://{serviceRoot}/{collection}/{id}형식이어야 한다

- GET, PUT, DELETE, POST, HEAD, PATCH, OPTIONS를 지원해야한다

- API 버저닝은 Major.minor로 하고  uri에 버전 정보를 포함시킨다

- 등등..

**로이필딩은 이것도 REST API가 아니라고 이건 그냥 HTTP API라고 말합니다**

또 이런 트윗을 SNS에 올립니다. "REST APIs must be hypertext-driven, REST API를 위한 최고의 버저닝 전략은 버저닝을 안 하는 것"

REST API : REST 아키텍쳐 스타일을 따르는 API

REST : 분산 하이퍼미디어 시스템(예: 웹)을 위한 아키텍쳐 스타일이면서 동시에 아키텍쳐 스타일의 집합..  

아키텍쳐 스타일 : 제약 조건의 집합

즉 제약 조건들을 모두 따라야 REST API를 따른다고 말할 수 있습니다

## REST를 구성하는 스타일

- client-server

- stateless

- cache

- **uniform interface** 

- layered system

- code-on-demand(optional) : 서버에서 클라이언트로 보내서 실행할 수 있어야 한다

### <mark>Uniform Interface의 제약조건</mark>

- identification of resources

- manipulation of resources through representations / 리소스 만들거나 삭제하거나 할 때 HTTP 메시지에 넣어서 통신해야 한다는 것을 말함

-- 이 2가지는 거의 모든 것들이 지키지 못하고 있습니다 --

- **self-descriptive message** 

- **hypermedia as the engin of application state(HATEOAS)** 

<mark>****Self-descriptive message : 메세지는 스스로 설명해야 한다**</mark>**

> GET / HTTP/1.1
> 
> 이 HTTP 요청 메시지는 뭔가 빠져있어서 self-descriptive 하지 못하다
> 
> GET / HTTP/1.1
> 
> Host: www.example.org
> 
> 목적지를 추가하면 이제 self-descriptive

> HTTP/1.1 200 OK
> 
> [{"op":"remove", "path":"/a/b/c"}]
> 
> 정보를 해석하는 방법이 적혀지지 않아서
> 
> self-descriptive 하지 못하다
> 
> HTTP/1.1 200 OK
> 
> Content-Type: application/json
> 
> [{"op":"remove", "path":"/a/b/c"}]
> 
> 근데 사실 이것만으로는 op ,path가 뭘 말하는지 모릅니다..
> 
> 올바르게 이해하기 위해서는 
> 
> HTTP/1.1 200 OK
> 
> Content-Type: application/json-patch+json
> 
> [{"op":"remove", "path":"/a/b/c"}]
> 
> json-patch라는 명세를 찾아가서 이해하고서 메시지를 찾아가면 이해할 수 있습니다
> 
> 즉, 메시지 내용으로 온전히 해석이 다 가능해야 하지만 오늘날 REST API는 이걸 충족하지 않습니다

**<mark>HATEOAS :  애플리케이션의 상태는 Hyperlink를 이용해 전이되어야 한다</mark>**

HTML 형식의 데이터인 경우 Hyperlink를 통해서 그 다음 상태로 전이가 되기 때문에 HATEOAS를 만독합니다 JSON으로 해도 만족할 수 는 있는데 Link: </article/1>; rel="previous", </article/3>; rel="next"라고 표현을 해줘야 했습니다 이 정보는 링크 헤더가 온전히 해석되어서 어떻게 링크가 되었는가를 이해해서 다음 상태로 전이가 되도록 하면 HATEOAS를 만족합니다

### 왜 Uniform Interface를 만족해야 하는가?

##### 독립적인 진화를 하기 위해서

- 서버와 클라이언트가 각각 독립적으로 진화한다

- **서버의 기능이 변경되어도 클라이언트를 업데이트할 필요가 없다**

- REST를 만들게 된 계기 : "How do i improve HTTP without breaking the Web"

##### 독립적인 진화가 잘 지켜지고 있는가?

**웹**

- 웹 페이지를 변경했다고 해서 웹 브라우저를 업데이트할 필요는 없다

- 웹 브라우저를 업데이트 했다고 웹 페이지를 변경할 필요도 없다

- HTTP 명세가 변경되어도 웹은 잘 동작한다

- HTML 명세가 변경되어도 웹은 잘 동작한다

=> 매우 잘 만족시키고 있음

모바일은 서버에서 만든 것을 이용하기 위해 업데이트를 해야하는 경우들이 생깁니다. 그걸 토대로 미루어보면 모바일 환경은 REST Architecture를 따르지 않는다고 생각할 수 있습니다

웹은 어떻게 한 걸까? 이들의 노력으로 만들었습니다

W3C Working Group (HTML) , IETF Working Group(HTTP), 웹 브라우저 개발자들, 웹 서버 개발자들 

HTML5 첫 초안에서 권고안 나오는데 6년,

HTTP/1.1 명세 개정판 작업하는데 7년 - 문서만 다듬는데 7년, 하위 호환성을 깨뜨리면 안 되서 많은 토론으로 작업한 것입니다. 명세에 바뀐게 없는데도요.

### <mark>상호운용성(interoperability)에 대한 집착</mark>

- Referer 오타지만 안 고침 - 고치는 순간 웹이 깨지기 때문에..

- charset 잘못 지은 이름이지만 안 고침 - 원래는 인코딩이라고 지어야 하지만 당시에 개발하던 분들이 인코딩에 대해서 개념이 없어서 character set하고 같은 건줄알고 이대로 간 것입니다

- HTTP 상태코드 416 포기함( I'm a teapot) - 만우절때 만든 이상한 스펙 HTCPC인가에서 쓰는 상태코드를 만들었었는데 수많은 서버 구현체들이 이걸 HTTP 상태코드로 이미 구현을 해버리는 상황이 벌어졌습니다 제거하려고 시도하다가 이미 만들어진 구현체들과의 상호운용성도 지켜줘야 하기 때문에.. 웹을 안깨지게 하고자.. 416은 그냥 영구 결번으로... 

- HTTP/0.9 아직도 지원함 (크롬, 파이어폭스) - 크롬에서 0.9를 이제 빼도 되지 않겠느냐고 이야기를 했었으나 한번 빼봤더니 몇몇 일부 proxy에서 오류가 생기는게 있어서 결국 그 빼버리는 걸 포기합니다.. 

만약 이런 노력이 없다면 웹도 000 v.00은 지원하지 않습니다 와 같은 상황이 생깁니다

웹서버가 브라우저 호환성을 제대로 고려하지 않고 구현이 되었을 때 나오는 상황처럼 되는 것입니다

### REST가 웹의 독립적 진화에 도움을 주었나

- HTTP에 지속적으로 영향을 줌

- Host 헤더 추가

- 길이 제한을 다루는 방법이 명시 (414 URI Too Long 등)

- URI 에서 리소스의 정의가 추상적으로 변경됨: '문서의 위치' -> '식별하고자 하는 무언가'

- 기타 HTTP와 URI에 많은 영향을 줌

- HTTP/1.1 명세 최신판에서 REST에 대한 언급이 들어감 / Representation에 대한 언급을 하면서 REST 논문에 대해 소개함

- Reminder : 로이필딩이 HTTP와 URI 명세의 저자 중 한명 입니다

    

### REST는 성공했는가?

- REST는 웹의 독립적 진화를 위해 만들어졌고 그대로 이뤄졌다.

### REST API는?

- REST API는 REST 아키텍쳐 스타일을 따라야한다.

- 오늘날 스스로 REST API라고 하는 API들의 대부분이 REST 아키텍쳐 스타일을 따르지 않습니다

##### REST API도 저 제약조건들을 다 지켜야 하는가?

로이필딩은 다 지켜야 한다고 말했습니다. 

**하이퍼 텍스트를 포함한 self-descriptive message의 uniform interface를 통해 리소스에 접근하는 API여야 한다고요**



#### 처음 REST를 알았을때는 SOAP가 복잡하고 REST는 단순하다고 생각을 했는데 오늘날 알고보니.. REST도 SOAP도 어렵다는 것을요



**원격 API가 꼭 REST API여야 하는건가?**

-> 로이필딩은 아니라고 답했습니다.

> **시스템 전체를 통제할 수 있다고 생각하거나 진화에 관심이 없다면 REST에 대해 따지느라 시간을 낭비하지 마라**

시스템 전체를 통제할 수 있다는 것은 제가 서버개발자라고 하면 제가 클라이언트 개발자를 통제할 수 있는가, 내가 REST 클라이언트랑 서버를 만들어요 그러면 완전한 통제가 가능합니다. 



진화에 관심이 없다는 건, 예를 들면 계속 여러번 업데이트  시켜도 소수의 사용자들이 불만이 있어도 상관없다고 여기는 것을 말합니다 

불평할게 그리 많지 않습니다. 반드시 우리가 진화를 달성할 목표로 삼을 필요가 없습니다 



만약 오랜시간걸치는 진화를 설계하고 싶다면 그제서야 REST에 관심을 가져야 합니다



### 그럼 이제 어떻게 할까??

1. REST API를 구현하고 REST API라고 부른다

2. REST API 구현을 포기하고 HTTP API라고 부른다

3. REST API가 아니지만 REST API라고 부른다 **(현재상태)**



로이필딩은 **제발 제약 조건을 따르던지 아니면 다른 단어를 써라**라고 말하고 계십니다



##### 왜 API는 REST가 잘 안되나?

|            | 흔한 웹 페이지 | HTTP API |
| ---------- | -------- | -------- |
| Protocol   | HTTP     | HTTP     |
| 커뮤니케이션     | 사람 - 기계  | 기계 - 기계  |
| Media Type | HTML     | JSON     |

Media Type의 차이가 문제였던 것입니다

|                  | HTML        | JSON      |
| ---------------- | ----------- | --------- |
| Hyperlink        | 가능(a태그)     | 불가능(정의 X) |
| Self-descriptive | 가능(HTML 명세) | 불완전       |

html명세는 어떤 태그는 어떤 태그인지 다 정의되어있지만 JSON은 키밸류에대해서 정의가 명확하지 않습니다. 문법과 Parsing 에 대한 정의는 있지만 그 안에 값들(key,value)이 어떤 의미를 가져야 한다는 정의가 없습니다

결국 -> JSON은 문법 해석은 가능하지만, 의미를 해석하려면 별도의 문서(API 문서 등) 필요



##### Self-descriptive와 HATEOAS가 독립적 진화에 어떻게 도움이 되는가?

Self-descriptive : 확장 가능한 커뮤니케이션

- 서버나 클라이언트가 변경되더라도 오고가는 메시지는 언제나 self-descriptive하므로 언제나 해석이 가능하다



HATEOAS : 애플리케이션 상태 전이의 late binding

- 어디서 어디로 전이가 가능한지 미리 결정되지 않는다. 어떤 상태로 전이가 완료되고 나서야 그 다음 전이될 수 있는 상태가 결정된다. 즉 쉽게 말해 링크는 동적으로 변경될 수 있다 - 서버가 링크를 마음대로 바꿀 수 있습니다



### 그럼 REST API처럼 바꿔보면,

1. self-descriptive
   
   - Custom Media type을 등록하는 방법
   1. 미디어 타입을 하나 정의한다
   
   2. 미디어 타입 문서를 작성한다  이 문서에 "id"는 무엇이고 "title"은 어떤 의미인지 정의한다
   
   3. IANA 미디어 타입을 등록한다 2단계에서 만든 문서를 미디어 타입의 명세로 등록한다
   
   4. 이제 이 메시지를 보는 사람은 명세를 찾아갈 수 있으므로 이 메시지의 의미를 온전히 해석한다
   
   -> 단점 : 매번 media type을 정의해야 한다
   
   
   
   - Profile
     
     > Link: <https://example.org/docs/example>; rel="profile" 이런식으로 통신하는데 같이 붙여줍니다
   1. "id","title" 등의 의미를 정의한 명세를 작성한다
   
   2. Link 헤더에 profile relation으로 해당 명세를 링크한다
   
   3. 이제 메시지를 보는 사람은 명세를 찾아갈 수 있으므로 이 문서의 의미를 온전히 해석할 수 있다
   
   -> 단점 : 1. 클라이언트가 Link헤더, profile을 이해해야 한다 2. Content negotitation을 할 수 없다
   
   
   
   미디어 타입이 아니라 link 헤더로만 판단하기 때문에 컨텐츠 타협을 못합니다
   
   > 참고!  [컨텐츠 타협(content Negotiation) : 네이버 블로그](https://m.blog.naver.com/anrdl06/220462703884)
   > 
   > <u>컨텐츠 타협(content Negotiation)</u>
   > 
   > - 하나의 URL에 문서가 여러개 존재하도록 할 목적으로 연구되고 있는 것,
   > 
   > 브라우저와 서버가 타협을 해서 사용자에게 가장 알맞은 페이지를 주는 것을 말한다.
   > 
   > 제공하는 리소스르 언어와 문자세트, 인코딩 방식 등을 기준으로 판단.
   > 
   > 3가지 방식이 존재
   > 
   > 1. 서버 구동형 네고시에이션(server-driven Negotiation) // 서버에서 판단
   > 
   > 2. 에이전트 구동형 네고시에이션(agent-driven Negotiaion) // 클라이언트에서 판단
   > 
   > 3. 트랜스페어런트 네고시에이션(transparent Negotiation) // 서버와 클라이언트가 각각 판단



2. HATEOAS
   
   방법1 : data로
   
   1. 데이터에 다양한 방법으로 하이퍼링크를 표현한다
   
   > [{
   > 
   > "link":"https://example.org" }]
   
       
   
   이런식으로 uri 템플릿을 정의해서 직접 id를 입력해서 찾아가서 확인할 수 있습니다
   
   > {
   > 
   > "links":{
   > 
   > "todo":"https//example.org/{id}"
   > 
   > },
   > 
   > "data":[
   > 
   > {
   > 
   > "id":1,
   > 
   > "title":"등산가기"
   > 
   > },
   > 
   > {
   > 
   > "id":2,
   > 
   > "title":"바다가기"
   > 
   > }
   > 
   > ]
   > 
   > }

                > 단점 : 링크를 표현하는 방법을 직접 정의해야 한다



사실, JSON으로 하이퍼링크를 표현하는 방법을 정의한 명세들이 있어서 이를 활용할 수 있습니다

> ex) Content-Type: application/vnd.api+json 



- JSON API

- HAL

- UBER

- Siren

- Collection+json

- ...

    - 단점 : 명세들 요구사항처럼 기존 API를 바꿔야한다



방법2 :  HTTP 헤더로

Link, Location 등의 헤더로 링크를 표현한다

> Location: /todos/1



    - 단점 : 정의된 relation만 활용한다면 표현에 한계가 있다



**HATEOAS는 data, 헤더 모두 활용하면 좋습니다**

**Hyperlink는 반드시 uri여야 하는건 아닌가?**

-> 어떻게든 상관없습니다

| 종류                      | 예                             |
| ----------------------- | ----------------------------- |
| uri                     | https://example.com/users/vvv |
| uri reference(absolute) | /users/vvv                    |
| uri reference(relative) | vvv                           |
| uri template            | /users/{username}             |



**Media Type 등록은 필수인가? No**

의도된 청중들이 이해할 수 있다면 굳이 IANA에 등록하지 않아도 됨

물론 IANA에 등록하면 좋습니다

- 누구나 쉽게 사용할 수 있게 된다

- 이름 충돌을 피할 수 있다

- 등록이 별로 어렵지 않다고 주장..?



**REST는 긴 시간에 걸쳐(수십년)진화 하는 웹 애플리케이션을 위한 것이다**

REST를 따를 것인지는 API를 설계하는 이들이 스스로 판단하여 결정해야 한다

따른다면 Self-descriptive와 HATEOAS를 만족시켜야 한다



****



REST를 따르지 않겠다면 HTTP API라고 부를 수 있고 

그냥 이대로 REST API라고 부를 수도 있다... - 로이필드는 싫어하겠지만..




