- systematic way
	- 체계적인 방법
- we now have a systematic way to slide our window across the input while maintaining its validity
	- 우리는 지금 입력을 가로질러 윈도우를 미끄러뜨리는(이동시키는) 체계적인 방법이 있다.
- there are n-2 subarrays of length 3 and so on until there is only 1 subarray of length of n.
	- this means there are n(n+1)/2 subarrays. it is the partial sums of this series.
	- 
- 길이가 2인 부분배열을 만들기 위해서는 뒤에 최소 하나의 원소가 있어야한다.
- 마찬가지로 길이가 3인 부분배열을 만들기 위해서라면 뒤에서 최소 2개의 원소가 있어야한다.

- sliding window template
```plain

function fn(arr):
	left = 0
	
	for (int right = 0; right < arr.length; right++):
		Do some logic to "add" element at arr[right] to window
		
		while WINDOW_IS_INVALID:
			Do some logic to "remove" element at arr[left] from window
			left++
		
		Do some logic to update the answser

```

- amortized analysis
	- even though the worst case for an iteration inside the for loop is O(n) ,it averages out to  O(1) when you consider the entire runtime of the algorithm.
		- inside? in?
			- inside는 코드의 물리적인 범위(scope)나 블록내부
			- "in이 단순히 루프 안에 있다" 상태를 나타낸다면, inside는 그 루프가 감싸고 있는 그 본문 공간 내부라는 뜻
			- 물리적인가, 추상적인가의 차이

		- what is averages out to O(1)?
			- sliding window에서 총연산은 2n, for loop는 n번, 루프 한번당 평균연산은 2n / n = 2. 즉,  O(1)