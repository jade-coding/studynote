# Map 객체

Map은 ES6부터 표준으로 추가된 데이터 집합체(Collection)의 한 종류

키(Key)와 값(Value)의 한 쌍으로 저장하고, 중복된 키는 허용하지 않음

반복 가능한 객체로써 Map 객체 내부를 순환할 수 있음

##### Object와는 다른 Map객체만의 특징

- Map 객체 키(Key)는 다양한 자료형 값으로 정의할 수 있으며 Object는 문자나 Symbol자료형만 가능

- Map 객체는 반복 가능한 객체로 Symbol.iterator 이 기본적으로 정의되어 있으나 Object는 없음





```javascript
const map = new Map();

map.set('one',1);
map.set('two',1);

console.log(map.get('one'));
console.log(map.get('two'));
map.delete('one');

console.log(map.has('one'))
console.log(map.has('two'))


/*
1
1
false
true
*/

```



```javascript
map.set('one',1);
map.set(2,'two')
map.set([1,2,3], 'Three elements')
map.set({a :'A',b :'B'}, 'object elements')
map.set(function(){}, 'functional elements')

for(let a of map) console.log(a);
console.log(map.size);

/* 실행결
[ 'one', 1 ]
[ 2, 'two' ]
[ [ 1, 2, 3 ], 'Three elements' ]
[ { a: 'A', b: 'B' }, 'object elements' ]
[ [Function (anonymous)], 'functional elements' ]
5
*/
```



```javascript
const map = new Map();

map.set('one',1);
map.set('two',2);
map.set('three',3);

const keys = map.keys();
const values = map.values();
const entries = map.entries();

console.log(keys.next().value);
console.log(values.next().value);
console.log(entries.next().value);

console.log(keys);
console.log(values);
console.log(entries);

/*
one
1
[ 'one', 1 ]
[Map Iterator] { 'two', 'three' }
[Map Iterator] { 2, 3 }
[Map Entries] { [ 'two', 2 ], [ 'three', 3 ] }
*/
```

- keys() :  Map 객체 요소의 키(key)정보만 모아 Iterator 객체로 반환함

- values() : Map 객체 요소의 값(value)정보만 모아 Iterator 객체로 반환

- entries() : Map 객체 요소의 키(key)와 값(value)을 한 쌍으로 배열로 만듦. 

Iterator 객체를 통해 반환된 값들을 순회할 수 있음


