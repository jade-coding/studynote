# DOM

## DOM (Document Object Model) 문서객체모델

DOM 은 HTML과 XML을 위한 프로그래밍 인터페이스

인터페이스는 해당 타입에 어떤 속성과 메소드들이 존재해야 하는지 기술만 하고 실제 구현은 각 구현체에서 다르게 구현 할 수 있게 약속을 정의해놓은 것

DOM은 인터페이스, browser에서 실제 구현



DOM은 Js를 통해서 사용할 수 있음. 

DOM을 통해서 문서의 구조, 스타일, 내용을 변경할 수 있음.

HTML 문서를 브라우저가 읽으면 그 문서에 해당하는 DOM이 만들어짐 -> DOM은 객체 형태로 표현됨





```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Dom 이해하기</title>
</head>
<body>
    <div id="div1">hello DOM</div>
    <ul>
        <li>list item1</li>
        <li>list item2</li>
    </ul>
    <script>
        const div1El = document.getElementById('div1');
        console.log(div1El.innerHTML); // hello DOM

        console.log(div1El.nodeType); // 1
        console.log(div1El.nodeType === Node.ELEMENT_NODE); // true

        console.log(div1El.constructor) // ƒ HTMLDivElement() { [native code] }
        console.log(div1El instanceof HTMLDivElement) //  true
        console.log(div1El instanceof HTMLElement) // true
        console.log(div1El instanceof Element) // true
        console.log(div1El instanceof Node) // true
        console.log(Element.prototype) // Element {…}after
        console.log(div1El.tagName) // DIV

        const div1El2 = document.querySelector('#div1')
        console.log(div1El2.innerHTML) // hello DOM

        const liEls = document.querySelectorAll('ul li');
        console.log(liEls.item(0).innerHTML) // list item1
        console.log(liEls.item(1).textContent) // list item2
    </script>
</body>
</html>

```



body 태그 안에 div와 ul 태그들을 작성하고 마지막에 script 태그를 작성합니다. 마지막에 script 태그를 작성하는 이유는 앞의 태그들을 브라우저가 먼저 읽어야지 자바스크립트 코드가 해석되기 전에 DOM으로 만들어져서 <script> 태그의 JS 코드에서 해당 dom에 접근할 수 있기 때문입니다.



DOM은 document 전역 객체를 통해 접근할 수 있습니다. 그리고 document 객체는 DOM에 접근하기 위해 다양한 메소드를 제공합니다.



getElementById는 아이디를 인자로 전달받아 해당 아이디의 요소를 문서에서 찾아 반환합니다. <mark>div 태그를 자바스크립트에서 DOM객체로 표현되고 이 객체가 Node입니다.</mark> 태그를 포함한 문서에 작성되는 모든 것들은 노드가 되고 이러한 노드는 여러 종류가 있습니다.



> ### Notes
> 
> HTML 문서의 태그들은 자바스크립트에서 노드가 됩니다. Node는 여러 하위 타입을 가지게 됩니다. 다음은 주요 노드 타입의 목록이고 각 타입은 상수로 정의됩니다.
> 
> - ELEMENT_NODE = 1 ex) <body> <a> <p> <script> <style> <html> <h1>
> 
> - ATTRIBUTE_NODE = 2 ex) class = "hello"
> 
> - TEXT_NODE = 3 ex) HTML문서의 텍스트들
> 
> - COMMENT_NODE = 8 ex) HTML문서의 주석들 (<!--- --->)
> 
> - DOCUMENT_NODE = 9 ex)document
> 
> - DOCUMENT_TYPE_NODE = 10 ex)<!DOCTYPE html>



```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>DOM 내비게이션 예제</title>
    <link rel="stylesheet" href="css/dom-navigation.css">
</head>
<body>
    <h1>국내여행지</h1>
    <ul>
        <li>서울</li>
        <!-- <li>수원</li> -->
        <li>제주</li>
        <li>속초</li>
        <li>부산</li>
    </ul>
    <script>
        const bodyEl = document.body;
        const bodyElChildren = bodyEl.children;
        console.log(bodyElChildren);
/*
HTMLCollection(3)
0: h1
1: ul
2: script
length: 3
[[Prototype]]: HTMLCollection
*/
        const cityList = bodyElChildren[1];

        console.log(cityList.children.length); // 4
        const item2 = cityList.children.item(1);
        console.log(item2); //제주
        console.log(item2.previousElementSibling); //서울
        console.log(item2.previousElementSibling);//서울
        console.log(item2.previousElementSibling.previousElementSibling);// null
        console.log(cityList.childNodes)
        /*
        NodeList(11)
        0:text
        1:li
        2:text
        3:comment
        4:text
        5:li
        6:text
        7:li
        8:text
        9:li
        10:text
        length:11
        */
        console.log(item2.firstChild)// 제주
        console.log(item2.lastChild) // 제주
        console.log(item2.parentElement) // <ul> <li>서울</li> <li>제주</li> ... </ul> 로 구성됨 
    </script>
</body>
</html>
```








