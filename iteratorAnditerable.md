# Iterator&iterable객체

Js에서는 반복 동작에 대햔 규약으로 Iterable, Iterator를 정의함

### The Iterable Protocol(반복 가능한 규약)

객체 안의 값들을 반복할 수 있도록, 반복 동작을 정의하는 것을 허용

=> 즉, 반복 동작에 대한 정의

**우선 객체가 반복 가능하려면 @@iterator 메소드를 구현해야 함**

**속성 키(key)는 반드시 <mark>Symbol.iterator이어야 함</mark>**

속성 값(value)은 <mark>매개변수가 없는 함수가 대입됨</mark>

이 함수는 **반복자 규약(Iterator Protocol)을 따르는 객체를 반환**함

### The Iterator Protocol(반복자 규약)

연속된 값을 만드는 방법을 정의함

객체가 반복자 규약을 충족하려면, **<mark>next 메소드를 가지고 있어야 함</mark>**

객체 속성 키(Key)는 next()이고, 속성값(Value)은 매개변수가 없는 함수로 정의

**함수는 value와 done 속성을 가진 객체를 반환함 => 반복자의 next 메소드를 호출하면, 속성 키 이름이 value,done인 객체가 반환됨. 이런 반복자 규약을 충족하는 객체가 iterator**

```javascript
// 반복가능한 객체와 반복자 이해하기

const items = ['j','a','v','a','s','c','r','i','p','t'];
const seq = {
    [Symbol.iterator](){
        let i = 0;
        return {
            next(){
                const value = items[i];
                i++;
                const done = i > items.length;
                return {value, done};
            }
        }
    }
}

for(let s of seq) console.log(s); // for of 구문이 가능해짐
/*
j
a
v
a
s
c
r
i
p
t
*/
const [a,b,c, ...arr] = seq
console.log('a >>> ',a); // a >>>  j
console.log('b >>> ',b); // b >>>  a
console.log('c >>> ',c); // c >>>  v
console.log('arr >>> ', arr);
/*
arr >>>  [
    'a', 's', 'c',
    'r', 'i', 'p',
    't'
  ]
  */
```

이처럼 iterable를 직접 작성하는 것 외에도, 자바스크립트의 몇몇 내장 객체들은 기존에 내장 되어 있는 iterable를 통해 기본 반복 동작을 할 수 있습니다.
**<mark>Array, String, Map, Set이 해당함. </mark>**

이들은 반복 가능한 동작(Iterable)을 허용함으로써 반복 가능한 객체
**Prototype에서 @@Iterator 메소드를 이행한 Symbol.iterator이 구현되어 있어 반복이 가능함**
