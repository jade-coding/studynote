# Promise

##### ECMA script2015에서 비동기 처리를 위해 Promise 객체가 소개됨

Promise는 객체로써 언젠가 완료될 일(계산)을 나타냄. 완료되면 하나의 값을 결과로 반환하는데 실패하면 실패의 이유를 반환할 수도 있음

#### Promise 객체는 3가지 상태를 가짐

- 대기중(Pending) : 아직 결과가 없는 상태. 약속했지만 아직 약속에 대한 겨로가가 나오지 않은 상태임

- 이행됨(Fulfilled) : 비동기 처리가 성공적으로 완료되어 약속을 이행한 상태. 이때 결과로 하나의 값이 전달됨

- 거부됨(Rejected) : 비동기 처리가 실패한 상태. 약속이 거부되어 그 결과로 거절된 이유를 전달

#### Promise 객체는 2가지 메소드를 가짐

- then(onFulfilled, onReject) : 약속이 완료됐을 때 호출될 함수들을 정의. 1째 인자로 전달되는 함수는 약속이 이행되었을 때 호출되고 2째 인자는 거부됐을때 호출됨. **두 전달 인자 함수들은 매개변수를 가지는데 각각의 결과가 매개변수를 통해 전달됨**

- catch(onReject) : **약속이 거부됐을 때 호출될 함수(onReject)를 등록합니다.**

```javascript
function promiseForHomework(mustDo){
    return new Promise((resolve, reject)=>{
        setTimeout(()=>{
            console.log('doing homework');
            if(mustDo){
                resolve({
                    result:"homework-result!"
                })
            }else{
                reject(new Error('Too lazy!'));
            }
        },3000);
    })
}

const promiseA = promiseForHomework(true);
console.log('promiseA created!');

const promiseB = promiseForHomework();
console.log('promiseB created!');

promiseA.then(v => console.log(v));

promiseB.then(v => console.log(v)).catch(e => console.log(e));
```

하나의 비동기 계산이 다른 비동기 계산의 결과에 의해 처리되어야 하는 경우가 많기에 Promise가 나오기 전에는 콜백 패턴으로 비동기 처리를 하였음.

다만, 중첩된 비동기 코드들을 처리하다보면 피라미드 형태의 코드 일명 <mark>콜백지옥</mark>을 형성

Promise의 then 메소드에서 새로운 비동기 코드를 실행하는 Promise를 반환할 수 있는데 다음 then 메소드는 새롭게 만들어진 Promise 코드가 이행되기 전까지 호출되지 않음

```javascript
function doJob(name, person){
    return new Promise((resolve, reject)=>{
        setTimeout(()=>{
            if(person.stamina > 50){
                resolve({
                    result:`${name} success`,
                    loss:30
                });
            }else{
                reject(new Error(`${name} failed`));
            }
        },1000)
    })
}

const harin = {stamina : 100};
doJob('work',harin)
.then(v => {
    console.log(v.result);
    harin.stamina -= v.loss;
    return doJob('study', harin);
})
.then(v => {
    console.log(v.result);
    harin.stamina -= v.loss;
    return doJob('work',harin);
})
.then(v => {
    console.log(v.result);
    harin.stamina -= v.loss;
    return doJob('study', harin);
})
.catch(e => console.log(e));
```

> ### 실행결과
> 
> 1. **work success**
> 
> 2. **study success**
> 
> 3. **Error: work failed`**
