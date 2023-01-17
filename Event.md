# Event

어떤 사건이 특히 예외적이거나 중요하게 발생되는 것을 이벤트라고 통상 부릅니다. 이처럼 '어떤 사건의 발생'이라는 측면에서 이벤트라고 부르며 발생된 이벤트에 대한 응답으로 반응하는 것을 리스너(Listener)라고 합니다. 



Event Emitter는 바로 이 이벤트-리스너 패턴으로 구현됩니다.



```javascript
class Emitter {
    constructor(){
        this.events = {}
    }

    on(type,listener){ // event에 키로 type을 지정하고 해당 키 값에 리스너를 추가. 즉, 해당 이벤트의 listener들을 모아 놓는 형태
        this.events[type] = this.events[type] || []
        this.events[type].push(listener)
    }

    emit(type){
        if(this.events[type]){
            this.events[type].forEach((listener)=>{
                listener()
            })
        }
    }
}

module.exports = Emitter
```





Node.js에는 두가지 종류의 이벤트가 있습니다

- System Event - libuv : 컴퓨터에서 시스템적으로 발생되는 이벤트. 예를 들어 파일 읽기, 파일 열기, 통신으로 데이터 받기 등 컴퓨터 시스템적으로 수행되는 이벤트들을 libuv라는 라이브러리가 처리합니다. libuv는 C++로 작성된 Node Core 라이브러리입니다. 시스템상의 이벤트이기 때문에 C++로 작성되었습니다

- Custom Events : 시스템 상 이벤트를 떠나, 직접 구현하여 만들 수 있는 이벤트가 있습니다. 직접 작성된 이벤트를 다루는 것이므로 JS로 작성된 라이브러리가 수행됩니다. 즉, NodeJS의 Event Emitter와 관련된 내장 모듈이 바로 이 이벤트들을 처리해줍니다.



```javascript
const Emitter = require('events')
const eventConfig = require('./config').events
const em = new Emitter()

em.on(eventConfig.GREET, () => {
    console.log('Somewhere, someone said heelo.')
})

em.on(eventConfig.GREET, () => {
    console.log('A Greeting occurred!')
})

em.emit(eventConfig.GREET)

// 실행 결과
//Somewhere, someone said heelo.
//A Greeting occurred!


// config.js
module.exports = {
    events: {
        GREET:'greet'
    }
}
```














