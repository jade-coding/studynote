# Blob

**Form Data에 대해서 공부를 하다가 '파일과 같은 Blob 데이터를 보낼 수도 있다'는 문장에서 나의 학습이 시작되었다.**

## Blob(Binary Large Object) : 일련의 데이터를 처리하거나 간접 참조하는 객체

일반적 정의 : Blob은 일반적으로 미디어(이미지, 사운드, 비디오)파일과 같은 큰 용량의 파일을 뜻함.  

- Blob Object 
  
  - Blob Object는 File과 같은 불변 객체를 나타내며, raw Data를 가진다. 
    
    - 추가로 File 인터페이스는 Blob인터페이스의 모든 특성들을 상속 받는다.

유사 배열의 객체임. Type array라고 부름.

[JavaScript 형식화 배열 - JavaScript | MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript/Typed_arrays)  내용 중 일부

> JavaScript 형식화 배열(typed array)은 배열같은 객체이고 원시(raw) 이진 데이터에 액세스하기 위한 메커니즘을 제공합니다. 이미 아시다시피, [`Array`](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array) 객체는 동적으로 늘었다 줄고 어떤 JavaScript 값이든 가질 수 있습니다. JavaScript 엔진은 이러한 배열이 빨라지도록 최적화를 수행합니다. 그러나, audio 및 video 조작과 같은 기능 추가, WebSocket을 사용한 원시 데이터에 액세스 등 웹 어플리케이션이 점점 더 강력해짐에 따라, 빠르고 쉽게 형식화 배열의 원시 이진 데이터를 조작할 수 있게 하는 것이 JavaScript 코드에 도움이 될 때가 있음이 분명해 졌습니다.

> 그러나, 형식화 배열은 보통 배열과 혼동되지는 않습니다, 형식화 배열에 [`Array.isArray()`](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/isArray) 호출은 `false`를 반환하기에. 게다가, 보통 배열에 이용할 수 있는 모든 메서드가 형식화 배열에 의해 지원되지는 않습니다(가령 push 및 pop).

SQL DB에서 유래하였음.

JS에서 Blob은 이진 데이터, 해당 데이터의 크기가 매우 크다는 것을 뜻하지만 사실 그렇지 않은 것에도 종종 사용한다.

**작은 텍스트 파일의 내용도 Blob으로 나타낼 수 있고** Blob은 대게 바이트의 크기를 알아내거나 해당 MIME 타입이 무엇인지 요청하며, 데이터를 작은 Blob으로 잘게 나누는 등의 작업에 사용됨.<mark> 즉, 데이터 자체가 아니라 데이터를 간접적으로 접근하기 위한 객체</mark>

```javascript
var blob = 'something' 

blob.size // blob의 바이트 단위 크기

blob.type // Blob의 MIME 타입을 저장하거나, 알 수 없으면 ""을 저장한다.

var subblob = blob.slice(0,1024, "text/plain")
// Blob 첫 1kb를 텍스트로 가져온다. 

var last = blob.slice(blob.size - 1024, 1024)
// 마지막 1kb를 타입 없이 가져온다.
```

!! 웹 브라우저는 메모리 또는 디스크에 Blob을 저장할 수 있으며, Blob은 비디오 파일과 같이 매우 커서, 메모리에 적재하려면 slice()를 활용하여 작은 조각으로 먼저 분리해야 할 수도 있다.  데이터의 크기가 매우 크기 때문에 디스크를 사용해야 하므로 Blob API는 비동기 방식으로 동작한다.  // (Java의 worker Thread에서는 동기방식으로 동작 가능)

## Blob으로써의 파일

로컬 파일 접근을 허용하는 브라우저에서 <input type="file">요소의 files 프로퍼티는 FileList 객체일 것이다. 이 유사 배열 객체는 사용자가 선택한 0개 이상의 File 객체 목록이다. File 객체는 name과 lastModifiedDate 프로퍼티가 존재하는 Blob 객체다.

```js
<!-- 여러 개의 이미지 파일을 선택할 수 있고, 선택 목록을 fileinfo()로 전달한다. -->
<input type="file" accept="image/*" multiple onchange="fileinfo(this.files)" />



<script>

//선택된 파일의 목록에 관한 로그 정보

function fileinfo(files) { 

//files는 유사 배열 객체다. // 배열은 아니지만 배열과 유사해서 배열 유사 객체.


for(var i=0; i<files.length; i++) {

var f = files[i];

console.log(f.name,    //경로를 제외한 파일명

f.size, f.type,    //size와 type은 Blob 프로퍼티다.

f.lastModifiedDate);    //lastModifiedDate는 File 프로퍼티다.

}

}

</script>
```

## Blob 다운로드

XMLHttpRequest를 활용한 Blob 다운로드 하기js

```javascript
// Get 방식으로 URL의 내용을 Blob으로 가져온 다음 지정한 콜백으로 전달

// 아래 코드 테스트 필요!

function getBlob(url , callback){
 let xhr = new XMLHttpRequest()
 xhr.open("GET", url)
 xhr.responseType = "blob" // blob 형식으로 요 xhr.onload = function(){
 //blob을 콜백으로 전달
 callback(xhr.response) // responseText                    
}

xhr.send(null)
}
```

