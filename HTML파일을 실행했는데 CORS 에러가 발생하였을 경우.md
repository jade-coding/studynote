# HTML파일을 실행했는데 CORS 에러가 발생하였을 경우

![](https://blog.kakaocdn.net/dn/buEEqj/btrwJXdkQxl/cuRb9h0mHZJia6T4lnxkf0/img.png)

로컬에서 html을 cli 환경에서 open 명령어로 실행 시켰을시 위와 같이 CORS Policy 관련 에러가 발생할 수 있습니다.

동일 출처 정책으로 인해 호스트, 포트, 프로토콜이 다른 url 으로부터의 응답은 script에서 받을 수 없습니다.

서버에서 응답을 주더라도 브라우저단에서 사용을 할 수 없게 됩니다.

위의 error는 vscode 환경에서 live server로 html 파일을 실행시키거나 http-server 패키지를 이용하면 해결할 수 있습니다.

그런데 왜 로컬에서 위와 같은 현상이 발생하는 것일까요???

**답은 script의 type 속성에 있습니다.**

```xml
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <link rel="stylesheet" href="./circle.css">
</head>
<body>
  <script type="module" src="./circle.js"></script>
</body>
</html>
```

위의 코드는 예제에서 사용한 html 파일입니다.

실제로 type="module"을 제거하면 위의 에러는 발생하지 않습니다.

하지만 type="module"을 주지 않게되면 js 파일 내에서 import, export 등을 할수가 없습니다.

설사 사용하지 않더라도 변수들은 전역변수에 포함되고 전역변수는 모든 범위에서 접근할 수 있는 장점이자 큰 단점을 가지게 됩니다.

**type을 module로 설정한 script 태그에서는 html 파일을 로컬에서 로드했을때 자바스크립트 모듈 보안 요구사항으로 인해 cors오류를 발생시킨다고 합니다.**

> module과 일반적인 script의 차이  
> 
> * **로컬 테스트에서의 주의 사항 -> HTML 파일을 로컬(예를들어 file://URL)에서 로드하려고 하면, 자바스크립트 모듈 보안 요구사항으로 인해 cors오류가 발생합니다. 서버를 통해 테스트 해야 합니다.**  
> 
> * 표준 스크립트와 달리 모듈 내부에서 정의된 스크립트 섹션과는 다르게 동작할 수 있습니다. 이는 모듈이 자동적으로 strict mode를 사용하기 때문입니다.  
> * 모듈 스크립트를 불러올 때 defer 속성은 사용할 필요가 없습니다. 모듈은 자동으로 defer 됩니다.  
> * 모듈 기능을 단일 스크립트의 스코프로 가져왔음을 분명히 해야 합니다.

*** 로컬의 리소스를 요청할때 origin이 null입니다.**

아래는 에러를 반환해준 콘솔창에서의 내용입니다.

![](https://blog.kakaocdn.net/dn/muFbp/btrwwrURXsO/yj5evf5v4OIsLXzrOoipj1/img.png)

**따라서 출처를 같게 만들어주기 위하여 서버를 통해 실행시키면 로컬에서의 cors 상황은 해결될 수 있습니다.**

[JavaScript modules - JavaScript | MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Modules)

[로컬에서 CORS policy 관련 에러가 발생하는 이유 😃 — 공부정리](https://dkrnfls.tistory.com/298)
