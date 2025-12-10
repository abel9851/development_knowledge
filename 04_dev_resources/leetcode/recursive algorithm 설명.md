- 반복 알고리즘은 재귀알고리즘으로 표현할 수 있다.
- 재귀 함수의 시작점에 존재하는, 재귀함수의 호출을 종료하는 조건인, base case가 필수다.
	- 시작하는 부분에 있지 않는다면, 정확히는 재귀함수를 내부에서 호출하는 코드 밑에 있다면 영원히 재귀함수는 멈추지 않는다.
	- base case가 없어도 마찬가지다.
```javascript
function recursive_func(n) {
	// 이 if조건이 base case다.
	if (n > 10) {
		return
	}
	print(i);
	recursive_func(n+1);
	return
}
```

- 재귀함수는 `피호출자 함수의 실행이 끝나기 전까지 호출자 함수들 위에 피호출자 함수가 쌓이는, LIFO(last In, First Out)구조다.`
```javascript
function recursive_func(n) {
  if (n > 3) {
   return
  };
  
  console.log(n);
  recursive_func(n+1);
  console.log(`end of call where i = ${n}`);
};

recursive_func(1);

/**
1
2
3
end of call where i = 3
end of call where i = 2
end of call where i = 1
*/

```

- 실행문맥(execution of context)이 스택에 저장된다.
	- 위의 코드를 예시로 들자면 로컬변수(현재값)n과 내부에서 다른 함수를 호출하고 그 함수의 호출이 끝난 뒤, 어느 줄을 실행할지를 적어논, 복귀주소를 적어놓은, 실행 문맥을 스택 프레임에 저장한다.
		- recursive_func(3)이 실행되고 있을 때, recursive_func(4)이 종료된 뒤, 즉, recursive_func(3)의 스택으로 돌아와서 3의 업무일지에서 다른 함수를 호출한뒤에 돌아와졌을 경우 어떤 줄을 실행할지를 적어논 것
	- i = 3으로 돌아가서, recursive_func(4)라인이 종료되고 그 다음 줄인 print가 실행되고 return이 실행되고, i=2인 함수로 돌아가서 recursive_func(3)라인이 종료되고... 이것을 반복한다.

#알고리즘 