## Blob 만들기

Blob은 흔히 로컬파일이나 URL, DB와 같이 외부 출처로부터 가져온 데이터들을 나타냅니다. 그러나 웹에 업로드하거나 파일 혹은 DB에 저장하거나 다른 스레드로 전달하기 위해서 웹 어플리케이션 고유의 Blob을 생성해야 할 수도 있습니다. 데이터에서 Blob을 생성하려면 BlobBuilder를 사용하면 됩니다.

```javascript
let bb = new BlobBuilder()
bb.append("이 blob은 이 텍스트와 부호가 있는 32비트 빅 엔디언 정수 10개를 포함한다.")
bb.append("\0") // null 문자 표시

// ArrayBuffer에 몇가지 데이터를 저장한다.
let ab = new ArrayBuffer(5*10)
let dv = new DataView(ab)
for(let i=0; i < 10; i++)dv.setInt32(i*4,i)

bb.append(ab)

var blob = bb.getBlob("x-optional/mime-type-here)

/*
Blob을 작은 조각으로 나누는 slice()메소드를 살펴보았다. 이러한 Blob들을 하나로 결합하려면
이들을 BlobBuilder의 append()메소드로 전달하면 됨
*/
```

## Blob URL

가져오거나 생성한 Blob을 활용하여 실제로 무엇을 할 수 있을까?

Blob로 할 수 있는 가장 간단한 형태 중 하나는 Blob을 가리키는 URL을 생성하는 것

이러한 URL을 생성해두면, DOM이나 스타일시트 또는 XMLHttpRequest의 목적 URL과 같이 일반 URL을 사용한 곳 어디라도 Blob Url을 사용할 수 있다.

**Blob URL은 createObjectURL()함수를 활용하여 생성한다.**

<mark>Blob URL은 영구적이지 않다.</mark> Blob URL은 해당 URL을 생성한 문서를 벗어나거나 종료하면 더 이상 유효하지 않다. 예를 들어 Blob URL을 로컬 스토리지에 저장해두고 다음 사용자가 웹 어플리케이션의 새로운 세션을 시작하였을 때 재활용할 수 있게 하기란 불가능하다.

URL.revokeObjectURL()또는 webkitURL.revokeObjectURL()을 호출함으로써 수동적으로 Blob URL의 효력을 '폐기'시킬수도 있다. 이건 메모리 관리에 관한 문제다. 썸네일 이미지가 한번 출력되면 더이상 Blob이 필요하지 않아서 garbage Collection의 대상이 되어야 한다.

## Blob 읽어 들이기

FileReader 객체는 Blob에 포함된 문자나 바이트들을 읽을 수 있게 하며 BlobBuilder와는 반대의 성격을 가진 것이라고 생각할 수 있습니다(파일만이 아니라 어떠한 Blob을 사용해도 작업이 가능하기 때문에 BlobReader라고 불러도 될 것 같습니다) Blob은 매우 큰 크기의 객체를 파일 시스템에 저장할 수 있으므로 Blob의 내용을 읽으려면 FileReader 객체 API는 XMLHttpRequestAPI와 매우 비슷하게도 비동기적이다.

```javascript
//readAsArrayBuffer() 메서드는 readAsText()와 비슷하지만, 
//파일 내용을 문자열 형태보다는 ArrayBuffer 형태로 다룰 때 좀더 적합하다. 
//아래의 예제는 readAsArrayBuffer()를 사용하여 
//파일의 첫 4바이트를 빅 엔디언 정수로 읽어 들이는 코드다.

<script>

//지정한 blob의 첫 4바이트를 가져온다.
//가져온 이 '매직 넘버'로 파일의 타입을 식별할 수 있으면,
//Blob의 verified_type 프로퍼티를 비동기적으로 설정한다.

function typefile(file) {

var slice = file.slice(0,4);
//비동기 FileReader를 생성한다.
var reader = new FileReader();

//파일의 조각을 읽어 들인다.
reader.readAsArrayBuffer(slice);

reader.onload = function(e) {

var buffer = reader.result; //결과 ArrayBuffer
//결과 바이트의 접근 권한을 획득한다.
var view = new DataView(buffer);

//빅 엔디언으로 4바이트를 읽어 들인다.

var magic = view.getUint32(0, false);

//이 내용으로 파일의 타입을 결정한다.

switch(magic) {

case 0x89504E47: file.verified_type = "image/png"; break;
case 0x47474638: file.verified_type = "image/gif"; break;
case 0x25504446: file.verified_type = "application/pdf"; break;
case 0x504b0304: file.verified_type = "application/zip"; break;

}

console.log(file.name, file.verified_type);
};
};

</script>

<input type="file" onchange="typefile(this.files[0])"></input>
```

참조 : 

- [Blob](https://iamawebdeveloper.tistory.com/106)

- [JavaScript 를 통해 Binary Data 조작하기](http://mohwa.github.io/blog/javascript/2015/08/31/binary-inJS/)

- [Blob - Web APIs | MDN](https://developer.mozilla.org/en-US/docs/Web/API/Blob)
