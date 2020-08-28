# setInterval / setTimeout


### setInterval은 정확한 간격마다 실행되지 않을 수 있다

- setInterval

```javascript
// setInterval을 사용하여 100ms 마다 반복 호출
let i = 1;
setInterval(function() {
  func(i);
}, 100);
```

![setInterval](https://mblogthumb-phinf.pstatic.net/MjAxODA3MzFfMjE0/MDAxNTMzMDM2Mjc3MTE4.LOjA-oDsLLJvfCeiM4q_Anj8KuUouonehHsKWU7NuCYg.f470sNJjq3QldxV-pXbEEk_zSUFMm3-EgdVe7TWlfYAg.PNG.jhc9639/setinterval-interval.png?type=w800)

setInterval은 함수가 언제 어떻게 끝나든 상관 없이 일정 시간마다 실행된다. 즉, 일정 시간 주기마다 실행하기 위한 함수이다. 
하지만 실제로는 오차가 발생하기 때문에 오랜 시간동안 사용한다면 setTimeout을 이용한 재귀적 호출이 더 정확하게 동작할 수 있다. (2ms 오차가 일주일 지속되면 1분이상 차이날 수 있음)


- 재귀적 setTimeout

```javascript
// setTimeout 사용하여 100ms 마다 반복 호출
let i = 1;
setTimeout(function run() {
  func(i);
  setTimeout(run, 100);
}, 100);
```

![setTimeout](https://mblogthumb-phinf.pstatic.net/MjAxODA3MzFfOTkg/MDAxNTMzMDM2Mjc3MTQ0.XbYisifdEq7zaTIlohvGjeC663cJ7kfg-TIsOkOdAJ8g.9DVXSKGxsNmtvG0z0pp3NVpfHlQkcFuaTxiIPg8E6pAg.PNG.jhc9639/settimeout-interval.png?type=w800)

재귀적 setTimeout은 함수가 끝난 다음 일정간격 이후에 실행된다. setTimeout 함수 자체가 delay 시간 이후에 함수를 실행하기 때문이다.
setTimeout 함수를 잘 활용하면 긴 시간 사용하더라도 오차가 적은 정밀한 인터벌을 구현할 수 있다.

```javascript
// 밀리세컨드 단위 오차가 유지되는 정밀한 인터벌 함수
var interval = 1000
var start = Date.now()
var expected = Date.now() + interval
function s(){
    var cnt = 0
    for(var i = 0; i < 10000; i ++){
        cnt++;
    }
}
setTimeout(function run(){
    console.log(new Date - start)
    var drift = Date.now() - expected
    s();
    expected += interval;
    setTimeout(run, interval - drift)
});
```

> 정밀하게 일정시간 단위로 함수를 호출할때는 재귀적 setTimeout을 사용해야 한다

중요한 것은 setInterval, setTimeout 모두 지정한 delay 시간을 지키지 못할 수 있다. 
싱글스레드인 자바스크립트 특성상 무거운 작업이 있다면 해당 작업이 종료되기를 기다린다. (이벤트 루프가 스택이 비워질때까지 기다리고 비워지면 실행)
따라서 지정한 시간보다 훨씬 뒤에 실행될 수 있다.


---
### setTimeout 0

```javascript
setTimeout(() => alert("world")); // = setTimeout(() => alert("world"), 0);
alert("hello);

// 실행 결과
// hello
// world
```

```javascript
// 이게 어떻게 실행될지 안다면 이벤트 루프에 대해 정확히 이해하는 것!
function f1() {
  console.log('f1 1');
  setTimeout(() => {
    console.log('f1 2');
    setTimeout(() => console.log('f1 3'));
  });
  console.log('f1 4');
}

function f2() {
  console.log('f2 1');
  setTimeout(() => {
    console.log('f2 2');
  });
  console.log('f2 3');
}

f1();
f2();
console.log('hi');


// 실행 결과
// f1 1
// f1 4
// f2 1
// f2 3
// hi
// f1 2   // 1중 setTimeout
// f2 2   // 1중 setTimeout
// f1 3   // 2중 setTimeout
```

```javascript
// delay 시간을 지키지 못하는 setTimeout
const start = Date.now();
setTimeout(() => console.log(Date.now() - start), 100);
for(let i=0 ; i<1e4 ; i++) {
  console.log('!');
}

// 실행결과 
// ! 
// ! 
// ...  // ! * 10000 개
// 955  // 그때그때 다른 값 하지만 절대 100ms 는 아님
```

기본적으로 javascript 실행 엔진은 싱글스레드이기 때문에 한번에 한가지 일만 처리할 수 있다.
하지만 setTimeout과 같이 callback으로 지정된 함수는 백그라운드 스레드 풀로 던져져서 실행된 함수에 맞게 대기한다.(setTimeout의 경우 delay 시간만큼 대기) 
백그라운드 스레드 풀에서 해당 처리가 끝나면 callback의 함수를 이벤트 큐(태스크 큐)에 순차적으로 쌓는다. 
이벤트 루프는 싱글스레드 호출 스택에 쌓인 실행 라인이 모두 사라지면 이벤트 큐에서 순차적으로 꺼내서 실행하고 이 과정을 반복한다. (setTimeout 안의 setTimeout의 callback이 가장 늦게 실행되는 이유)


![event loop](https://cdn.filestackcontent.com/28uVaQ7sRq6LRmU89ptG)

따라서 setTimeout 0 을 사용하는 이유는 함수의 호출 실행 순서를 바꾸기 위함이다. 
하지만 setTimeout은 호출 스택에 함수가 가득 차 있거나 중첩된 for문 등 무거운 실행이 있다면 정확히 해당 시간 이후에 실행되지 않을 수 있다. (실제로 많은 반복문이 있으면 설정한 delay 시간 보다 지연됨)
이러한 버거운 작업들은 setTimeout으로 해결 가능하다.


```javascript
// 1000개로 분할해서 빠르게 수행, 1e9(= 1e6 * 1000)
let i = 0;
function count() { 
  do {
    i++;
  } while (i % 1e6 != 0); 
  if (i == 1e9) {
    console.log("Done") 
  } else {
    setTimeout(count, 0); 
  } 
} 
count();
```


---
- [setTimeout과 setInterval의 깊은 이해](https://simsimjae.tistory.com/368)
- [자세히 보는 setInterval과 setTimeout](https://m.blog.naver.com/PostView.nhn?blogId=jhc9639&logNo=221330252016&proxyReferer=https:%2F%2Fwww.google.com%2F)
- [호출 스택과 이벤트루프](https://www.zerocho.com/category/JavaScript/post/597f34bbb428530018e8e6e2)
