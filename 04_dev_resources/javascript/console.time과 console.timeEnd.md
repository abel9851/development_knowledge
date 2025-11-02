- elapsed time(경과시간)을 확인하기 위해 `console.time(<name>)` 과 `console.timeEnd(<name>)` 을 사용할 수 있다.
- 두 개는 한쌍이므로 추가로 시간을 더 확인하기 위해 `console.time` 없이 `console.timeEnd` 를 사용하면 `(node:7497) Warning: No such label 'timer' for console.timeEnd() (Use `node --trace-warnings ...` to show where the warning was created)`  라고, label이 없다며 경고문이 출력된다.
- 아래의 예제 코드에서는 timer1과 timer2라는 label을 사용해서 확인한 결과, 경과시간이 javascript 코드 자체가 실행되었을 때부터 시간을 체크하는게 아니라  `console.time("timer")` 가 호출된 순간부터 시간을 체크한다는 것을 알 수 있다.
- `console.time` 과 `console.timeEnd` 는 동기, 비동기 코드의 특정 섹션에 대해 측정할 때 유용하다.
- 이와 다르게 전체 실행 범위, 특히 비동기 이벤트와 연관된 시간을 측정하고 싶다면 [[Date.now()]] 가 유용할 것이다. Performance API인, [[performance.now()]]는 브라우저환경에서 더욱 정확한 시간을 알려준다고 한다. 
	- chrome과 같은 브라우저의 개발자 도구나 Node's profiler등을 사용해도 된다.
```javascript
console.time("timer");

// 1초 뒤에 "A"를 출력
setTimeout(
    () => {
        console.log("A");
        console.timeEnd("timer"); // This will log the time taken for the timeout
    },
    1000
);

setTimeout(() =>{
    console.time("timer2");

    setTimeout(() => {
        console.log("B");
        console.timeEnd("timer2")
    }, 1000)
}, 500);


// output
// A
// timer: 1.004s
// B
// timer2: 1.001s

```


#javascript #시간측정  #time-measure