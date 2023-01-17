# Async

#### ECMA script 2017(ES8)에서 소개된 비동기 방식 함수

async 함수는 함수안의 await 구문과 함께 비동기 작업을 제어함.

**<mark>await 키워드는 반드시 async 함수 안에서만 유효함</mark>**

처음 async 함수가 호출되어 await 키워드가 있는 비동기 작업(promise 객체)이 실행되면, 해당 비동기 함수는 이벤트 루프를 통해 비동기로 작업을 처리. **그동안 async 함수는 이러한 비동기 작업이 완료될 때까지 일시 중지 상태로 비동기 작업(Promise)의 해결(resolve)을 기다림.** 이 작업이 완료되면 async 함수가 다시 실행되고 함수 결과를 반환

async 함수를 선언하는 방법에는 **<mark>async 함수 선언문과 표현식</mark>**이 있음

```javascript
function doJob(name, person){
    return new Promise((resolve, reject)=>{
        setTimeout(()=>{
            if(person.stamina > 50){
                person.stamina -= 30;
                resolve({
                    result:`${name} success`,
                });
            }else{
                reject(new Error(`${name} failed`));
            }
        },1000)
    })
}

const harin = {stamina : 100};

const execute = async function() {
    try{
        let v = await doJob('work',harin);
        console.log(v.result);
        v = await doJob('study', harin);
        console.log(v.result);
        v = await doJob('work', harin);
        console.log(v.result);
        v = await doJob('study', harin);
        console.log(v.result);
    }catch(e){
        console.log(e);
    }
}

execute();
```

> 실행결과
> 
> work success
> 
> study success
> 
> Error: work failed
