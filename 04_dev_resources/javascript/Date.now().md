- `Date.now()` 을 실행하면 January 1, 1970 (UTC)로부터 경과된 시간을 출력한다.
	- 1762061334750
		- 경과된 시간을 초로 나타나기 때문에 알기 어렵다.
		- 이런식으로 시작시간으로부터 빼고 난 뒤, `Math.floor`를 사용하거나, 현재 시간을 알고 싶다면 `Date.now()` 를 인자로 갖는 Date 오브젝트를 생성해야한다.
			- Date는 javascript계의 Datetime(파이썬)이다.
```javascript
	  
const start = Date.now();

setTimeout(
    () => {
        const millis = Date.now() - start;
        console.log(`seconds elapsed =  ${Math.floor(millis / 1000)}`)
    },
    1000
);

// output: 1

console.log(new Date(start).toISOString());

// 2025-11-02T06:07:17.920Z

```
		  

#javascript #시간측정  #time-measure